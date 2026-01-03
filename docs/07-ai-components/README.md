# Раздел 7: ИИ-компоненты

## 7.1 Роль ИИ в системе

### 7.1.1 Философия: Систематизация, а не генерация

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    AI ROLE IN BIOS-I                                    │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ТРАДИЦИОННЫЙ ПОДХОД К ИИ:                                             │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  "Сгенерируй мне код"                                           │   │
│  │  "Напиши статью"                                                │   │
│  │  "Создай изображение"                                           │   │
│  │                                                                  │   │
│  │  ФОКУС: Создание нового контента                                │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  ПОДХОД BIOS-I:                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │  "Найди подходящие компоненты"                                  │   │
│  │  "Определи совместимость"                                       │   │
│  │  "Предложи оптимальный кластер"                                 │   │
│  │  "Объясни, как это работает"                                    │   │
│  │                                                                  │   │
│  │  ФОКУС: Организация существующего                               │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
│  КЛЮЧЕВОЕ ОТЛИЧИЕ:                                                      │
│  ├── Не изобретаем колесо                                              │
│  ├── Находим лучшее существующее колесо                                │
│  ├── Объясняем, как его использовать                                   │
│  └── Комбинируем с другими колёсами                                    │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 7.1.2 Задачи ИИ в системе

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         AI TASKS                                        │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  1. ПОНИМАНИЕ ЗАПРОСОВ                                                  │
│     ├── Интерпретация естественного языка                              │
│     ├── Извлечение требований                                          │
│     └── Уточняющие вопросы                                             │
│                                                                         │
│  2. СЕМАНТИЧЕСКИЙ ПОИСК                                                │
│     ├── Поиск по смыслу, не по ключевым словам                         │
│     ├── Понимание синонимов и контекста                                │
│     └── Ранжирование по релевантности                                  │
│                                                                         │
│  3. РЕКОМЕНДАЦИИ                                                        │
│     ├── Подбор компонентов под задачу                                  │
│     ├── Предложение кластеров                                          │
│     └── Персонализация под контекст                                    │
│                                                                         │
│  4. АНАЛИЗ СОВМЕСТИМОСТИ                                               │
│     ├── Предсказание конфликтов                                        │
│     ├── Оценка качества комбинаций                                     │
│     └── Выявление рисков                                               │
│                                                                         │
│  5. ГЕНЕРАЦИЯ ДОКУМЕНТАЦИИ                                             │
│     ├── Автоматические описания                                        │
│     ├── Инструкции по установке                                        │
│     └── Troubleshooting guides                                         │
│                                                                         │
│  6. ОПТИМИЗАЦИЯ                                                         │
│     ├── Предложения по улучшению кластеров                             │
│     ├── Выявление неэффективностей                                     │
│     └── Альтернативные решения                                         │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 7.2 Архитектура ИИ-системы

### 7.2.1 Общая архитектура

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      AI SYSTEM ARCHITECTURE                             │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      USER INTERFACE                              │   │
│  │                                                                  │   │
│  │  "I need to automate my e-commerce email marketing"              │   │
│  │                            │                                     │   │
│  └────────────────────────────┼─────────────────────────────────────┘   │
│                               │                                         │
│                               v                                         │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                   AI ORCHESTRATOR                                │   │
│  │                                                                  │   │
│  │  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐       │   │
│  │  │    Intent    │    │   Context    │    │   Response   │       │   │
│  │  │   Classifier │───>│   Builder    │───>│  Generator   │       │   │
│  │  └──────────────┘    └──────────────┘    └──────────────┘       │   │
│  │                                                                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│           │                     │                     │                 │
│           v                     v                     v                 │
│  ┌─────────────┐       ┌─────────────┐       ┌─────────────┐           │
│  │   LLM API   │       │  Embedding  │       │   Custom    │           │
│  │  (Claude/   │       │   Model     │       │   Models    │           │
│  │   GPT-4)    │       │             │       │             │           │
│  └─────────────┘       └─────────────┘       └─────────────┘           │
│           │                     │                     │                 │
│           └─────────────────────┼─────────────────────┘                 │
│                                 │                                       │
│                                 v                                       │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                      DATA LAYER                                  │   │
│  │                                                                  │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐        │   │
│  │  │  Vector  │  │   Graph  │  │  Catalog │  │   User   │        │   │
│  │  │   DB     │  │    DB    │  │    DB    │  │   Data   │        │   │
│  │  │ (Qdrant) │  │  (Neo4j) │  │(Postgres)│  │          │        │   │
│  │  └──────────┘  └──────────┘  └──────────┘  └──────────┘        │   │
│  │                                                                  │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 7.2.2 Компоненты системы

