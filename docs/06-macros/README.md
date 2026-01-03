# Раздел 6: Система макросов

## 6.1 Концепция макросов

### 6.1.1 Определение

```
┌─────────────────────────────────────────────────────────────────────────┐
│                           MACRO DEFINITION                              │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  МАКРОС — автоматизированная последовательность действий,              │
│  которая выполняется по триггеру или по запросу.                       │
│                                                                         │
│  АНАЛОГИЯ С EXCEL:                                                      │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  Excel Macro                    │  BIOS-I Macro                 │   │
│  ├─────────────────────────────────────────────────────────────────┤   │
│  │  Записывает действия           │  Определяет workflow          │   │
│  │  Воспроизводит по кнопке       │  Запускает по триггеру        │   │
│  │  Работает в одном файле        │  Работает между системами     │   │
│  │  VBA код                       │  YAML/JSON + скрипты          │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  КЛЮЧЕВЫЕ ОТЛИЧИЯ ОТ СЦЕНАРИЕВ Make.com/Zapier:                        │
│  ├── Версионируемые (git)                                              │
│  ├── Параметризуемые                                                   │
│  ├── Композируемые (макрос в макросе)                                  │
│  ├── Тестируемые                                                       │
│  └── Переиспользуемые как библиотеки                                   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 6.1.2 Типы макросов

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          MACRO TYPES                                    │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  1. TRIGGER-BASED (Реактивные)                                         │
│     ├── Webhook: HTTP запрос вызывает макрос                           │
│     ├── Schedule: Запуск по расписанию                                 │
│     ├── Event: Событие в системе (новый заказ, email, и т.д.)          │
│     └── Watch: Отслеживание изменений                                  │
│                                                                         │
│  2. ON-DEMAND (По запросу)                                             │
│     ├── Manual: Запуск пользователем                                   │
│     ├── API: Запуск через API                                          │
│     └── CLI: Запуск из командной строки                                │
│                                                                         │
│  3. COMPOSITE (Составные)                                              │
│     ├── Pipeline: Последовательность макросов                          │
│     ├── Parallel: Параллельное выполнение                              │
│     └── Conditional: Ветвление по условию                              │
│                                                                         │
│  4. UTILITY (Служебные)                                                │
│     ├── Transform: Преобразование данных                               │
│     ├── Validate: Проверка данных                                      │
│     └── Notify: Уведомления                                            │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 6.2 Спецификация макросов

### 6.2.1 YAML Schema

```yaml
# Полная схема определения макроса
macro:
  # === ИДЕНТИФИКАЦИЯ ===
  id: "macro-sales-lead-nurturing"
  slug: "sales-lead-nurturing"
  name: "Sales Lead Nurturing"
  version: "1.2.0"

  description: |
    Автоматизация nurturing лидов:
    - Обогащение данных
    - Сегментация
    - Персонализированные email-цепочки
    - Уведомления для продаж

  # === МЕТАДАННЫЕ ===
  metadata:
    author:
      id: "author-123"
      name: "BIOS-I Team"
    category: "sales"
    tags: ["leads", "nurturing", "email", "crm"]
    license: "MIT"
    created_at: "2025-01-01"
    updated_at: "2026-01-02"

  # === ТРИГГЕР ===
  trigger:
    type: "event"  # event | schedule | webhook | manual
    source: "hubspot"
    event: "contact.created"
    filter:
      lifecycle_stage: "lead"
      lead_score:
        $gte: 30

  # === ПАРАМЕТРЫ ===
  parameters:
    - name: "email_sequence"
      type: "select"
      label: "Email Sequence"
      options:
        - value: "standard"
          label: "Standard (5 emails)"
        - value: "aggressive"
          label: "Aggressive (10 emails)"
        - value: "light"
          label: "Light Touch (3 emails)"
      default: "standard"
      required: true

    - name: "notify_sales_threshold"
      type: "number"
      label: "Score to notify sales"
      default: 70
      min: 0
      max: 100

    - name: "slack_channel"
      type: "string"
      label: "Slack channel for notifications"
      default: "#sales-leads"

  # === ПЕРЕМЕННЫЕ ОКРУЖЕНИЯ ===
  env:
    - name: "HUBSPOT_API_KEY"
      required: true
      secret: true

    - name: "CLEARBIT_API_KEY"
      required: false
      secret: true

    - name: "SLACK_WEBHOOK_URL"
      required: true
      secret: true

  # === ШАГИ ===
  steps:
    # Шаг 1: Получение данных лида
    - id: "get_lead"
      name: "Get Lead Data"
      type: "connector"
      connector: "hubspot"
      operation: "getContact"
      params:
        contact_id: "{{ trigger.contact_id }}"
      output: "lead"

    # Шаг 2: Обогащение данных (опционально)
    - id: "enrich"
      name: "Enrich Lead Data"
      type: "connector"
      connector: "clearbit"
      operation: "enrichPerson"
      condition: "{{ env.CLEARBIT_API_KEY != null }}"
      params:
        email: "{{ lead.email }}"
      output: "enriched_data"
      on_error: "continue"  # continue | stop | retry

    # Шаг 3: Обновление контакта обогащёнными данными
    - id: "update_lead"
      name: "Update Lead with Enriched Data"
      type: "connector"
      connector: "hubspot"
      operation: "updateContact"
      condition: "{{ enriched_data != null }}"
      params:
        contact_id: "{{ lead.id }}"
        properties:
          company: "{{ enriched_data.company.name }}"
          job_title: "{{ enriched_data.employment.title }}"
          industry: "{{ enriched_data.company.industry }}"

    # Шаг 4: Сегментация
    - id: "segment"
      name: "Determine Segment"
      type: "script"
      language: "javascript"
      code: |
        const lead = inputs.lead;
        const enriched = inputs.enriched_data || {};

        let segment = 'standard';

        // Enterprise segment
        if (enriched.company?.employees > 1000) {
          segment = 'enterprise';
        }
        // SMB segment
        else if (enriched.company?.employees > 50) {
          segment = 'smb';
        }
        // Startup segment
        else if (lead.lead_score > 60) {
          segment = 'hot_startup';
        }

        return { segment };
      inputs:
        lead: "{{ lead }}"
        enriched_data: "{{ enriched_data }}"
      output: "segmentation"

    # Шаг 5: Добавление в email-последовательность
    - id: "add_to_sequence"
      name: "Add to Email Sequence"
      type: "connector"
      connector: "hubspot"
      operation: "addToSequence"
      params:
        contact_id: "{{ lead.id }}"
        sequence_id: "{{ sequences[segmentation.segment][params.email_sequence] }}"

    # Шаг 6: Уведомление в Slack (условное)
    - id: "notify_sales"
      name: "Notify Sales Team"
      type: "connector"
      connector: "slack"
      operation: "sendMessage"
      condition: "{{ lead.lead_score >= params.notify_sales_threshold }}"
      params:
        channel: "{{ params.slack_channel }}"
        text: |
          :fire: Hot Lead Alert!

          *{{ lead.firstname }} {{ lead.lastname }}*
          Company: {{ enriched_data.company.name || 'Unknown' }}
          Score: {{ lead.lead_score }}
          Segment: {{ segmentation.segment }}

          <{{ hubspot_url }}/{{ lead.id }}|View in HubSpot>

    # Шаг 7: Логирование
    - id: "log"
      name: "Log Execution"
      type: "internal"
      operation: "log"
      params:
        level: "info"
        message: "Lead nurturing completed"
        data:
          lead_id: "{{ lead.id }}"
          segment: "{{ segmentation.segment }}"
          notified_sales: "{{ lead.lead_score >= params.notify_sales_threshold }}"

  # === ОБРАБОТКА ОШИБОК ===
  error_handling:
    default: "stop"
    retry:
      max_attempts: 3
      backoff: "exponential"
      initial_delay_ms: 1000

    on_failure:
      - type: "notify"
        connector: "slack"
        params:
          channel: "#alerts"
          text: "Macro failed: {{ error.message }}"

  # === ТЕСТЫ ===
  tests:
    - name: "Standard lead processing"
      trigger_data:
        contact_id: "test-123"
      mocks:
        hubspot.getContact:
          return:
            id: "test-123"
            email: "test@example.com"
            lead_score: 50
      assertions:
        - "{{ segmentation.segment == 'standard' }}"
        - "{{ steps.notify_sales.skipped == true }}"

    - name: "High score lead triggers notification"
      trigger_data:
        contact_id: "test-456"
      mocks:
        hubspot.getContact:
          return:
            id: "test-456"
            email: "hot@startup.io"
            lead_score: 85
      assertions:
        - "{{ steps.notify_sales.executed == true }}"
