# Раздел 5: Система кластеров

## 5.1 Концепция кластеров

### 5.1.1 Определение

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         CLUSTER DEFINITION                              │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  КЛАСТЕР — это проверенная комбинация компонентов, которые:            │
│                                                                         │
│  ✓ Решают конкретную бизнес-задачу                                     │
│  ✓ Протестированы на совместимость                                     │
│  ✓ Имеют готовую конфигурацию                                          │
│  ✓ Разворачиваются одной командой                                      │
│  ✓ Документированы и поддерживаются                                    │
│                                                                         │
│  АНАЛОГИИ:                                                              │
│  ├── Рецепт = список ингредиентов + инструкция                         │
│  ├── LEGO набор = детали + схема сборки                                │
│  ├── Docker Compose = сервисы + конфигурация                           │
│  └── Terraform module = ресурсы + переменные                           │
│                                                                         │
│  ОТЛИЧИЕ ОТ ПРОСТОГО СПИСКА:                                           │
│  ├── Проверенная совместимость                                         │
│  ├── Правильный порядок установки                                      │
│  ├── Предварительная конфигурация                                      │
│  └── Интеграция между компонентами                                     │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 5.1.2 Иерархия кластеров

```
УРОВЕНЬ 4: ENTERPRISE SOLUTIONS
├── Полные бизнес-системы
├── Пример: "Digital Transformation Suite"
└── Компонентов: 50-100+

УРОВЕНЬ 3: DOMAIN SOLUTIONS
├── Решения для отрасли/домена
├── Пример: "E-commerce Complete", "SaaS Starter"
└── Компонентов: 15-50

УРОВЕНЬ 2: FUNCTIONAL CLUSTERS
├── Функциональные блоки
├── Пример: "Payment Processing", "Email Marketing"
└── Компонентов: 5-15

УРОВЕНЬ 1: MICRO-CLUSTERS
├── Минимальные связки
├── Пример: "WooCommerce + Stripe"
└── Компонентов: 2-5
```

### 5.1.3 Таксономия кластеров

```yaml
taxonomy:
  ecommerce:
    - basic_store
    - marketplace
    - subscription_commerce
    - b2b_commerce
    - dropshipping
    - digital_products

  marketing:
    - email_marketing
    - social_media
    - content_marketing
    - seo_suite
    - analytics
    - advertising

  crm_sales:
    - basic_crm
    - sales_automation
    - lead_management
    - customer_success
    - pipeline_management

  productivity:
    - project_management
    - document_collaboration
    - team_communication
    - knowledge_base
    - time_tracking

  devops:
    - ci_cd_pipeline
    - monitoring_observability
    - infrastructure_as_code
    - container_orchestration
    - security_scanning

  support:
    - helpdesk
    - live_chat
    - knowledge_base
    - ticketing
    - omnichannel

  finance:
    - invoicing
    - accounting
    - expense_management
    - payroll
    - payment_processing

  hr:
    - recruiting
    - onboarding
    - performance_management
    - employee_engagement
    - learning_management
```

---

## 5.2 Библиотека кластеров

### 5.2.1 E-commerce кластеры

#### Кластер: WordPress E-commerce Starter

```yaml
id: "cluster-wp-ecommerce-starter"
name: "WordPress E-commerce Starter"
category: "ecommerce"
subcategory: "basic_store"
level: 2

description: |
  Минимальный, но полностью функциональный интернет-магазин на WordPress.
  Включает всё необходимое для старта продаж: каталог, корзину, оплату, SEO.

target_audience:
  - Малый бизнес
  - Стартапы
  - Первый интернет-магазин

components:
  - id: "wordpress/woocommerce"
    role: core
    required: true
    description: "Ядро интернет-магазина"

  - id: "wordpress/woocommerce-stripe"
    role: integration
    required: true
    description: "Приём платежей картами"
    alternatives:
      - "wordpress/woocommerce-paypal"
      - "wordpress/woocommerce-square"

  - id: "wordpress/yoast-seo"
    role: enhancement
    required: true
    description: "SEO оптимизация"

  - id: "wordpress/wordfence"
    role: utility
    required: true
    description: "Безопасность"

  - id: "wordpress/updraftplus"
    role: utility
    required: true
    description: "Бэкапы"

estimated_setup_time: "30 minutes"
monthly_cost: "$0-50"
difficulty: "beginner"

metrics:
  installs: 5678
  rating: 4.7
  success_rate: 0.94
```