```python
from abc import ABC, abstractmethod
from typing import List, Dict, Optional
from dataclasses import dataclass

@dataclass
class AIResponse:
    text: str
    recommendations: List[Dict]
    clusters: List[Dict]
    confidence: float
    sources: List[str]

class AIOrchestrator:
    """Главный оркестратор AI-системы"""

    def __init__(
        self,
        llm_client: LLMClient,
        embedding_model: EmbeddingModel,
        vector_store: VectorStore,
        graph_db: GraphDatabase,
        catalog_db: CatalogDatabase
    ):
        self.llm = llm_client
        self.embedder = embedding_model
        self.vectors = vector_store
        self.graph = graph_db
        self.catalog = catalog_db

        # Специализированные модули
        self.intent_classifier = IntentClassifier(llm_client)
        self.context_builder = ContextBuilder(catalog_db, graph_db)
        self.response_generator = ResponseGenerator(llm_client)

    async def process(
        self,
        user_message: str,
        conversation_history: List[Dict],
        user_context: Dict
    ) -> AIResponse:
        """Обработка запроса пользователя"""

        # 1. Классификация намерения
        intent = await self.intent_classifier.classify(
            user_message,
            conversation_history
        )

        # 2. Построение контекста
        context = await self.context_builder.build(
            intent,
            user_context
        )

        # 3. Выполнение соответствующего действия
        if intent.type == 'search':
            result = await self.handle_search(intent, context)
        elif intent.type == 'recommend':
            result = await self.handle_recommendation(intent, context)
        elif intent.type == 'explain':
            result = await self.handle_explanation(intent, context)
        elif intent.type == 'compare':
            result = await self.handle_comparison(intent, context)
        elif intent.type == 'deploy':
            result = await self.handle_deployment(intent, context)
        else:
            result = await self.handle_general(intent, context)

        # 4. Генерация ответа
        response = await self.response_generator.generate(
            intent,
            result,
            context
        )

        return response


class IntentClassifier:
    """Классификация намерений пользователя"""

    INTENTS = [
        'search',       # Поиск компонентов
        'recommend',    # Рекомендация решения
        'explain',      # Объяснение компонента/кластера
        'compare',      # Сравнение вариантов
        'deploy',       # Развёртывание
        'configure',    # Настройка
        'troubleshoot', # Решение проблем
        'general'       # Общий вопрос
    ]

    def __init__(self, llm_client: LLMClient):
        self.llm = llm_client

    async def classify(
        self,
        message: str,
        history: List[Dict]
    ) -> Intent:
        """Классификация намерения"""

        prompt = f"""Classify the user's intent based on their message.

Available intents:
- search: Looking for specific components or tools
- recommend: Wants suggestions for solving a problem
- explain: Wants to understand how something works
- compare: Wants to compare options
- deploy: Wants to install/deploy something
- configure: Needs help with configuration
- troubleshoot: Has a problem to solve
- general: General question

User message: {message}

Previous context:
{self._format_history(history)}

Respond with JSON:
{{
  "intent": "<intent_type>",
  "entities": {{
    "ecosystem": "<detected_ecosystem>",
    "category": "<detected_category>",
    "components": ["<mentioned_components>"],
    "requirements": ["<extracted_requirements>"]
  }},
  "confidence": <0.0-1.0>
}}
"""

        response = await self.llm.complete(prompt)
        return self._parse_intent(response)


class ContextBuilder:
    """Построение контекста для ИИ"""

    def __init__(
        self,
        catalog_db: CatalogDatabase,
        graph_db: GraphDatabase
    ):
        self.catalog = catalog_db
        self.graph = graph_db

    async def build(
        self,
        intent: Intent,
        user_context: Dict
    ) -> Context:
        """Построение релевантного контекста"""

        context = Context()

        # Добавляем информацию о текущем стеке пользователя
        if user_context.get('installed_components'):
            context.installed = await self.catalog.get_components(
                user_context['installed_components']
            )

        # Добавляем релевантные компоненты из намерения
        if intent.entities.get('components'):
            context.mentioned = await self.catalog.search_components(
                intent.entities['components']
            )

        # Добавляем рекомендации из графа
        if context.installed:
            context.graph_recommendations = await self.graph.get_recommendations(
                [c.id for c in context.installed]
            )

        # Добавляем популярные решения в категории
        if intent.entities.get('category'):
            context.popular_in_category = await self.catalog.get_popular(
                category=intent.entities['category'],
                limit=10
            )

        return context
```