```

### 6.2.2 Типы шагов

```yaml
step_types:
  # 1. Connector — вызов внешнего сервиса
  connector:
    connector: "string"      # ID коннектора
    operation: "string"      # Операция
    params: "object"         # Параметры
    output: "string"         # Имя выходной переменной
    timeout_ms: "number"     # Таймаут
    on_error: "enum"         # continue | stop | retry

  # 2. Script — выполнение кода
  script:
    language: "enum"         # javascript | python
    code: "string"           # Код
    inputs: "object"         # Входные данные
    output: "string"         # Имя выходной переменной
    timeout_ms: "number"

  # 3. HTTP — прямой HTTP запрос
  http:
    method: "enum"           # GET | POST | PUT | DELETE
    url: "string"
    headers: "object"
    body: "any"
    output: "string"

  # 4. Condition — ветвление
  condition:
    if: "string"             # Условие
    then: "array"            # Шаги если true
    else: "array"            # Шаги если false

  # 5. Loop — цикл
  loop:
    items: "string"          # Массив для итерации
    item_var: "string"       # Имя переменной элемента
    steps: "array"           # Шаги внутри цикла
    parallel: "boolean"      # Параллельное выполнение
    max_concurrency: "number"

  # 6. Parallel — параллельное выполнение
  parallel:
    branches: "array"        # Параллельные ветки
    wait: "enum"             # all | any | none
    timeout_ms: "number"

  # 7. Wait — ожидание
  wait:
    duration_ms: "number"    # Длительность
    until: "string"          # Условие
    poll_interval_ms: "number"

  # 8. Macro — вызов другого макроса
  macro:
    macro_id: "string"       # ID макроса
    params: "object"         # Параметры
    output: "string"

  # 9. Transform — преобразование данных
  transform:
    input: "string"
    operations: "array"
    output: "string"

  # 10. Internal — внутренние операции
  internal:
    operation: "enum"        # log | set_variable | validate
    params: "object"