#### Кластер: WooCommerce Pro Stack

```yaml
id: "cluster-woocommerce-pro"
name: "WooCommerce Pro Stack"
category: "ecommerce"
subcategory: "full_featured"
level: 3

description: |
  Профессиональный интернет-магазин со всеми функциями:
  платежи, доставка, email-маркетинг, аналитика, оптимизация.

components:
  # Core
  - id: "wordpress/woocommerce"
    role: core
    required: true

  # Payments
  - id: "wordpress/woocommerce-stripe"
    role: integration
    required: true

  - id: "wordpress/woocommerce-paypal"
    role: integration
    required: false

  # Shipping
  - id: "wordpress/woocommerce-shipping"
    role: integration
    required: true

  - id: "wordpress/flexible-shipping"
    role: enhancement
    required: false

  # SEO & Marketing
  - id: "wordpress/yoast-seo"
    role: enhancement
    required: true

  - id: "wordpress/yoast-woocommerce-seo"
    role: integration
    required: true

  - id: "wordpress/mailchimp-for-woocommerce"
    role: integration
    required: false

  # Performance
  - id: "wordpress/wp-rocket"
    role: enhancement
    required: true
    alternatives:
      - "wordpress/litespeed-cache"

  - id: "wordpress/smush"
    role: enhancement
    required: false

  # Security
  - id: "wordpress/wordfence"
    role: utility
    required: true

  # Backup
  - id: "wordpress/updraftplus-premium"
    role: utility
    required: true

  # Analytics
  - id: "wordpress/monsterinsights-pro"
    role: monitoring
    required: false

  # Customer Experience
  - id: "wordpress/woocommerce-product-filter"
    role: enhancement
    required: false

  - id: "wordpress/woocommerce-wishlist"
    role: enhancement
    required: false

internal_connections:
  - source: "woocommerce"
    target: "stripe"
    type: "payment_gateway"

  - source: "woocommerce"
    target: "mailchimp"
    type: "customer_sync"

  - source: "woocommerce"
    target: "yoast-woocommerce-seo"
    type: "product_seo"

estimated_setup_time: "2-4 hours"
monthly_cost: "$100-300"
difficulty: "intermediate"
```

### 5.2.2 DevOps кластеры

#### Кластер: Node.js CI/CD Pipeline

```yaml
id: "cluster-nodejs-cicd"
name: "Node.js CI/CD Pipeline"
category: "devops"
subcategory: "ci_cd_pipeline"
level: 2

description: |
  Полный CI/CD пайплайн для Node.js проектов:
  линтинг, тестирование, сборка, деплой.

target_platforms:
  - github

components:
  - id: "npm/eslint"
    role: utility
    required: true
    config:
      extends: ["eslint:recommended"]

  - id: "npm/prettier"
    role: utility
    required: true

  - id: "npm/jest"
    role: utility
    required: true
    alternatives:
      - "npm/vitest"
      - "npm/mocha"

  - id: "npm/husky"
    role: utility
    required: true

  - id: "npm/lint-staged"
    role: utility
    required: true

  - id: "github/actions"
    role: core
    required: true
    config:
      workflow: |
        name: CI/CD
        on: [push, pull_request]
        jobs:
          test:
            runs-on: ubuntu-latest
            steps:
              - uses: actions/checkout@v4
              - uses: actions/setup-node@v4
              - run: npm ci
              - run: npm run lint
              - run: npm test
              - run: npm run build

  - id: "github/dependabot"
    role: utility
    required: false

  - id: "github/codecov"
    role: monitoring
    required: false

scripts:
  install: |
    npm install eslint prettier jest husky lint-staged --save-dev
    npx husky install
    npx husky add .husky/pre-commit "npx lint-staged"

  configure: |
    # ESLint config
    cat > .eslintrc.json << 'EOF'
    {
      "extends": ["eslint:recommended"],
      "env": { "node": true, "es2021": true }
    }
    EOF

    # Prettier config
    cat > .prettierrc << 'EOF'
    {
      "semi": true,
      "singleQuote": true
    }
    EOF

    # lint-staged config
    cat > .lintstagedrc << 'EOF'
    {
      "*.js": ["eslint --fix", "prettier --write"]
    }
    EOF
```