---

## 7.3 Семантический поиск

### 7.3.1 Embedding Pipeline

```python
class EmbeddingPipeline:
    """Конвейер создания и поиска embeddings"""

    def __init__(
        self,
        model_name: str = "text-embedding-3-large",
        vector_store: VectorStore = None
    ):
        self.model_name = model_name
        self.vector_store = vector_store
        self.dimension = 3072  # для text-embedding-3-large

    async def embed_component(
        self,
        component: Dict
    ) -> List[float]:
        """Создание embedding для компонента"""

        # Формируем текст для embedding
        text = self._prepare_text(component)

        # Получаем embedding
        embedding = await self._get_embedding(text)

        return embedding

    def _prepare_text(self, component: Dict) -> str:
        """Подготовка текста для embedding"""

        parts = []

        # Название (высокий вес)
        parts.append(f"Name: {component['name']}")

        # Описание
        if component.get('description'):
            parts.append(f"Description: {component['description'][:500]}")

        # Категории и теги
        if component.get('categories'):
            parts.append(f"Categories: {', '.join(component['categories'])}")

        if component.get('tags'):
            parts.append(f"Tags: {', '.join(component['tags'])}")

        # Функциональность (из README если есть)
        if component.get('readme_summary'):
            parts.append(f"Features: {component['readme_summary']}")

        return '\n'.join(parts)

    async def index_all_components(self):
        """Индексация всех компонентов"""

        batch_size = 100
        offset = 0

        while True:
            components = await self.catalog.get_components(
                limit=batch_size,
                offset=offset
            )

            if not components:
                break

            # Создаём embeddings пачками
            embeddings = []
            for component in components:
                embedding = await self.embed_component(component)
                embeddings.append({
                    'id': component['id'],
                    'vector': embedding,
                    'payload': {
                        'name': component['name'],
                        'ecosystem': component['ecosystem'],
                        'type': component['type'],
                        'categories': component.get('categories', []),
                        'health_score': component.get('metrics', {}).get('health_score', 0)
                    }
                })

            # Загружаем в vector store
            await self.vector_store.upsert(
                collection='components',
                points=embeddings
            )

            offset += batch_size
            print(f"Indexed {offset} components")


class SemanticSearch:
    """Семантический поиск"""

    def __init__(
        self,
        embedding_pipeline: EmbeddingPipeline,
        vector_store: VectorStore,
        catalog_db: CatalogDatabase
    ):
        self.embedder = embedding_pipeline
        self.vectors = vector_store
        self.catalog = catalog_db

    async def search(
        self,
        query: str,
        filters: Dict = None,
        limit: int = 20
    ) -> List[SearchResult]:
        """Семантический поиск компонентов"""

        # 1. Получаем embedding запроса
        query_embedding = await self.embedder._get_embedding(query)

        # 2. Строим фильтры для vector store
        vector_filter = self._build_filter(filters)

        # 3. Поиск в vector store
        vector_results = await self.vectors.search(
            collection='components',
            query_vector=query_embedding,
            filter=vector_filter,
            limit=limit * 2  # Берём больше для пост-фильтрации
        )

        # 4. Обогащаем данными из каталога
        component_ids = [r.id for r in vector_results]
        full_components = await self.catalog.get_components_by_ids(component_ids)

        # 5. Ранжируем с учётом дополнительных факторов
        results = []
        for vr in vector_results:
            component = next(
                (c for c in full_components if c['id'] == vr.id),
                None
            )
            if component:
                final_score = self._calculate_final_score(
                    vr.score,
                    component
                )
                results.append(SearchResult(
                    component=component,
                    semantic_score=vr.score,
                    final_score=final_score
                ))

        # 6. Сортируем и возвращаем
        results.sort(key=lambda x: x.final_score, reverse=True)
        return results[:limit]

    def _calculate_final_score(
        self,
        semantic_score: float,
        component: Dict
    ) -> float:
        """Финальный скор с учётом качества и популярности"""

        # Semantic score: 0.6 weight
        score = semantic_score * 0.6

        # Health score: 0.2 weight
        health = component.get('metrics', {}).get('health_score', 50) / 100
        score += health * 0.2

        # Popularity: 0.1 weight
        downloads = component.get('metrics', {}).get('downloads_weekly', 0)
        popularity = min(1.0, downloads / 100000)
        score += popularity * 0.1

        # Rating: 0.1 weight
        rating = component.get('metrics', {}).get('rating', 3) / 5
        score += rating * 0.1

        return score
```