```

---

## 6.3 Движок выполнения макросов

### 6.3.1 Архитектура

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        MACRO ENGINE ARCHITECTURE                        │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      TRIGGER MANAGER                             │   │
│  │                                                                  │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐        │   │
│  │  │ Webhook  │  │ Schedule │  │  Event   │  │  Manual  │        │   │
│  │  │ Listener │  │  Runner  │  │ Listener │  │ Trigger  │        │   │
│  │  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘        │   │
│  │       └─────────────┼─────────────┼─────────────┘               │   │
│  │                     v                                            │   │
│  │              ┌──────────────┐                                    │   │
│  │              │ Trigger Bus  │                                    │   │
│  │              └──────┬───────┘                                    │   │
│  └─────────────────────┼────────────────────────────────────────────┘   │
│                        │                                                │
│                        v                                                │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      EXECUTION ENGINE                            │   │
│  │                                                                  │   │
│  │  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐       │   │
│  │  │   Context    │    │     Step     │    │   Output     │       │   │
│  │  │   Builder    │───>│   Executor   │───>│   Collector  │       │   │
│  │  └──────────────┘    └──────┬───────┘    └──────────────┘       │   │
│  │                             │                                    │   │
│  │                 ┌───────────┼───────────┐                       │   │
│  │                 v           v           v                        │   │
│  │  ┌──────────────────────────────────────────────────────────┐   │   │
│  │  │                   STEP HANDLERS                           │   │   │
│  │  │                                                           │   │   │
│  │  │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐         │   │   │
│  │  │  │Connector│ │ Script  │ │  HTTP   │ │  Loop   │  ...    │   │   │
│  │  │  │ Handler │ │ Handler │ │ Handler │ │ Handler │         │   │   │
│  │  │  └─────────┘ └─────────┘ └─────────┘ └─────────┘         │   │   │
│  │  │                                                           │   │   │
│  │  └──────────────────────────────────────────────────────────┘   │   │
│  │                                                                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                        │                                                │
│                        v                                                │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      SUPPORT SERVICES                            │   │
│  │                                                                  │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐        │   │
│  │  │   State  │  │   Log    │  │  Metric  │  │  Error   │        │   │
│  │  │  Manager │  │ Collector│  │ Recorder │  │ Handler  │        │   │
│  │  └──────────┘  └──────────┘  └──────────┘  └──────────┘        │   │
│  │                                                                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 6.3.2 Реализация движка

```typescript
class MacroEngine {
  private stepHandlers: Map<string, StepHandler>;
  private connectorManager: ConnectorManager;
  private stateManager: StateManager;