#### Кластер: Kubernetes Monitoring Stack

```yaml
id: "cluster-k8s-monitoring"
name: "Kubernetes Monitoring Stack"
category: "devops"
subcategory: "monitoring_observability"
level: 3

description: |
  Полный стек мониторинга для Kubernetes:
  метрики, логи, трейсы, алерты, дашборды.

components:
  # Metrics
  - id: "helm/prometheus"
    role: core
    required: true
    config:
      retention: "15d"
      resources:
        memory: "2Gi"

  - id: "helm/prometheus-node-exporter"
    role: integration
    required: true

  # Visualization
  - id: "helm/grafana"
    role: core
    required: true
    config:
      dashboards:
        - kubernetes-cluster
        - node-exporter
        - nginx-ingress

  # Logging
  - id: "helm/loki"
    role: core
    required: true
    config:
      retention: "7d"

  - id: "helm/promtail"
    role: integration
    required: true

  # Tracing
  - id: "helm/jaeger"
    role: enhancement
    required: false
    alternatives:
      - "helm/tempo"

  # Alerting
  - id: "helm/alertmanager"
    role: utility
    required: true
    config:
      receivers:
        - slack
        - email

  # Service Mesh Observability
  - id: "helm/kiali"
    role: enhancement
    required: false

internal_connections:
  - source: "prometheus"
    target: "grafana"
    type: "datasource"

  - source: "loki"
    target: "grafana"
    type: "datasource"

  - source: "jaeger"
    target: "grafana"
    type: "datasource"

  - source: "prometheus"
    target: "alertmanager"
    type: "alerts"

scripts:
  install: |
    # Add Helm repos
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update

    # Install Prometheus Stack
    helm install prometheus prometheus-community/kube-prometheus-stack \
      --namespace monitoring --create-namespace \
      --values prometheus-values.yaml

    # Install Loki Stack
    helm install loki grafana/loki-stack \
      --namespace monitoring \
      --values loki-values.yaml

  health_check: |
    kubectl get pods -n monitoring
    kubectl port-forward svc/prometheus-grafana 3000:80 -n monitoring
```

### 5.2.3 Marketing Automation кластеры

#### Кластер: Make.com Sales Automation

```yaml
id: "cluster-makecom-sales"
name: "Sales Automation Pipeline"
category: "crm_sales"
subcategory: "sales_automation"
level: 2
platform: "makecom"

description: |
  Автоматизация цикла продаж:
  от лида до закрытия сделки.

components:
  - id: "makecom/hubspot"
    role: core
    required: true
    modules:
      - "Watch Deals"
      - "Create Contact"
      - "Update Deal"

  - id: "makecom/linkedin-sales-navigator"
    role: integration
    required: false

  - id: "makecom/calendly"
    role: integration
    required: true
    modules:
      - "Watch Events"
      - "Create Event"

  - id: "makecom/zoom"
    role: integration
    required: true
    modules:
      - "Create Meeting"

  - id: "makecom/gmail"
    role: integration
    required: true
    modules:
      - "Send Email"
      - "Watch Emails"

  - id: "makecom/slack"
    role: utility
    required: true
    modules:
      - "Send Message"

  - id: "makecom/pandadoc"
    role: integration
    required: false
    modules:
      - "Create Document"
      - "Send Document"

scenarios:
  - name: "New Lead Processing"
    trigger: "HubSpot: Watch New Contacts"
    steps:
      - "Filter: Score > 50"
      - "LinkedIn: Enrich Data"
      - "HubSpot: Update Contact"
      - "Gmail: Send Welcome Email"
      - "Slack: Notify Sales Team"

  - name: "Meeting Scheduled"
    trigger: "Calendly: Watch Events"
    steps:
      - "HubSpot: Update Deal Stage"
      - "Zoom: Create Meeting"
      - "Gmail: Send Confirmation"

  - name: "Deal Won"
    trigger: "HubSpot: Watch Deal Updates"
    steps:
      - "Filter: Stage = Closed Won"
      - "PandaDoc: Create Contract"
      - "Gmail: Send Contract"
      - "Slack: Celebrate"
```