### 7.3.2 Query Understanding

```python
class QueryUnderstanding:
    """Понимание и обогащение запросов"""

    def __init__(self, llm_client: LLMClient):
        self.llm = llm_client
        self.synonyms = self._load_synonyms()

    async def enhance_query(self, query: str) -> EnhancedQuery:
        """Обогащение запроса"""

        # 1. Расширение синонимами
        expanded = self._expand_synonyms(query)

        # 2. Извлечение сущностей через LLM
        entities = await self._extract_entities(query)

        # 3. Определение категории
        category = await self._classify_category(query)

        # 4. Генерация вариаций запроса
        variations = await self._generate_variations(query)

        return EnhancedQuery(
            original=query,
            expanded=expanded,
            entities=entities,
            category=category,
            variations=variations
        )

    def _expand_synonyms(self, query: str) -> str:
        """Расширение синонимами"""

        words = query.lower().split()
        expanded_words = []

        for word in words:
            expanded_words.append(word)
            if word in self.synonyms:
                expanded_words.extend(self.synonyms[word])

        return ' '.join(expanded_words)

    async def _extract_entities(self, query: str) -> Dict:
        """Извлечение сущностей из запроса"""

        prompt = f"""Extract entities from this search query:

Query: "{query}"

Extract:
- component_names: specific tools/libraries mentioned
- technologies: programming languages, frameworks
- use_case: what the user wants to achieve
- platform: target platform (wordpress, node, etc.)

Respond with JSON:
{{
  "component_names": [],
  "technologies": [],
  "use_case": "",
  "platform": ""
}}
"""

        response = await self.llm.complete(prompt)
        return json.loads(response)

    async def _generate_variations(
        self,
        query: str,
        n: int = 3
    ) -> List[str]:
        """Генерация вариаций запроса"""

        prompt = f"""Generate {n} alternative phrasings of this search query.
Keep the same meaning but use different words.

Query: "{query}"

Respond with JSON array of strings.
"""

        response = await self.llm.complete(prompt)
        return json.loads(response)

    def _load_synonyms(self) -> Dict[str, List[str]]:
        """Загрузка словаря синонимов"""

        return {
            'auth': ['authentication', 'login', 'oauth', 'sso'],
            'db': ['database', 'storage', 'persistence'],
            'email': ['mail', 'smtp', 'messaging'],
            'payments': ['billing', 'checkout', 'stripe', 'payment'],
            'analytics': ['tracking', 'metrics', 'statistics'],
            'cache': ['caching', 'redis', 'memcached'],
            'queue': ['messaging', 'rabbitmq', 'kafka', 'jobs'],
            # ... ещё много синонимов
        }
```

---

## 7.4 Система рекомендаций

### 7.4.1 Recommendation Engine