  async execute(
    macro: Macro,
    triggerData: unknown,
    params: Record<string, unknown>
  ): Promise<ExecutionResult> {

    // Создание контекста выполнения
    const context = new ExecutionContext({
      macroId: macro.id,
      executionId: generateId(),
      startedAt: new Date(),
      triggerData,
      params,
      env: this.loadEnv(macro.env),
      variables: {},
      logs: []
    });

    try {
      // Валидация параметров
      this.validateParams(macro.parameters, params);

      // Выполнение шагов
      for (const step of macro.steps) {
        const shouldExecute = await this.evaluateCondition(
          step.condition,
          context
        );

        if (!shouldExecute) {
          context.log(`Step ${step.id} skipped: condition not met`);
          continue;
        }

        await this.executeStep(step, context);

        if (context.status === 'failed') {
          break;
        }
      }

      return this.createResult(context);

    } catch (error) {
      return this.handleError(error, context, macro.error_handling);
    }
  }

  private async executeStep(
    step: Step,
    context: ExecutionContext
  ): Promise<void> {

    context.log(`Executing step: ${step.name}`);
    const startTime = Date.now();

    const handler = this.stepHandlers.get(step.type);
    if (!handler) {
      throw new Error(`Unknown step type: ${step.type}`);
    }

    try {
      // Разрешение переменных в параметрах
      const resolvedParams = this.resolveTemplates(step.params, context);

      // Выполнение шага
      const result = await handler.execute(resolvedParams, context);

      // Сохранение результата
      if (step.output) {
        context.variables[step.output] = result;
      }

      context.logStepSuccess(step.id, Date.now() - startTime);

    } catch (error) {
      await this.handleStepError(error, step, context);
    }
  }

  private resolveTemplates(
    obj: unknown,
    context: ExecutionContext
  ): unknown {

    if (typeof obj === 'string') {
      return this.interpolate(obj, context);
    }

    if (Array.isArray(obj)) {
      return obj.map(item => this.resolveTemplates(item, context));
    }

    if (typeof obj === 'object' && obj !== null) {
      const resolved: Record<string, unknown> = {};
      for (const [key, value] of Object.entries(obj)) {
        resolved[key] = this.resolveTemplates(value, context);
      }
      return resolved;
    }

    return obj;
  }

  private interpolate(
    template: string,
    context: ExecutionContext
  ): string {

    // {{ expression }} синтаксис
    return template.replace(
      /\{\{\s*(.+?)\s*\}\}/g,
      (match, expression) => {
        try {
          return this.evaluateExpression(expression, context);
        } catch {
          return match;
        }
      }
    );
  }

  private evaluateExpression(
    expression: string,
    context: ExecutionContext
  ): string {

    // Создаём безопасный контекст для выполнения
    const sandbox = {
      trigger: context.triggerData,
      params: context.params,
      env: context.env,
      ...context.variables
    };

    // Используем безопасный eval
    return safeEval(expression, sandbox);
  }
}

// Step Handlers