---

## 5.3 Алгоритмы создания кластеров

### 5.3.1 Автоматическое обнаружение кластеров

```python
from typing import List, Dict, Set, Tuple
from collections import defaultdict
import networkx as nx

class ClusterDiscovery:
    """
    Автоматическое обнаружение потенциальных кластеров
    на основе анализа co-installation patterns.
    """

    def __init__(self, graph_db, min_support: float = 0.01):
        self.graph_db = graph_db
        self.min_support = min_support  # Минимальная частота

    async def discover_clusters(
        self,
        ecosystem: str,
        min_size: int = 3,
        max_size: int = 10
    ) -> List[Dict]:
        """
        Обнаружение кластеров с использованием
        алгоритма Apriori для частых itemsets.
        """

        # 1. Получить данные о совместных установках
        installations = await self._get_installation_data(ecosystem)

        # 2. Найти частые itemsets (Apriori)
        frequent_sets = self._find_frequent_itemsets(
            installations,
            min_size,
            max_size
        )

        # 3. Оценить качество потенциальных кластеров
        candidates = []
        for itemset, support in frequent_sets:
            score = await self._evaluate_cluster_candidate(itemset)
            if score > 0.5:
                candidates.append({
                    'components': list(itemset),
                    'support': support,
                    'quality_score': score,
                    'suggested_name': await self._generate_name(itemset)
                })

        # 4. Удалить подмножества
        filtered = self._remove_subsets(candidates)

        return sorted(filtered, key=lambda x: x['quality_score'], reverse=True)

    def _find_frequent_itemsets(
        self,
        transactions: List[Set[str]],
        min_size: int,
        max_size: int
    ) -> List[Tuple[frozenset, float]]:
        """Алгоритм Apriori для поиска частых наборов"""

        n_transactions = len(transactions)
        min_count = int(self.min_support * n_transactions)

        # Подсчёт одиночных элементов
        item_counts = defaultdict(int)
        for t in transactions:
            for item in t:
                item_counts[item] += 1

        # Фильтрация по min_support
        frequent_1 = {
            frozenset([item]): count / n_transactions
            for item, count in item_counts.items()
            if count >= min_count
        }

        all_frequent = dict(frequent_1)
        current_frequent = frequent_1

        # Расширение до max_size
        for k in range(2, max_size + 1):
            # Генерация кандидатов размера k
            candidates = self._generate_candidates(
                list(current_frequent.keys()), k
            )

            # Подсчёт поддержки
            candidate_counts = defaultdict(int)
            for t in transactions:
                t_set = frozenset(t)
                for candidate in candidates:
                    if candidate.issubset(t_set):
                        candidate_counts[candidate] += 1

            # Фильтрация
            current_frequent = {
                itemset: count / n_transactions
                for itemset, count in candidate_counts.items()
                if count >= min_count
            }

            if not current_frequent:
                break

            all_frequent.update(current_frequent)

        # Фильтрация по размеру
        result = [
            (itemset, support)
            for itemset, support in all_frequent.items()
            if min_size <= len(itemset) <= max_size
        ]

        return result

    async def _evaluate_cluster_candidate(
        self,
        components: frozenset
    ) -> float:
        """
        Оценка качества потенциального кластера.

        Критерии:
        - Совместимость компонентов
        - Покрытие use case
        - Разнообразие ролей
        - Отсутствие конфликтов
        """

        component_list = list(components)

        # 1. Проверка совместимости
        compatibility_score = await self._check_compatibility(component_list)

        # 2. Проверка покрытия ролей
        roles = await self._get_component_roles(component_list)
        role_coverage = len(set(roles)) / max(len(roles), 1)

        # 3. Проверка на конфликты
        conflicts = await self._check_conflicts(component_list)
        conflict_penalty = len(conflicts) * 0.2

        # 4. Проверка health компонентов
        avg_health = await self._get_avg_health(component_list)

        score = (
            compatibility_score * 0.4 +
            role_coverage * 0.2 +
            (1 - conflict_penalty) * 0.2 +
            (avg_health / 100) * 0.2
        )

        return max(0, min(1, score))

    async def _generate_name(self, components: frozenset) -> str:
        """Генерация названия кластера на основе компонентов"""

        # Получить категории компонентов
        categories = await self._get_categories(list(components))

        # Найти доминирующую категорию
        category_counts = defaultdict(int)
        for cat in categories:
            category_counts[cat] += 1

        main_category = max(category_counts, key=category_counts.get)

        # Найти "core" компонент
        core = await self._find_core_component(list(components))

        return f"{core} {main_category.title()} Stack"


class ClusterOptimizer:
    """Оптимизация существующих кластеров"""

    def __init__(self, catalog_db, graph_db):
        self.catalog_db = catalog_db
        self.graph_db = graph_db

    async def optimize_cluster(
        self,
        cluster: Dict
    ) -> Dict:
        """
        Оптимизация кластера:
        - Удаление ненужных компонентов
        - Добавление недостающих
        - Замена на лучшие альтернативы
        - Оптимизация порядка установки
        """

        optimized = cluster.copy()

        # 1. Анализ зависимостей
        deps = await self._analyze_dependencies(cluster['components'])

        # 2. Удаление избыточных
        optimized['components'] = self._remove_redundant(
            cluster['components'], deps
        )

        # 3. Добавление недостающих зависимостей
        missing = self._find_missing_dependencies(
            optimized['components'], deps
        )
        optimized['components'].extend(missing)

        # 4. Предложение альтернатив
        alternatives = await self._find_better_alternatives(
            optimized['components']
        )
        optimized['suggested_alternatives'] = alternatives

        # 5. Оптимизация порядка установки
        optimized['components'] = self._topological_sort(
            optimized['components'], deps
        )

        return optimized

    def _topological_sort(
        self,
        components: List[Dict],
        deps: Dict[str, List[str]]
    ) -> List[Dict]:
        """Топологическая сортировка по зависимостям"""

        graph = nx.DiGraph()

        for c in components:
            graph.add_node(c['component_id'])

        for c in components:
            cid = c['component_id']
            for dep in deps.get(cid, []):
                if dep in [x['component_id'] for x in components]:
                    graph.add_edge(dep, cid)

        try:
            order = list(nx.topological_sort(graph))
            return sorted(
                components,
                key=lambda x: order.index(x['component_id'])
            )
        except nx.NetworkXUnfeasible:
            # Цикл в зависимостях
            return components
```