```python
class RecommendationEngine:
    """Движок рекомендаций"""

    def __init__(
        self,
        catalog_db: CatalogDatabase,
        graph_db: GraphDatabase,
        vector_store: VectorStore,
        llm_client: LLMClient
    ):
        self.catalog = catalog_db
        self.graph = graph_db
        self.vectors = vector_store
        self.llm = llm_client

    async def get_recommendations(
        self,
        context: RecommendationContext
    ) -> RecommendationResult:
        """Получение рекомендаций"""

        # Собираем рекомендации из разных источников
        sources = await asyncio.gather(
            self._collaborative_filtering(context),
            self._content_based(context),
            self._graph_based(context),
            self._llm_enhanced(context)
        )

        collaborative, content, graph, llm_enhanced = sources

        # Объединяем и ранжируем
        combined = self._combine_recommendations(
            collaborative, content, graph, llm_enhanced,
            weights=[0.3, 0.2, 0.3, 0.2]
        )

        # Фильтруем уже установленные
        filtered = [
            r for r in combined
            if r.component_id not in context.installed_components
        ]

        # Добавляем объяснения
        with_explanations = await self._add_explanations(filtered[:10])

        return RecommendationResult(
            recommendations=with_explanations,
            context_summary=await self._summarize_context(context)
        )

    async def _collaborative_filtering(
        self,
        context: RecommendationContext
    ) -> List[ScoredComponent]:
        """Коллаборативная фильтрация"""

        if not context.installed_components:
            return []

        # Находим пользователей с похожими установками
        query = """
        MATCH (u:User)-[:INSTALLED]->(c:Component)
        WHERE c.id IN $installed
        WITH u, count(c) as overlap
        WHERE overlap >= 2

        MATCH (u)-[:INSTALLED]->(other:Component)
        WHERE NOT other.id IN $installed
        WITH other, count(DISTINCT u) as user_count

        RETURN other.id as component_id, user_count
        ORDER BY user_count DESC
        LIMIT 50
        """

        results = await self.graph.run(
            query,
            installed=context.installed_components
        )

        max_count = results[0]['user_count'] if results else 1
        return [
            ScoredComponent(
                component_id=r['component_id'],
                score=r['user_count'] / max_count,
                source='collaborative'
            )
            for r in results
        ]

    async def _content_based(
        self,
        context: RecommendationContext
    ) -> List[ScoredComponent]:
        """Рекомендации на основе контента"""

        if not context.use_case:
            return []

        # Поиск по описанию use case
        search_results = await self.vectors.search(
            collection='components',
            query_vector=await self._embed(context.use_case),
            limit=30
        )

        return [
            ScoredComponent(
                component_id=r.id,
                score=r.score,
                source='content'
            )
            for r in search_results
        ]

    async def _graph_based(
        self,
        context: RecommendationContext
    ) -> List[ScoredComponent]:
        """Рекомендации на основе графа"""

        if not context.installed_components:
            return []

        # Компоненты, интегрирующиеся с установленными
        query = """
        MATCH (installed:Component)-[r:INTEGRATES_WITH|COMPATIBLE_WITH]-(rec:Component)
        WHERE installed.id IN $installed
        AND NOT rec.id IN $installed

        WITH rec, avg(r.score) as avg_score, count(r) as connections
        RETURN rec.id as component_id,
               avg_score * connections as combined_score
        ORDER BY combined_score DESC
        LIMIT 30
        """

        results = await self.graph.run(
            query,
            installed=context.installed_components
        )

        max_score = results[0]['combined_score'] if results else 1
        return [
            ScoredComponent(
                component_id=r['component_id'],
                score=r['combined_score'] / max_score,
                source='graph'
            )
            for r in results
        ]

    async def _llm_enhanced(
        self,
        context: RecommendationContext
    ) -> List[ScoredComponent]:
        """Рекомендации, улучшенные LLM"""

        # Формируем промпт с контекстом
        prompt = f"""Based on the following context, recommend software components.

User's current stack: {context.installed_components}
User's goal: {context.use_case}
Platform: {context.platform}
Budget: {context.budget}

Consider:
1. What's missing in their current stack
2. What would integrate well
3. Industry best practices

Recommend 5-10 specific components with brief reasons.
Format as JSON: [{{"component_name": "...", "reason": "...", "priority": 1-10}}]
"""

        response = await self.llm.complete(prompt)
        suggestions = json.loads(response)

        # Матчим с реальными компонентами в каталоге
        results = []
        for s in suggestions:
            match = await self.catalog.find_by_name(s['component_name'])
            if match:
                results.append(ScoredComponent(
                    component_id=match['id'],
                    score=s['priority'] / 10,
                    source='llm',
                    explanation=s['reason']
                ))

        return results

    async def _add_explanations(
        self,
        recommendations: List[ScoredComponent]
    ) -> List[RecommendationWithExplanation]:
        """Добавление объяснений к рекомендациям"""

        results = []
        for rec in recommendations:
            component = await self.catalog.get_by_id(rec.component_id)

            explanation = await self._generate_explanation(
                component,
                rec.source,
                rec.score
            )

            results.append(RecommendationWithExplanation(
                component=component,
                score=rec.score,
                explanation=explanation,
                sources=[rec.source]
            ))

        return results

    async def _generate_explanation(
        self,
        component: Dict,
        source: str,
        score: float
    ) -> str:
        """Генерация объяснения рекомендации"""

        explanations = {
            'collaborative': f"Users with similar stacks often use {component['name']}",
            'content': f"{component['name']} matches your requirements",
            'graph': f"{component['name']} integrates well with your current tools",
            'llm': component.get('llm_explanation', f"Recommended: {component['name']}")
        }

        return explanations.get(source, f"Recommended: {component['name']}")
```