class ConnectorStepHandler implements StepHandler {
  constructor(private connectorManager: ConnectorManager) {}

  async execute(
    params: ConnectorStepParams,
    context: ExecutionContext
  ): Promise<unknown> {

    const connector = this.connectorManager.get(params.connector);
    const connection = await this.connectorManager.getConnection(
      params.connector,
      context.env
    );

    return connector.execute(
      connection,
      params.operation,
      params.params
    );
  }
}

class ScriptStepHandler implements StepHandler {
  async execute(
    params: ScriptStepParams,
    context: ExecutionContext
  ): Promise<unknown> {

    const inputs = params.inputs || {};

    if (params.language === 'javascript') {
      return this.executeJS(params.code, inputs);
    } else if (params.language === 'python') {
      return this.executePython(params.code, inputs);
    }

    throw new Error(`Unsupported language: ${params.language}`);
  }

  private async executeJS(
    code: string,
    inputs: Record<string, unknown>
  ): Promise<unknown> {

    // Изолированное выполнение
    const vm = new VM({
      timeout: 30000,
      sandbox: { inputs }
    });

    const wrappedCode = `
      (function() {
        ${code}
      })()
    `;

    return vm.run(wrappedCode);
  }
}

class LoopStepHandler implements StepHandler {
  constructor(private engine: MacroEngine) {}

  async execute(
    params: LoopStepParams,
    context: ExecutionContext
  ): Promise<unknown[]> {

    const items = context.variables[params.items] as unknown[];
    const results: unknown[] = [];

    if (params.parallel) {
      // Параллельное выполнение
      const chunks = this.chunk(items, params.max_concurrency || 10);

      for (const chunk of chunks) {
        const chunkResults = await Promise.all(
          chunk.map(async (item, index) => {
            const loopContext = context.createChild({
              [params.item_var]: item,
              $index: index
            });

            for (const step of params.steps) {
              await this.engine.executeStep(step, loopContext);
            }

            return loopContext.getResult();
          })
        );

        results.push(...chunkResults);
      }
    } else {
      // Последовательное выполнение
      for (let i = 0; i < items.length; i++) {
        const loopContext = context.createChild({
          [params.item_var]: items[i],
          $index: i
        });

        for (const step of params.steps) {
          await this.engine.executeStep(step, loopContext);
        }

        results.push(loopContext.getResult());
      }
    }

    return results;
  }
}
```

---

## 6.4 Библиотека готовых макросов

### 6.4.1 Sales & Marketing

```yaml
# Lead Scoring Macro
- id: "macro-lead-scoring"
  name: "Automated Lead Scoring"
  category: "sales"
  trigger: { type: "event", source: "hubspot", event: "contact.updated" }
  steps:
    - type: "connector"
      connector: "hubspot"
      operation: "getContact"
    - type: "script"
      code: |
        const score = 0;
        // Email engagement
        if (lead.email_opens > 5) score += 20;
        // Website visits
        if (lead.page_views > 10) score += 15;
        // Company size
        if (lead.company_size > 100) score += 25;
        return { score };
    - type: "connector"
      connector: "hubspot"
      operation: "updateContact"
      params: { lead_score: "{{ score }}" }

# Abandoned Cart Recovery
- id: "macro-abandoned-cart"
  name: "Abandoned Cart Recovery"
  category: "ecommerce"
  trigger:
    type: "schedule"
    cron: "0 * * * *"  # Every hour
  steps:
    - type: "connector"
      connector: "woocommerce"
      operation: "getAbandonedCarts"
      params: { abandoned_hours: 2 }
    - type: "loop"
      items: "carts"
      steps:
        - type: "connector"
          connector: "mailchimp"
          operation: "sendEmail"
          params:
            template: "abandoned_cart"
            to: "{{ item.email }}"
            data: "{{ item }}"

# Review Request
- id: "macro-review-request"
  name: "Post-Purchase Review Request"
  category: "ecommerce"
  trigger:
    type: "event"
    source: "woocommerce"
    event: "order.completed"
  steps:
    - type: "wait"
      duration_ms: 604800000  # 7 days
    - type: "connector"
      connector: "mailchimp"
      operation: "sendEmail"
      params:
        template: "review_request"