### 5.3.2 Валидация кластеров

```python
from dataclasses import dataclass
from typing import List, Optional
from enum import Enum

class ValidationSeverity(Enum):
    ERROR = "error"
    WARNING = "warning"
    INFO = "info"

@dataclass
class ValidationIssue:
    severity: ValidationSeverity
    code: str
    message: str
    component: Optional[str] = None
    suggestion: Optional[str] = None

class ClusterValidator:
    """Валидация кластеров перед публикацией"""

    def __init__(self, catalog_db, compatibility_analyzer):
        self.catalog_db = catalog_db
        self.compat = compatibility_analyzer

    async def validate(self, cluster: Dict) -> List[ValidationIssue]:
        """Полная валидация кластера"""

        issues = []

        # 1. Структурная валидация
        issues.extend(self._validate_structure(cluster))

        # 2. Проверка существования компонентов
        issues.extend(await self._validate_components_exist(cluster))

        # 3. Проверка совместимости
        issues.extend(await self._validate_compatibility(cluster))

        # 4. Проверка зависимостей
        issues.extend(await self._validate_dependencies(cluster))

        # 5. Проверка конфигурации
        issues.extend(self._validate_configuration(cluster))

        # 6. Проверка скриптов
        issues.extend(await self._validate_scripts(cluster))

        # 7. Проверка на дубликаты функциональности
        issues.extend(await self._validate_no_duplicates(cluster))

        return issues

    def _validate_structure(self, cluster: Dict) -> List[ValidationIssue]:
        """Проверка структуры кластера"""
        issues = []

        required_fields = ['id', 'name', 'category', 'components']
        for field in required_fields:
            if field not in cluster:
                issues.append(ValidationIssue(
                    severity=ValidationSeverity.ERROR,
                    code="MISSING_REQUIRED_FIELD",
                    message=f"Required field '{field}' is missing"
                ))

        if len(cluster.get('components', [])) < 2:
            issues.append(ValidationIssue(
                severity=ValidationSeverity.ERROR,
                code="TOO_FEW_COMPONENTS",
                message="Cluster must have at least 2 components"
            ))

        # Проверка на core компонент
        has_core = any(
            c.get('role') == 'core'
            for c in cluster.get('components', [])
        )
        if not has_core:
            issues.append(ValidationIssue(
                severity=ValidationSeverity.WARNING,
                code="NO_CORE_COMPONENT",
                message="Cluster should have at least one 'core' component",
                suggestion="Mark the main component with role: core"
            ))

        return issues

    async def _validate_compatibility(
        self,
        cluster: Dict
    ) -> List[ValidationIssue]:
        """Проверка совместимости компонентов"""
        issues = []

        components = cluster.get('components', [])
        component_ids = [c['component_id'] for c in components]

        for i, c1 in enumerate(component_ids):
            for c2 in component_ids[i+1:]:
                result = await self.compat.analyze(c1, c2)

                if result.score < 0.3:
                    issues.append(ValidationIssue(
                        severity=ValidationSeverity.ERROR,
                        code="INCOMPATIBLE_COMPONENTS",
                        message=f"Components '{c1}' and '{c2}' are incompatible",
                        suggestion=result.recommendations[0] if result.recommendations else None
                    ))
                elif result.score < 0.7:
                    issues.append(ValidationIssue(
                        severity=ValidationSeverity.WARNING,
                        code="POTENTIAL_COMPATIBILITY_ISSUE",
                        message=f"Components '{c1}' and '{c2}' may have compatibility issues",
                        suggestion="Test thoroughly before deployment"
                    ))

        return issues

    async def _validate_dependencies(
        self,
        cluster: Dict
    ) -> List[ValidationIssue]:
        """Проверка зависимостей"""
        issues = []

        components = cluster.get('components', [])
        component_ids = set(c['component_id'] for c in components)

        for comp in components:
            cid = comp['component_id']
            deps = await self._get_required_dependencies(cid)

            for dep in deps:
                if dep not in component_ids:
                    issues.append(ValidationIssue(
                        severity=ValidationSeverity.ERROR,
                        code="MISSING_DEPENDENCY",
                        message=f"Component '{cid}' requires '{dep}' which is not in cluster",
                        component=cid,
                        suggestion=f"Add '{dep}' to the cluster"
                    ))

        return issues

    async def _validate_no_duplicates(
        self,
        cluster: Dict
    ) -> List[ValidationIssue]:
        """Проверка на дублирование функциональности"""
        issues = []

        components = cluster.get('components', [])

        # Группировка по функции
        functions = defaultdict(list)
        for comp in components:
            func = await self._get_primary_function(comp['component_id'])
            functions[func].append(comp['component_id'])

        for func, comps in functions.items():
            if len(comps) > 1:
                # Проверяем, не альтернативы ли это
                are_alternatives = self._check_if_alternatives(comps, cluster)
                if not are_alternatives:
                    issues.append(ValidationIssue(
                        severity=ValidationSeverity.WARNING,
                        code="DUPLICATE_FUNCTIONALITY",
                        message=f"Multiple components provide '{func}': {comps}",
                        suggestion="Consider removing duplicates or marking as alternatives"
                    ))

        return issues
```