---

## 7.5 AI Assistant (Чат-бот)

### 7.5.1 Архитектура ассистента

```python
class AIAssistant:
    """ИИ-ассистент для взаимодействия с пользователем"""

    def __init__(
        self,
        llm_client: LLMClient,
        orchestrator: AIOrchestrator,
        catalog_db: CatalogDatabase
    ):
        self.llm = llm_client
        self.orchestrator = orchestrator
        self.catalog = catalog_db

    async def chat(
        self,
        message: str,
        conversation_id: str,
        user_context: Dict
    ) -> AssistantResponse:
        """Обработка сообщения в чате"""

        # Загружаем историю
        history = await self.load_history(conversation_id)

        # Обрабатываем через оркестратор
        ai_response = await self.orchestrator.process(
            message,
            history,
            user_context
        )

        # Форматируем ответ
        formatted = await self.format_response(ai_response)

        # Сохраняем в историю
        await self.save_to_history(
            conversation_id,
            message,
            formatted
        )

        return formatted

    async def format_response(
        self,
        ai_response: AIResponse
    ) -> AssistantResponse:
        """Форматирование ответа для UI"""

        response = AssistantResponse(
            text=ai_response.text,
            components=[],
            clusters=[],
            actions=[]
        )

        # Добавляем карточки компонентов
        for rec in ai_response.recommendations:
            component = await self.catalog.get_by_id(rec['component_id'])
            response.components.append(ComponentCard(
                id=component['id'],
                name=component['name'],
                description=component['short_description'],
                health_score=component['metrics']['health_score'],
                explanation=rec.get('explanation', '')
            ))

        # Добавляем кластеры
        for cluster in ai_response.clusters:
            response.clusters.append(ClusterCard(
                id=cluster['id'],
                name=cluster['name'],
                components_count=len(cluster['components']),
                match_score=cluster.get('match_score', 0)
            ))

        # Добавляем доступные действия
        response.actions = self._determine_actions(ai_response)

        return response

    def _determine_actions(
        self,
        ai_response: AIResponse
    ) -> List[Action]:
        """Определение доступных действий"""

        actions = []

        if ai_response.recommendations:
            actions.append(Action(
                type='compare',
                label='Compare options',
                data={'components': [r['component_id'] for r in ai_response.recommendations]}
            ))

        if ai_response.clusters:
            actions.append(Action(
                type='deploy',
                label='Deploy cluster',
                data={'cluster_id': ai_response.clusters[0]['id']}
            ))

        actions.append(Action(
            type='refine',
            label='Refine search',
            data={}
        ))

        return actions


# System Prompt для ассистента
ASSISTANT_SYSTEM_PROMPT = """You are BIOS-I Assistant, an AI helper for the Business Internet Operating System.

Your role is to help users:
1. Find the right software components for their needs
2. Understand how different tools work together
3. Choose and deploy clusters of integrated components
4. Solve integration and compatibility issues

Key principles:
- NEVER suggest creating new software - always recommend existing solutions
- Focus on INTEGRATION and RATIONALIZATION, not invention
- Provide specific, actionable recommendations
- Explain trade-offs between options
- Consider the user's existing stack and context

Available ecosystems: WordPress, npm, PyPI, GitHub, Make.com, Zapier, Docker

When recommending:
- Mention component names specifically
- Explain why each component is recommended
- Note any compatibility considerations
- Suggest relevant clusters if available

Format responses clearly with:
- Brief explanation
- Specific recommendations (bulleted)
- Next steps the user can take
"""
```

---

## 7.6 Модели машинного обучения

### 7.6.1 Модели для системы