```

### 6.4.2 DevOps

```yaml
# Deployment Notification
- id: "macro-deploy-notify"
  name: "Deployment Notification"
  category: "devops"
  trigger:
    type: "webhook"
    path: "/deploy"
  steps:
    - type: "connector"
      connector: "slack"
      operation: "sendMessage"
      params:
        channel: "#deployments"
        text: |
          :rocket: Deployment Started
          Branch: {{ trigger.branch }}
          Author: {{ trigger.author }}
    - type: "wait"
      until: "{{ deployment.status != 'pending' }}"
      poll_interval_ms: 10000
    - type: "condition"
      if: "{{ deployment.status == 'success' }}"
      then:
        - type: "connector"
          connector: "slack"
          params: { text: ":white_check_mark: Deployment successful" }
      else:
        - type: "connector"
          connector: "slack"
          params: { text: ":x: Deployment failed" }
        - type: "connector"
          connector: "pagerduty"
          operation: "createIncident"

# Auto-scaling
- id: "macro-autoscale"
  name: "Auto-scale on High Load"
  category: "devops"
  trigger:
    type: "event"
    source: "prometheus"
    event: "alert.firing"
    filter: { alertname: "HighCPUUsage" }
  steps:
    - type: "connector"
      connector: "kubernetes"
      operation: "scaleDeployment"
      params:
        deployment: "{{ trigger.labels.deployment }}"
        replicas: "{{ trigger.labels.current_replicas + 2 }}"
    - type: "connector"
      connector: "slack"
      params:
        text: "Scaled {{ trigger.labels.deployment }} to {{ replicas }} replicas"
```

### 6.4.3 Support

```yaml
# Ticket Routing
- id: "macro-ticket-routing"
  name: "Intelligent Ticket Routing"
  category: "support"
  trigger:
    type: "event"
    source: "zendesk"
    event: "ticket.created"
  steps:
    - type: "connector"
      connector: "openai"
      operation: "classify"
      params:
        text: "{{ trigger.ticket.subject }} {{ trigger.ticket.description }}"
        categories: ["billing", "technical", "sales", "general"]
      output: "classification"
    - type: "connector"
      connector: "zendesk"
      operation: "updateTicket"
      params:
        ticket_id: "{{ trigger.ticket.id }}"
        group: "{{ classification.category }}"
        priority: "{{ classification.urgency }}"

# SLA Breach Warning
- id: "macro-sla-warning"
  name: "SLA Breach Warning"
  category: "support"
  trigger:
    type: "schedule"
    cron: "*/15 * * * *"
  steps:
    - type: "connector"
      connector: "zendesk"
      operation: "getTickets"
      params:
        status: "open"
        sla_breach_in: 30  # minutes
    - type: "loop"
      items: "tickets"
      steps:
        - type: "connector"
          connector: "slack"
          params:
            channel: "#support-urgent"
            text: ":warning: SLA breach in 30 min: {{ item.id }}"
```

---

## 6.5 Тестирование и отладка

### 6.5.1 Тестовый фреймворк

```typescript
class MacroTestRunner {
  async runTests(macro: Macro): Promise<TestResults> {
    const results: TestResult[] = [];

    for (const test of macro.tests) {
      const result = await this.runTest(macro, test);
      results.push(result);
    }

    return {
      passed: results.filter(r => r.passed).length,
      failed: results.filter(r => !r.passed).length,
      results
    };
  }

  private async runTest(
    macro: Macro,
    test: MacroTest
  ): Promise<TestResult> {

    // Создаём mock-контекст
    const mockContext = new MockExecutionContext({
      triggerData: test.trigger_data,
      mocks: test.mocks
    });

    // Создаём mock-движок
    const mockEngine = new MockMacroEngine(mockContext);

    try {
      // Выполняем макрос
      const result = await mockEngine.execute(macro, test.trigger_data, {});

      // Проверяем assertions
      const assertionResults = test.assertions.map(assertion => {
        const passed = mockContext.evaluate(assertion);
        return { assertion, passed };
      });

      const allPassed = assertionResults.every(a => a.passed);

      return {
        testName: test.name,
        passed: allPassed,
        assertions: assertionResults,
        executionResult: result
      };

    } catch (error) {
      return {
        testName: test.name,
        passed: false,
        error: error.message
      };
    }
  }
}