---

## 5.4 Развёртывание кластеров

### 5.4.1 Процесс развёртывания

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      DEPLOYMENT PROCESS                                 │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  1. PREPARATION                                                         │
│     ├── Validate cluster definition                                     │
│     ├── Check target requirements                                       │
│     ├── Collect configuration variables                                 │
│     └── Plan deployment order                                           │
│                                                                         │
│  2. PRE-INSTALL                                                         │
│     ├── Execute pre-install scripts                                     │
│     ├── Create backups                                                  │
│     └── Set up rollback points                                          │
│                                                                         │
│  3. INSTALLATION                                                        │
│     ├── For each component (in order):                                  │
│     │   ├── Download/fetch component                                    │
│     │   ├── Install component                                           │
│     │   ├── Verify installation                                         │
│     │   └── Record state                                                │
│     └── Handle failures with rollback                                   │
│                                                                         │
│  4. CONFIGURATION                                                       │
│     ├── Apply component configurations                                  │
│     ├── Set up integrations between components                          │
│     ├── Configure connections                                           │
│     └── Apply security settings                                         │
│                                                                         │
│  5. VERIFICATION                                                        │
│     ├── Run health checks                                               │
│     ├── Test integrations                                               │
│     ├── Verify functionality                                            │
│     └── Performance baseline                                            │
│                                                                         │
│  6. FINALIZATION                                                        │
│     ├── Clean up temporary files                                        │
│     ├── Generate deployment report                                      │
│     ├── Send notifications                                              │
│     └── Update deployment registry                                      │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 5.4.2 Deployment Engine Implementation