```python
# Модели, используемые в BIOS-I

MODELS = {
    # Embedding модели
    'embedding': {
        'primary': 'text-embedding-3-large',  # OpenAI, 3072 dim
        'fallback': 'text-embedding-3-small',  # OpenAI, 1536 dim
        'local': 'sentence-transformers/all-mpnet-base-v2'  # Local fallback
    },

    # LLM для генерации
    'llm': {
        'primary': 'claude-3-5-sonnet',  # Anthropic
        'fallback': 'gpt-4-turbo',  # OpenAI
        'fast': 'claude-3-haiku'  # Для быстрых операций
    },

    # Кастомные модели
    'custom': {
        'compatibility_predictor': 'bios-i/compat-bert-v2',
        'category_classifier': 'bios-i/category-classifier-v1',
        'health_scorer': 'bios-i/health-xgboost-v1'
    }
}

class CompatibilityPredictor:
    """Предсказание совместимости компонентов"""

    def __init__(self, model_path: str):
        self.model = self._load_model(model_path)
        self.tokenizer = AutoTokenizer.from_pretrained(model_path)

    def predict(
        self,
        component_a: Dict,
        component_b: Dict
    ) -> CompatibilityPrediction:
        """Предсказание совместимости двух компонентов"""

        # Формируем входные данные
        features = self._extract_features(component_a, component_b)

        # Текстовое представление для BERT
        text = self._create_text_representation(component_a, component_b)
        encoded = self.tokenizer(
            text,
            padding=True,
            truncation=True,
            return_tensors='pt'
        )

        # Предсказание
        with torch.no_grad():
            outputs = self.model(**encoded, features=features)

        probability = torch.sigmoid(outputs.logits).item()

        return CompatibilityPrediction(
            score=probability,
            confidence=outputs.confidence.item(),
            risk_factors=self._extract_risk_factors(outputs)
        )

    def _extract_features(
        self,
        a: Dict,
        b: Dict
    ) -> torch.Tensor:
        """Извлечение числовых признаков"""

        features = [
            # Одна экосистема?
            1.0 if a['ecosystem'] == b['ecosystem'] else 0.0,

            # Общие категории
            len(set(a.get('categories', [])) & set(b.get('categories', []))) / 5,

            # Общие теги
            len(set(a.get('tags', [])) & set(b.get('tags', []))) / 10,

            # Разница в health score
            abs(a['metrics']['health_score'] - b['metrics']['health_score']) / 100,

            # Активность
            min(a['metrics'].get('commits_monthly', 0), 100) / 100,
            min(b['metrics'].get('commits_monthly', 0), 100) / 100,
        ]

        return torch.tensor(features).unsqueeze(0)


class CategoryClassifier:
    """Классификация компонентов по категориям"""

    CATEGORIES = [
        'ecommerce', 'cms', 'analytics', 'security',
        'performance', 'seo', 'email', 'crm', 'payments',
        'social', 'forms', 'backup', 'development'
    ]

    def __init__(self, model_path: str):
        self.model = AutoModelForSequenceClassification.from_pretrained(model_path)
        self.tokenizer = AutoTokenizer.from_pretrained(model_path)

    def classify(self, component: Dict) -> List[CategoryScore]:
        """Классификация компонента"""

        text = f"{component['name']} {component.get('description', '')}"
        encoded = self.tokenizer(
            text,
            padding=True,
            truncation=True,
            return_tensors='pt'
        )

        with torch.no_grad():
            outputs = self.model(**encoded)

        probabilities = torch.softmax(outputs.logits, dim=-1)[0]

        results = []
        for i, category in enumerate(self.CATEGORIES):
            results.append(CategoryScore(
                category=category,
                score=probabilities[i].item()
            ))

        return sorted(results, key=lambda x: x.score, reverse=True)
```

---

## 7.7 Выводы раздела

### Ключевые ИИ-компоненты

1. **Семантический поиск** — понимание смысла запросов, а не ключевых слов

2. **Рекомендательная система** — комбинация collaborative filtering, content-based и graph-based подходов

3. **AI Assistant** — интерфейс естественного языка для взаимодействия с системой

4. **Кастомные модели** — специализированные модели для предсказания совместимости и классификации

### Принципы использования ИИ

| Задача | ИИ делает | ИИ НЕ делает |
|--------|-----------|--------------|
| Поиск | Понимает запрос | Не придумывает компоненты |
| Рекомендации | Предлагает существующее | Не генерирует код |
| Документация | Объясняет и описывает | Не изобретает функции |
| Совместимость | Предсказывает риски | Не гарантирует работу |

→ [Раздел 8: Дорожная карта](../08-roadmap/README.md)