// Mock Connector для тестирования
class MockConnectorManager {
  private mocks: Map<string, Map<string, unknown>>;

  constructor(mocks: Record<string, Record<string, unknown>>) {
    this.mocks = new Map();
    for (const [connector, operations] of Object.entries(mocks)) {
      const opMap = new Map(Object.entries(operations));
      this.mocks.set(connector, opMap);
    }
  }

  async execute(
    connector: string,
    operation: string,
    params: unknown
  ): Promise<unknown> {

    const mockKey = `${connector}.${operation}`;
    const connectorMocks = this.mocks.get(connector);

    if (connectorMocks?.has(operation)) {
      const mock = connectorMocks.get(operation);
      if (typeof mock === 'function') {
        return mock(params);
      }
      return mock.return;
    }

    throw new Error(`No mock defined for ${mockKey}`);
  }
}
```

### 6.5.2 Отладчик макросов

```typescript
class MacroDebugger {
  private breakpoints: Set<string>;
  private watchExpressions: string[];

  async debug(
    macro: Macro,
    triggerData: unknown
  ): Promise<void> {

    const context = new DebugContext(triggerData);

    for (const step of macro.steps) {
      // Проверка breakpoint
      if (this.breakpoints.has(step.id)) {
        await this.pause(step, context);
      }

      // Выполнение шага
      console.log(`[DEBUG] Executing: ${step.name}`);
      console.log(`[DEBUG] Context:`, this.formatContext(context));

      await this.executeStepWithLogging(step, context);

      // Вывод watch expressions
      for (const expr of this.watchExpressions) {
        console.log(`[WATCH] ${expr} =`, context.evaluate(expr));
      }
    }
  }

  private async pause(
    step: Step,
    context: DebugContext
  ): Promise<void> {

    console.log(`\n[BREAKPOINT] Paused at step: ${step.name}`);
    console.log(`[BREAKPOINT] Step config:`, step);
    console.log(`[BREAKPOINT] Current context:`, context.getSnapshot());

    // Интерактивный режим
    const rl = readline.createInterface({
      input: process.stdin,
      output: process.stdout
    });

    return new Promise((resolve) => {
      const prompt = () => {
        rl.question('debug> ', (input) => {
          if (input === 'continue' || input === 'c') {
            rl.close();
            resolve();
          } else if (input.startsWith('eval ')) {
            const expr = input.slice(5);
            console.log(context.evaluate(expr));
            prompt();
          } else if (input === 'context') {
            console.log(context.getSnapshot());
            prompt();
          } else if (input === 'step') {
            // Step into next
            rl.close();
            resolve();
          } else {
            console.log('Commands: continue (c), eval <expr>, context, step');
            prompt();
          }
        });
      };
      prompt();
    });
  }
}
```

---

## 6.6 Выводы раздела

### Ключевые возможности системы макросов

1. **Декларативное определение** — макросы описываются в YAML, понятном не-программистам

2. **Параметризация** — макросы можно настраивать без изменения кода

3. **Композиция** — макросы могут вызывать другие макросы

4. **Тестируемость** — встроенная система тестов с моками

5. **Отладка** — пошаговое выполнение с breakpoints

6. **Версионирование** — макросы хранятся в git

### Отличия от существующих решений

| Функция | Zapier | Make.com | BIOS-I Macros |
|---------|--------|----------|---------------|
| Версионирование | Нет | Нет | Git |
| Параметры | Ограничено | Ограничено | Полные |
| Тесты | Нет | Нет | Встроенные |
| Композиция | Нет | Частично | Полная |
| Код | Нет | Частично | JS/Python |
| Отладка | Нет | Базовая | Полная |

→ [Раздел 7: ИИ-компоненты](../07-ai-components/README.md)