```typescript
interface DeploymentContext {
  clusterId: string;
  targetId: string;
  config: Record<string, unknown>;
  dryRun: boolean;

  // State
  installedComponents: string[];
  rollbackStack: RollbackAction[];
  logs: LogEntry[];
}

class ClusterDeploymentEngine {
  async deploy(
    cluster: Cluster,
    target: DeploymentTarget,
    config: DeploymentConfig
  ): Promise<DeploymentResult> {

    const ctx = this.createContext(cluster, target, config);

    try {
      // Phase 1: Preparation
      await this.prepare(ctx, cluster, target);

      // Phase 2: Pre-install
      await this.preInstall(ctx, cluster);

      // Phase 3: Installation
      for (const component of this.orderComponents(cluster.components)) {
        await this.installComponent(ctx, component, target);
      }

      // Phase 4: Configuration
      await this.configure(ctx, cluster);

      // Phase 5: Verification
      await this.verify(ctx, cluster);

      // Phase 6: Finalization
      await this.finalize(ctx, cluster);

      return this.createSuccessResult(ctx);

    } catch (error) {
      // Rollback on failure
      await this.rollback(ctx);
      return this.createFailureResult(ctx, error);
    }
  }

  private async installComponent(
    ctx: DeploymentContext,
    component: ClusterComponent,
    target: DeploymentTarget
  ): Promise<void> {

    ctx.log(`Installing ${component.componentId}...`);

    // Get connector for target
    const connector = this.getConnector(target.type);

    // Download/prepare
    const artifact = await this.prepareArtifact(component);

    // Install
    const result = await connector.install(
      target.connection,
      artifact,
      component.config
    );

    if (!result.success) {
      throw new InstallationError(component.componentId, result.error);
    }

    // Verify
    const verified = await connector.verify(
      target.connection,
      component.componentId
    );

    if (!verified) {
      throw new VerificationError(component.componentId);
    }

    // Record for rollback
    ctx.rollbackStack.push({
      type: 'uninstall',
      componentId: component.componentId,
      connector,
      target
    });

    ctx.installedComponents.push(component.componentId);
    ctx.log(`Installed ${component.componentId} successfully`);
  }

  private orderComponents(
    components: ClusterComponent[]
  ): ClusterComponent[] {
    // Topological sort based on dependencies
    const graph = new Map<string, Set<string>>();
    const inDegree = new Map<string, number>();

    for (const c of components) {
      graph.set(c.componentId, new Set());
      inDegree.set(c.componentId, 0);
    }

    // Build dependency graph
    for (const c of components) {
      const deps = this.getDependencies(c, components);
      for (const dep of deps) {
        graph.get(dep)!.add(c.componentId);
        inDegree.set(c.componentId, inDegree.get(c.componentId)! + 1);
      }
    }

    // Kahn's algorithm
    const queue: string[] = [];
    for (const [id, degree] of inDegree) {
      if (degree === 0) queue.push(id);
    }

    const order: string[] = [];
    while (queue.length > 0) {
      const current = queue.shift()!;
      order.push(current);

      for (const next of graph.get(current)!) {
        const newDegree = inDegree.get(next)! - 1;
        inDegree.set(next, newDegree);
        if (newDegree === 0) queue.push(next);
      }
    }

    return order.map(id =>
      components.find(c => c.componentId === id)!
    );
  }

  private async rollback(ctx: DeploymentContext): Promise<void> {
    ctx.log('Starting rollback...');

    while (ctx.rollbackStack.length > 0) {
      const action = ctx.rollbackStack.pop()!;

      try {
        await this.executeRollbackAction(action);
        ctx.log(`Rolled back: ${action.componentId}`);
      } catch (error) {
        ctx.log(`Rollback failed for ${action.componentId}: ${error}`);
        // Continue with remaining rollbacks
      }
    }

    ctx.log('Rollback completed');
  }
}
```

---

## 5.5 Примеры полных кластеров

### 5.5.1 SaaS Starter Kit

```yaml
id: "cluster-saas-starter"
name: "SaaS Starter Kit"
category: "productivity"
subcategory: "saas"
level: 3

description: |
  Всё необходимое для запуска SaaS продукта:
  аутентификация, биллинг, email, аналитика, мониторинг.

target_platforms:
  - vercel
  - aws
  - self-hosted

tech_stack:
  frontend: "next.js"
  backend: "node.js"
  database: "postgresql"

components:
  # Framework
  - id: "npm/next"
    role: core
    version: "^14.0.0"

  # Authentication
  - id: "npm/next-auth"
    role: core
    config:
      providers: ["google", "github", "email"]

  # Database
  - id: "npm/prisma"
    role: core
    config:
      provider: "postgresql"

  # Payments
  - id: "npm/stripe"
    role: integration

  # Email
  - id: "npm/resend"
    role: integration
    alternatives:
      - "npm/sendgrid"
      - "npm/postmark"

  # UI
  - id: "npm/tailwindcss"
    role: enhancement

  - id: "npm/shadcn-ui"
    role: enhancement

  # Analytics
  - id: "npm/posthog-js"
    role: monitoring
    alternatives:
      - "npm/mixpanel"
      - "npm/amplitude"

  # Error Tracking
  - id: "npm/sentry"
    role: monitoring

  # Feature Flags
  - id: "npm/launchdarkly"
    role: enhancement
    required: false

  # Testing
  - id: "npm/vitest"
    role: utility

  - id: "npm/playwright"
    role: utility

configuration:
  variables:
    - name: "DATABASE_URL"
      type: "string"
      required: true

    - name: "NEXTAUTH_SECRET"
      type: "secret"
      required: true

    - name: "STRIPE_SECRET_KEY"
      type: "secret"
      required: true

    - name: "RESEND_API_KEY"
      type: "secret"
      required: true

    - name: "SENTRY_DSN"
      type: "string"
      required: false

scripts:
  install: |
    npx create-next-app@latest . --typescript --tailwind --app
    npm install next-auth @prisma/client stripe resend
    npm install @sentry/nextjs posthog-js
    npm install -D prisma vitest @playwright/test

  configure: |
    # Initialize Prisma
    npx prisma init

    # Set up NextAuth
    mkdir -p app/api/auth/[...nextauth]
    # ... configuration files

  health_check: |
    npm run build
    npm run test

estimated_setup_time: "1-2 hours"
monthly_cost: "$50-200"
```

---

## 5.6 Выводы раздела

### Ключевые концепции

1. **Кластеры — это больше, чем списки** — это проверенные, конфигурированные, интегрированные решения

2. **Иерархия кластеров** позволяет компоновать простые решения в сложные

3. **Автоматическое обнаружение** на основе co-installation patterns выявляет популярные комбинации

4. **Строгая валидация** гарантирует качество и совместимость

5. **Deployment Engine** обеспечивает надёжное развёртывание с rollback

→ [Раздел 6: Система макросов](../06-macros/README.md)
