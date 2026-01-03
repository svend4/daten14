# Раздел 4: Система каталогизации

## 4.1 Обзор системы каталогизации

### 4.1.1 Цели и задачи

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    CATALOG SYSTEM OBJECTIVES                            │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  ПЕРВИЧНЫЕ ЦЕЛИ:                                                       │
│  ├── Агрегация данных из всех источников                               │
│  ├── Нормализация в единый формат                                      │
│  ├── Обогащение метаданными                                            │
│  └── Обеспечение быстрого поиска                                       │
│                                                                         │
│  ВТОРИЧНЫЕ ЦЕЛИ:                                                        │
│  ├── Построение графа связей                                           │
│  ├── Вычисление метрик качества                                        │
│  ├── Отслеживание изменений                                            │
│  └── Генерация отчётов                                                 │
│                                                                         │
│  МЕТРИКИ УСПЕХА:                                                        │
│  ├── Покрытие: 95% активных компонентов топ-экосистем                  │
│  ├── Актуальность: обновление < 24 часа                                │
│  ├── Полнота: все обязательные поля заполнены                          │
│  └── Точность: < 1% ошибок в метаданных                                │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### 4.1.2 Поток данных

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         DATA FLOW                                       │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  SOURCES              INGESTION           PROCESSING          STORAGE  │
│                                                                         │
│  ┌──────────┐        ┌──────────┐        ┌──────────┐       ┌────────┐ │
│  │WordPress │───────>│          │        │          │       │        │ │
│  │   API    │        │          │        │          │       │        │ │
│  └──────────┘        │          │        │          │       │        │ │
│                      │  Crawler │        │          │       │        │ │
│  ┌──────────┐        │  Fleet   │───────>│   ETL    │──────>│Catalog │ │
│  │  GitHub  │───────>│          │        │ Pipeline │       │   DB   │ │
│  │   API    │        │          │        │          │       │        │ │
│  └──────────┘        │          │        │          │       │        │ │
│                      │          │        │          │       │        │ │
│  ┌──────────┐        │          │        │          │       │        │ │
│  │   npm    │───────>│          │        │          │       │        │ │
│  │ Registry │        └──────────┘        └──────────┘       └────────┘ │
│  └──────────┘              │                   │                  │    │
│                            │                   │                  │    │
│  ┌──────────┐              v                   v                  v    │
│  │  Make    │        ┌──────────┐        ┌──────────┐       ┌────────┐ │
│  │  .com    │───────>│  Queue   │        │ Enricher │──────>│ Graph  │ │
│  └──────────┘        │ (Kafka)  │        │          │       │  DB    │ │
│                      └──────────┘        └──────────┘       └────────┘ │
│  ┌──────────┐                                  │                  │    │
│  │  PyPI    │                                  │                  │    │
│  └──────────┘                                  v                  v    │
│                                          ┌──────────┐       ┌────────┐ │
│  ┌──────────┐                            │ Embedder │──────>│ Vector │ │
│  │  Docker  │                            │          │       │  DB    │ │
│  │   Hub    │                            └──────────┘       └────────┘ │
│  └──────────┘                                                          │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 4.2 Схемы данных

### 4.2.1 JSON Schema для компонента

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "https://bios-i.io/schemas/component.json",
  "title": "Universal Component",
  "description": "Schema for any software component in BIOS-I catalog",
  "type": "object",

  "required": [
    "id",
    "ecosystem",
    "native_id",
    "slug",
    "name",
    "type"
  ],

  "properties": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "Unique identifier in BIOS-I"
    },

    "ecosystem": {
      "type": "string",
      "enum": [
        "wordpress",
        "npm",
        "pypi",
        "github",
        "makecom",
        "zapier",
        "composer",
        "rubygems",
        "docker",
        "homebrew",
        "apt",
        "crates"
      ],
      "description": "Source ecosystem"
    },

    "native_id": {
      "type": "string",
      "description": "ID in the source ecosystem"
    },

    "slug": {
      "type": "string",
      "pattern": "^[a-z0-9-]+$",
      "description": "URL-friendly identifier"
    },

    "name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 500,
      "description": "Human-readable name"
    },

    "description": {
      "type": "string",
      "maxLength": 10000,
      "description": "Full description"
    },

    "short_description": {
      "type": "string",
      "maxLength": 500,
      "description": "Brief description for lists"
    },

    "type": {
      "type": "string",
      "enum": [
        "plugin",
        "package",
        "library",
        "framework",
        "tool",
        "app",
        "template",
        "boilerplate",
        "connector",
        "theme"
      ]
    },

    "categories": {
      "type": "array",
      "items": {
        "type": "string"
      },
      "description": "Hierarchical categories"
    },

    "tags": {
      "type": "array",
      "items": {
        "type": "string"
      },
      "description": "Flat tags for search"
    },

    "urls": {
      "type": "object",
      "properties": {
        "homepage": { "type": "string", "format": "uri" },
        "repository": { "type": "string", "format": "uri" },
        "documentation": { "type": "string", "format": "uri" },
        "issues": { "type": "string", "format": "uri" },
        "changelog": { "type": "string", "format": "uri" }
      }
    },

    "author": {
      "type": "object",
      "properties": {
        "name": { "type": "string" },
        "email": { "type": "string", "format": "email" },
        "url": { "type": "string", "format": "uri" },
        "organization": { "type": "string" }
      }
    },

    "license": {
      "type": "object",
      "properties": {
        "spdx": { "type": "string" },
        "name": { "type": "string" },
        "url": { "type": "string", "format": "uri" }
      }
    },

    "version": {
      "type": "object",
      "properties": {
        "current": { "type": "string" },
        "stable": { "type": "string" },
        "all": {
          "type": "array",
          "items": { "type": "string" }
        }
      }
    },

    "requirements": {
      "type": "object",
      "properties": {
        "platforms": {
          "type": "array",
          "items": { "type": "string" }
        },
        "runtime": {
          "type": "object",
          "additionalProperties": { "type": "string" }
        },
        "services": {
          "type": "array",
          "items": { "type": "string" }
        }
      }
    },

    "dependencies": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/dependency"
      }
    },

    "metrics": {
      "$ref": "#/definitions/metrics"
    },

    "documentation": {
      "$ref": "#/definitions/documentation"
    },

    "timestamps": {
      "type": "object",
      "properties": {
        "created_at": { "type": "string", "format": "date-time" },
        "updated_at": { "type": "string", "format": "date-time" },
        "last_release_at": { "type": "string", "format": "date-time" },
        "indexed_at": { "type": "string", "format": "date-time" }
      }
    },

    "extended": {
      "type": "object",
      "description": "Ecosystem-specific data"
    }
  },

  "definitions": {
    "dependency": {
      "type": "object",
      "required": ["target_id", "type"],
      "properties": {
        "target_id": { "type": "string" },
        "target_name": { "type": "string" },
        "type": {
          "type": "string",
          "enum": ["required", "optional", "dev", "peer", "build"]
        },
        "version_constraint": { "type": "string" }
      }
    },

    "metrics": {
      "type": "object",
      "properties": {
        "popularity": {
          "type": "object",
          "properties": {
            "downloads_total": { "type": "integer", "minimum": 0 },
            "downloads_weekly": { "type": "integer", "minimum": 0 },
            "downloads_daily": { "type": "integer", "minimum": 0 },
            "active_installs": { "type": "integer", "minimum": 0 },
            "stars": { "type": "integer", "minimum": 0 },
            "watchers": { "type": "integer", "minimum": 0 },
            "forks": { "type": "integer", "minimum": 0 }
          }
        },

        "quality": {
          "type": "object",
          "properties": {
            "rating": {
              "type": "number",
              "minimum": 0,
              "maximum": 5
            },
            "rating_count": { "type": "integer", "minimum": 0 },
            "health_score": {
              "type": "integer",
              "minimum": 0,
              "maximum": 100
            },
            "quality_score": {
              "type": "integer",
              "minimum": 0,
              "maximum": 100
            },
            "security_score": {
              "type": "integer",
              "minimum": 0,
              "maximum": 100
            }
          }
        },

        "activity": {
          "type": "object",
          "properties": {
            "contributors_count": { "type": "integer", "minimum": 0 },
            "commits_total": { "type": "integer", "minimum": 0 },
            "commits_monthly": { "type": "integer", "minimum": 0 },
            "releases_count": { "type": "integer", "minimum": 0 },
            "issues_open": { "type": "integer", "minimum": 0 },
            "issues_closed": { "type": "integer", "minimum": 0 },
            "prs_open": { "type": "integer", "minimum": 0 },
            "prs_merged": { "type": "integer", "minimum": 0 },
            "last_commit_at": { "type": "string", "format": "date-time" }
          }
        },

        "support": {
          "type": "object",
          "properties": {
            "response_time_hours": { "type": "number", "minimum": 0 },
            "resolution_rate": {
              "type": "number",
              "minimum": 0,
              "maximum": 1
            },
            "has_paid_support": { "type": "boolean" },
            "support_channels": {
              "type": "array",
              "items": { "type": "string" }
            }
          }
        }
      }
    },

    "documentation": {
      "type": "object",
      "properties": {
        "readme": { "type": "string" },
        "readme_quality_score": {
          "type": "integer",
          "minimum": 0,
          "maximum": 100
        },
        "has_api_docs": { "type": "boolean" },
        "has_examples": { "type": "boolean" },
        "has_changelog": { "type": "boolean" },
        "has_contributing": { "type": "boolean" },
        "languages": {
          "type": "array",
          "items": { "type": "string" }
        },
        "tutorials_count": { "type": "integer", "minimum": 0 }
      }
    }
  }
}
```

### 4.2.2 JSON Schema для кластера

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "https://bios-i.io/schemas/cluster.json",
  "title": "Cluster",
  "description": "Schema for component clusters",
  "type": "object",

  "required": [
    "id",
    "slug",
    "name",
    "category",
    "components"
  ],

  "properties": {
    "id": {
      "type": "string",
      "format": "uuid"
    },

    "slug": {
      "type": "string",
      "pattern": "^[a-z0-9-]+$"
    },

    "name": {
      "type": "string",
      "minLength": 1,
      "maxLength": 200
    },

    "description": {
      "type": "string",
      "maxLength": 5000
    },

    "category": {
      "type": "string",
      "enum": [
        "ecommerce",
        "marketing",
        "analytics",
        "crm",
        "cms",
        "devops",
        "security",
        "productivity",
        "communication",
        "finance",
        "hr",
        "support",
        "education",
        "healthcare",
        "logistics",
        "manufacturing"
      ]
    },

    "subcategory": {
      "type": "string"
    },

    "tags": {
      "type": "array",
      "items": { "type": "string" }
    },

    "target_platforms": {
      "type": "array",
      "items": {
        "type": "string",
        "enum": ["wordpress", "shopify", "saas", "self-hosted", "docker", "kubernetes"]
      }
    },

    "components": {
      "type": "array",
      "minItems": 2,
      "items": {
        "$ref": "#/definitions/cluster_component"
      }
    },

    "configuration": {
      "$ref": "#/definitions/configuration"
    },

    "scripts": {
      "$ref": "#/definitions/scripts"
    },

    "internal_connections": {
      "type": "array",
      "items": {
        "$ref": "#/definitions/connection"
      }
    },

    "metadata": {
      "type": "object",
      "properties": {
        "author": {
          "type": "object",
          "properties": {
            "id": { "type": "string" },
            "name": { "type": "string" },
            "verified": { "type": "boolean" }
          }
        },
        "version": { "type": "string" },
        "license": { "type": "string" },
        "is_verified": { "type": "boolean" },
        "is_featured": { "type": "boolean" },
        "created_at": { "type": "string", "format": "date-time" },
        "updated_at": { "type": "string", "format": "date-time" }
      }
    },

    "metrics": {
      "type": "object",
      "properties": {
        "installs_count": { "type": "integer", "minimum": 0 },
        "rating": { "type": "number", "minimum": 0, "maximum": 5 },
        "rating_count": { "type": "integer", "minimum": 0 },
        "success_rate": { "type": "number", "minimum": 0, "maximum": 1 },
        "avg_deploy_time_seconds": { "type": "integer", "minimum": 0 }
      }
    },

    "pricing": {
      "type": "object",
      "properties": {
        "type": {
          "type": "string",
          "enum": ["free", "freemium", "paid", "subscription"]
        },
        "price_usd": { "type": "number", "minimum": 0 },
        "billing_period": {
          "type": "string",
          "enum": ["once", "monthly", "yearly"]
        },
        "components_breakdown": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "component_id": { "type": "string" },
              "price_usd": { "type": "number" },
              "required": { "type": "boolean" }
            }
          }
        }
      }
    }
  },

  "definitions": {
    "cluster_component": {
      "type": "object",
      "required": ["component_id", "role"],
      "properties": {
        "component_id": { "type": "string" },
        "role": {
          "type": "string",
          "enum": ["core", "integration", "enhancement", "utility", "monitoring", "optional"]
        },
        "required": { "type": "boolean", "default": true },
        "config": { "type": "object" },
        "alternatives": {
          "type": "array",
          "items": { "type": "string" }
        },
        "version_constraint": { "type": "string" }
      }
    },

    "configuration": {
      "type": "object",
      "properties": {
        "requirements": {
          "type": "object",
          "properties": {
            "min_memory": { "type": "string" },
            "min_disk": { "type": "string" },
            "min_cpu": { "type": "string" },
            "platforms": { "type": "array", "items": { "type": "string" } },
            "services": { "type": "array", "items": { "type": "string" } },
            "runtime": { "type": "object" }
          }
        },
        "variables": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/config_variable"
          }
        },
        "validation_rules": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "rule": { "type": "string" },
              "message": { "type": "string" }
            }
          }
        }
      }
    },

    "config_variable": {
      "type": "object",
      "required": ["name", "type"],
      "properties": {
        "name": { "type": "string" },
        "label": { "type": "string" },
        "description": { "type": "string" },
        "type": {
          "type": "string",
          "enum": ["string", "number", "boolean", "select", "multiselect", "secret", "url", "email"]
        },
        "required": { "type": "boolean", "default": false },
        "default": {},
        "options": {
          "type": "array",
          "items": {
            "type": "object",
            "properties": {
              "value": {},
              "label": { "type": "string" }
            }
          }
        },
        "pattern": { "type": "string" },
        "min": { "type": "number" },
        "max": { "type": "number" }
      }
    },

    "scripts": {
      "type": "object",
      "properties": {
        "pre_install": { "type": "string" },
        "install": { "type": "string" },
        "post_install": { "type": "string" },
        "configure": { "type": "string" },
        "health_check": { "type": "string" },
        "pre_uninstall": { "type": "string" },
        "uninstall": { "type": "string" },
        "upgrade": { "type": "string" }
      }
    },

    "connection": {
      "type": "object",
      "required": ["source", "target", "type"],
      "properties": {
        "source": { "type": "string" },
        "target": { "type": "string" },
        "type": {
          "type": "string",
          "enum": ["data_flow", "api_call", "webhook", "shared_db", "event"]
        },
        "config": { "type": "object" }
      }
    }
  }
}
```

---

## 4.3 Алгоритмы обогащения данных

### 4.3.1 Вычисление Health Score

```python
from dataclasses import dataclass
from datetime import datetime, timedelta
from typing import Optional

@dataclass
class HealthScoreInput:
    last_updated: Optional[datetime]
    last_commit: Optional[datetime]
    issues_open: int
    issues_closed: int
    support_threads: int
    support_resolved: int
    rating: Optional[float]
    rating_count: int
    downloads_trend: float  # % изменение за месяц
    has_active_maintainer: bool

def calculate_health_score(input: HealthScoreInput) -> int:
    """
    Вычисление Health Score (0-100) на основе множества факторов.

    Формула:
    HS = w1*Activity + w2*Support + w3*Trend + w4*Reputation + w5*Maintenance

    где веса: w1=0.25, w2=0.20, w3=0.15, w4=0.20, w5=0.20
    """

    # 1. Activity Score (0-100)
    activity_score = calculate_activity_score(
        input.last_updated,
        input.last_commit
    )

    # 2. Support Score (0-100)
    support_score = calculate_support_score(
        input.issues_open,
        input.issues_closed,
        input.support_threads,
        input.support_resolved
    )

    # 3. Trend Score (0-100)
    trend_score = calculate_trend_score(input.downloads_trend)

    # 4. Reputation Score (0-100)
    reputation_score = calculate_reputation_score(
        input.rating,
        input.rating_count
    )

    # 5. Maintenance Score (0-100)
    maintenance_score = 100 if input.has_active_maintainer else 30

    # Взвешенное среднее
    health_score = (
        0.25 * activity_score +
        0.20 * support_score +
        0.15 * trend_score +
        0.20 * reputation_score +
        0.20 * maintenance_score
    )

    return int(min(100, max(0, health_score)))


def calculate_activity_score(
    last_updated: Optional[datetime],
    last_commit: Optional[datetime]
) -> float:
    """
    Оценка активности разработки.

    Шкала:
    - Обновление < 1 месяц: 100
    - Обновление < 3 месяца: 80
    - Обновление < 6 месяцев: 60
    - Обновление < 1 год: 40
    - Обновление < 2 года: 20
    - Обновление > 2 года: 0
    """
    now = datetime.utcnow()

    # Берём более свежую дату
    latest = max(
        last_updated or datetime.min,
        last_commit or datetime.min
    )

    if latest == datetime.min:
        return 0

    days_since_update = (now - latest).days

    if days_since_update < 30:
        return 100
    elif days_since_update < 90:
        return 80
    elif days_since_update < 180:
        return 60
    elif days_since_update < 365:
        return 40
    elif days_since_update < 730:
        return 20
    else:
        return 0


def calculate_support_score(
    issues_open: int,
    issues_closed: int,
    support_threads: int,
    support_resolved: int
) -> float:
    """
    Оценка качества поддержки.

    Метрики:
    - Issue resolution rate
    - Support resolution rate
    - Issue backlog ratio
    """
    # Issue resolution rate
    total_issues = issues_open + issues_closed
    if total_issues > 0:
        issue_resolution_rate = issues_closed / total_issues
    else:
        issue_resolution_rate = 1.0  # нет issues = хорошо

    # Support resolution rate
    if support_threads > 0:
        support_resolution_rate = support_resolved / support_threads
    else:
        support_resolution_rate = 1.0

    # Issue backlog penalty
    # Много открытых issues = плохо
    backlog_penalty = min(1.0, issues_open / 100) * 20

    score = (
        issue_resolution_rate * 40 +
        support_resolution_rate * 40 +
        20 - backlog_penalty
    )

    return score


def calculate_trend_score(downloads_trend: float) -> float:
    """
    Оценка тренда популярности.

    downloads_trend: процент изменения за месяц
    - >50%: 100
    - >20%: 80
    - >0%: 60
    - >-20%: 40
    - >-50%: 20
    - <-50%: 0
    """
    if downloads_trend > 50:
        return 100
    elif downloads_trend > 20:
        return 80
    elif downloads_trend > 0:
        return 60
    elif downloads_trend > -20:
        return 40
    elif downloads_trend > -50:
        return 20
    else:
        return 0


def calculate_reputation_score(
    rating: Optional[float],
    rating_count: int
) -> float:
    """
    Оценка репутации на основе рейтинга.

    Учитывает:
    - Средний рейтинг
    - Количество оценок (confidence)

    Формула Bayesian Average:
    BA = (C * m + Σ(ratings)) / (C + n)

    где:
    C = confidence factor (например, 10)
    m = prior mean (например, 3.0)
    n = количество оценок
    """
    if rating is None or rating_count == 0:
        return 50  # нейтральная оценка

    # Bayesian average
    C = 10  # confidence factor
    m = 3.0  # prior mean

    bayesian_rating = (C * m + rating * rating_count) / (C + rating_count)

    # Нормализация в 0-100
    # rating 1-5 -> score 0-100
    score = (bayesian_rating - 1) / 4 * 100

    return score
```

### 4.3.2 Вычисление Quality Score

```python
from dataclasses import dataclass
from typing import List, Optional

@dataclass
class QualityScoreInput:
    # Документация
    has_readme: bool
    readme_length: int
    has_api_docs: bool
    has_examples: bool
    has_changelog: bool
    has_contributing: bool

    # Тестирование
    has_tests: bool
    test_coverage: Optional[float]  # 0-1

    # CI/CD
    has_ci: bool
    ci_passing: Optional[bool]

    # Код
    has_types: bool  # TypeScript, Python type hints
    has_linting: bool
    code_style: Optional[str]  # eslint, prettier, black

    # Безопасность
    has_security_policy: bool
    known_vulnerabilities: int

    # Лицензия
    has_license: bool
    license_type: Optional[str]

def calculate_quality_score(input: QualityScoreInput) -> int:
    """
    Вычисление Quality Score (0-100).

    Категории:
    - Documentation: 25%
    - Testing: 25%
    - Code Quality: 25%
    - Security: 15%
    - Legal: 10%
    """

    # Documentation Score (0-25)
    doc_score = calculate_documentation_score(input)

    # Testing Score (0-25)
    test_score = calculate_testing_score(input)

    # Code Quality Score (0-25)
    code_score = calculate_code_quality_score(input)

    # Security Score (0-15)
    security_score = calculate_security_score(input)

    # Legal Score (0-10)
    legal_score = calculate_legal_score(input)

    total = doc_score + test_score + code_score + security_score + legal_score

    return int(min(100, max(0, total)))


def calculate_documentation_score(input: QualityScoreInput) -> float:
    """Оценка документации (0-25)"""
    score = 0

    # README
    if input.has_readme:
        score += 5
        # Бонус за длинный README (>1000 символов)
        if input.readme_length > 1000:
            score += 3
        elif input.readme_length > 500:
            score += 1

    # API docs
    if input.has_api_docs:
        score += 6

    # Examples
    if input.has_examples:
        score += 5

    # Changelog
    if input.has_changelog:
        score += 3

    # Contributing guide
    if input.has_contributing:
        score += 3

    return min(25, score)


def calculate_testing_score(input: QualityScoreInput) -> float:
    """Оценка тестирования (0-25)"""
    score = 0

    if input.has_tests:
        score += 10

        # Coverage bonus
        if input.test_coverage is not None:
            if input.test_coverage >= 0.80:
                score += 10
            elif input.test_coverage >= 0.60:
                score += 7
            elif input.test_coverage >= 0.40:
                score += 4
            else:
                score += 2

    # CI bonus
    if input.has_ci:
        score += 3
        if input.ci_passing:
            score += 2

    return min(25, score)


def calculate_code_quality_score(input: QualityScoreInput) -> float:
    """Оценка качества кода (0-25)"""
    score = 0

    # Type annotations
    if input.has_types:
        score += 10

    # Linting
    if input.has_linting:
        score += 8

    # Code style
    if input.code_style:
        score += 7

    return min(25, score)


def calculate_security_score(input: QualityScoreInput) -> float:
    """Оценка безопасности (0-15)"""
    score = 15  # начинаем с максимума

    # Штраф за уязвимости
    if input.known_vulnerabilities > 0:
        penalty = min(10, input.known_vulnerabilities * 3)
        score -= penalty

    # Бонус за security policy
    if input.has_security_policy:
        score += 0  # уже учтено в базовом
    else:
        score -= 3

    return max(0, score)


def calculate_legal_score(input: QualityScoreInput) -> float:
    """Оценка легальности (0-10)"""
    if not input.has_license:
        return 0

    # Оценка по типу лицензии
    permissive = ['MIT', 'Apache-2.0', 'BSD-3-Clause', 'BSD-2-Clause', 'ISC']
    copyleft = ['GPL-3.0', 'GPL-2.0', 'AGPL-3.0', 'LGPL-3.0']

    if input.license_type in permissive:
        return 10
    elif input.license_type in copyleft:
        return 8
    else:
        return 6
```

### 4.3.3 Определение совместимости

```python
from dataclasses import dataclass
from typing import List, Dict, Tuple
from enum import Enum

class CompatibilitySource(Enum):
    EXPLICIT = "explicit"  # Заявлено разработчиком
    TESTED = "tested"  # Автоматически протестировано
    INFERRED = "inferred"  # Выведено из данных
    USER_REPORTED = "user_reported"  # Сообщено пользователями
    CO_INSTALL = "co_install"  # Анализ совместных установок

@dataclass
class CompatibilityResult:
    component_a: str
    component_b: str
    score: float  # 0-1
    confidence: float  # 0-1
    source: CompatibilitySource
    details: Dict
    issues: List[str]
    recommendations: List[str]

class CompatibilityAnalyzer:
    def __init__(self, graph_db, catalog_db):
        self.graph_db = graph_db
        self.catalog_db = catalog_db

    async def analyze(
        self,
        component_a_id: str,
        component_b_id: str
    ) -> CompatibilityResult:
        """
        Анализ совместимости двух компонентов.

        Алгоритм:
        1. Проверка явных данных (заявлено разработчиком)
        2. Проверка результатов автотестов
        3. Анализ пользовательских отчётов
        4. Анализ co-installation data
        5. Вывод на основе зависимостей
        """

        # 1. Явные данные
        explicit = await self._check_explicit_compatibility(
            component_a_id, component_b_id
        )
        if explicit:
            return explicit

        # 2. Результаты тестов
        tested = await self._check_tested_compatibility(
            component_a_id, component_b_id
        )
        if tested:
            return tested

        # 3. Пользовательские отчёты
        user_reports = await self._analyze_user_reports(
            component_a_id, component_b_id
        )

        # 4. Co-installation analysis
        co_install = await self._analyze_co_installations(
            component_a_id, component_b_id
        )

        # 5. Inference from dependencies
        inferred = await self._infer_from_dependencies(
            component_a_id, component_b_id
        )

        # Комбинирование результатов
        return self._combine_results(
            user_reports, co_install, inferred
        )

    async def _check_explicit_compatibility(
        self,
        a_id: str,
        b_id: str
    ) -> Optional[CompatibilityResult]:
        """Проверка явно заявленной совместимости"""

        query = """
        MATCH (a:Component {id: $a_id})-[r:COMPATIBLE_WITH]-(b:Component {id: $b_id})
        RETURN r.score as score, r.source as source, r.details as details
        """

        result = await self.graph_db.run(query, a_id=a_id, b_id=b_id)

        if result:
            return CompatibilityResult(
                component_a=a_id,
                component_b=b_id,
                score=result['score'],
                confidence=1.0,  # Явные данные = высокая уверенность
                source=CompatibilitySource.EXPLICIT,
                details=result['details'],
                issues=[],
                recommendations=[]
            )

        return None

    async def _analyze_co_installations(
        self,
        a_id: str,
        b_id: str
    ) -> Dict:
        """
        Анализ совместных установок.

        Логика:
        - Если A и B часто устанавливают вместе -> вероятно совместимы
        - Если A и B редко устанавливают вместе -> неизвестно
        - Если A устанавливают, но B удаляют после -> несовместимы
        """

        query = """
        // Сколько пользователей установили оба
        MATCH (u:User)-[:INSTALLED]->(a:Component {id: $a_id})
        MATCH (u)-[:INSTALLED]->(b:Component {id: $b_id})
        WITH count(DISTINCT u) as both_installed

        // Сколько установили A
        MATCH (u2:User)-[:INSTALLED]->(a2:Component {id: $a_id})
        WITH both_installed, count(DISTINCT u2) as a_installed

        // Сколько установили B
        MATCH (u3:User)-[:INSTALLED]->(b2:Component {id: $b_id})
        WITH both_installed, a_installed, count(DISTINCT u3) as b_installed

        RETURN both_installed, a_installed, b_installed,
               toFloat(both_installed) / toFloat(a_installed) as ratio_from_a,
               toFloat(both_installed) / toFloat(b_installed) as ratio_from_b
        """

        result = await self.graph_db.run(query, a_id=a_id, b_id=b_id)

        if result['both_installed'] < 10:
            # Недостаточно данных
            return {
                'score': 0.5,
                'confidence': 0.1,
                'reason': 'insufficient_data'
            }

        # Lift = (A&B) / (P(A) * P(B))
        # Если > 1 -> чаще вместе, чем случайно
        lift = result['both_installed'] / (
            result['a_installed'] * result['b_installed'] / 1000000
        )

        if lift > 2:
            score = 0.9
        elif lift > 1.5:
            score = 0.8
        elif lift > 1:
            score = 0.7
        elif lift > 0.5:
            score = 0.5
        else:
            score = 0.3

        confidence = min(1.0, result['both_installed'] / 100)

        return {
            'score': score,
            'confidence': confidence,
            'lift': lift,
            'both_installed': result['both_installed']
        }

    async def _infer_from_dependencies(
        self,
        a_id: str,
        b_id: str
    ) -> Dict:
        """
        Вывод совместимости из зависимостей.

        Правила:
        - Общие зависимости -> вероятно совместимы
        - Конфликтующие версии зависимостей -> несовместимы
        - Один зависит от другого -> совместимы
        """

        # Проверка прямой зависимости
        query = """
        MATCH (a:Component {id: $a_id})-[:DEPENDS_ON]->(b:Component {id: $b_id})
        RETURN true as depends
        UNION
        MATCH (b:Component {id: $b_id})-[:DEPENDS_ON]->(a:Component {id: $a_id})
        RETURN true as depends
        """

        direct_dep = await self.graph_db.run(query, a_id=a_id, b_id=b_id)

        if direct_dep:
            return {
                'score': 0.95,
                'confidence': 0.9,
                'reason': 'direct_dependency'
            }

        # Проверка общих зависимостей
        query = """
        MATCH (a:Component {id: $a_id})-[:DEPENDS_ON]->(common)<-[:DEPENDS_ON]-(b:Component {id: $b_id})
        RETURN count(common) as shared_deps, collect(common.name) as dep_names
        """

        shared = await self.graph_db.run(query, a_id=a_id, b_id=b_id)

        if shared['shared_deps'] > 5:
            return {
                'score': 0.8,
                'confidence': 0.6,
                'reason': 'many_shared_dependencies',
                'shared_count': shared['shared_deps']
            }
        elif shared['shared_deps'] > 0:
            return {
                'score': 0.7,
                'confidence': 0.4,
                'reason': 'some_shared_dependencies',
                'shared_count': shared['shared_deps']
            }

        return {
            'score': 0.5,
            'confidence': 0.2,
            'reason': 'no_relationship_found'
        }

    def _combine_results(
        self,
        user_reports: Dict,
        co_install: Dict,
        inferred: Dict
    ) -> CompatibilityResult:
        """Комбинирование результатов из разных источников"""

        # Взвешенное среднее с учётом confidence
        total_weight = (
            user_reports['confidence'] * 2 +  # Пользователи важнее
            co_install['confidence'] +
            inferred['confidence']
        )

        if total_weight == 0:
            final_score = 0.5
            final_confidence = 0.1
        else:
            final_score = (
                user_reports['score'] * user_reports['confidence'] * 2 +
                co_install['score'] * co_install['confidence'] +
                inferred['score'] * inferred['confidence']
            ) / total_weight

            final_confidence = min(1.0, total_weight / 3)

        return CompatibilityResult(
            component_a=a_id,
            component_b=b_id,
            score=final_score,
            confidence=final_confidence,
            source=CompatibilitySource.INFERRED,
            details={
                'user_reports': user_reports,
                'co_install': co_install,
                'inferred': inferred
            },
            issues=[],
            recommendations=[]
        )
```

---

## 4.4 API каталога

### 4.4.1 REST API Endpoints

```yaml
openapi: 3.0.0
info:
  title: BIOS-I Catalog API
  version: 1.0.0

paths:
  # ============ COMPONENTS ============

  /v1/components:
    get:
      summary: List components
      tags: [Components]
      parameters:
        - name: ecosystem
          in: query
          schema:
            type: array
            items:
              type: string
        - name: type
          in: query
          schema:
            type: string
        - name: category
          in: query
          schema:
            type: string
        - name: tags
          in: query
          schema:
            type: array
            items:
              type: string
        - name: min_health_score
          in: query
          schema:
            type: integer
        - name: min_quality_score
          in: query
          schema:
            type: integer
        - name: sort
          in: query
          schema:
            type: string
            enum: [downloads, rating, health_score, updated_at]
        - name: order
          in: query
          schema:
            type: string
            enum: [asc, desc]
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
            maximum: 100
        - name: offset
          in: query
          schema:
            type: integer
            default: 0
      responses:
        '200':
          description: List of components
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/ComponentSummary'
                  pagination:
                    $ref: '#/components/schemas/Pagination'

  /v1/components/{id}:
    get:
      summary: Get component by ID
      tags: [Components]
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
        - name: include
          in: query
          description: Additional data to include
          schema:
            type: array
            items:
              type: string
              enum: [metrics, documentation, dependencies, compatibility]
      responses:
        '200':
          description: Component details
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Component'
        '404':
          description: Component not found

  /v1/components/{id}/similar:
    get:
      summary: Get similar components
      tags: [Components]
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
      responses:
        '200':
          description: Similar components
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    component:
                      $ref: '#/components/schemas/ComponentSummary'
                    similarity_score:
                      type: number

  /v1/components/{id}/compatibility:
    get:
      summary: Get compatibility information
      tags: [Components]
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
        - name: with
          in: query
          description: Component IDs to check compatibility with
          schema:
            type: array
            items:
              type: string
      responses:
        '200':
          description: Compatibility information
          content:
            application/json:
              schema:
                type: object
                properties:
                  compatible_with:
                    type: array
                    items:
                      $ref: '#/components/schemas/CompatibilityInfo'
                  incompatible_with:
                    type: array
                    items:
                      $ref: '#/components/schemas/CompatibilityInfo'

  # ============ SEARCH ============

  /v1/search:
    get:
      summary: Semantic search
      tags: [Search]
      parameters:
        - name: q
          in: query
          required: true
          description: Search query
          schema:
            type: string
        - name: ecosystem
          in: query
          schema:
            type: array
            items:
              type: string
        - name: type
          in: query
          schema:
            type: string
        - name: min_health_score
          in: query
          schema:
            type: integer
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Search results
          content:
            application/json:
              schema:
                type: object
                properties:
                  results:
                    type: array
                    items:
                      type: object
                      properties:
                        component:
                          $ref: '#/components/schemas/ComponentSummary'
                        relevance_score:
                          type: number
                        highlights:
                          type: object
                  suggestions:
                    type: array
                    items:
                      type: string
                  facets:
                    type: object

  /v1/search/autocomplete:
    get:
      summary: Search autocomplete
      tags: [Search]
      parameters:
        - name: q
          in: query
          required: true
          schema:
            type: string
        - name: limit
          in: query
          schema:
            type: integer
            default: 10
      responses:
        '200':
          description: Autocomplete suggestions
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    text:
                      type: string
                    type:
                      type: string
                      enum: [component, category, tag, cluster]
                    id:
                      type: string

  # ============ RECOMMENDATIONS ============

  /v1/recommendations:
    post:
      summary: Get recommendations based on context
      tags: [Recommendations]
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              properties:
                installed_components:
                  type: array
                  items:
                    type: string
                platform:
                  type: string
                use_case:
                  type: string
                budget:
                  type: string
                  enum: [free, low, medium, high]
      responses:
        '200':
          description: Recommendations
          content:
            application/json:
              schema:
                type: object
                properties:
                  recommended_components:
                    type: array
                    items:
                      type: object
                      properties:
                        component:
                          $ref: '#/components/schemas/ComponentSummary'
                        reason:
                          type: string
                        confidence:
                          type: number
                  recommended_clusters:
                    type: array
                    items:
                      type: object
                      properties:
                        cluster:
                          $ref: '#/components/schemas/ClusterSummary'
                        match_score:
                          type: number

  # ============ GRAPH ============

  /v1/graph/paths:
    get:
      summary: Find paths between components
      tags: [Graph]
      parameters:
        - name: from
          in: query
          required: true
          schema:
            type: string
        - name: to
          in: query
          required: true
          schema:
            type: string
        - name: max_depth
          in: query
          schema:
            type: integer
            default: 3
      responses:
        '200':
          description: Paths between components
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    path:
                      type: array
                      items:
                        type: string
                    relationships:
                      type: array
                      items:
                        type: string

  /v1/graph/neighbors:
    get:
      summary: Get component neighbors in graph
      tags: [Graph]
      parameters:
        - name: id
          in: query
          required: true
          schema:
            type: string
        - name: relationship
          in: query
          schema:
            type: string
            enum: [DEPENDS_ON, INTEGRATES_WITH, SIMILAR_TO, COMPATIBLE_WITH]
        - name: direction
          in: query
          schema:
            type: string
            enum: [incoming, outgoing, both]
        - name: limit
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: Neighboring components
          content:
            application/json:
              schema:
                type: array
                items:
                  type: object
                  properties:
                    component:
                      $ref: '#/components/schemas/ComponentSummary'
                    relationship:
                      type: string
                    weight:
                      type: number

components:
  schemas:
    ComponentSummary:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        ecosystem:
          type: string
        type:
          type: string
        short_description:
          type: string
        health_score:
          type: integer
        quality_score:
          type: integer
        downloads_weekly:
          type: integer
        rating:
          type: number

    Component:
      allOf:
        - $ref: '#/components/schemas/ComponentSummary'
        - type: object
          properties:
            description:
              type: string
            categories:
              type: array
              items:
                type: string
            tags:
              type: array
              items:
                type: string
            urls:
              type: object
            author:
              type: object
            license:
              type: object
            version:
              type: object
            metrics:
              type: object
            documentation:
              type: object
            dependencies:
              type: array
            timestamps:
              type: object

    CompatibilityInfo:
      type: object
      properties:
        component_id:
          type: string
        component_name:
          type: string
        score:
          type: number
        confidence:
          type: number
        source:
          type: string
        issues:
          type: array
          items:
            type: string

    ClusterSummary:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        category:
          type: string
        components_count:
          type: integer
        rating:
          type: number
        installs_count:
          type: integer

    Pagination:
      type: object
      properties:
        total:
          type: integer
        limit:
          type: integer
        offset:
          type: integer
        has_more:
          type: boolean
```

---

## 4.5 Выводы раздела

### Ключевые компоненты системы каталогизации

1. **Унифицированная схема данных** позволяет хранить компоненты из любой экосистемы в едином формате

2. **Алгоритмы обогащения** вычисляют метрики качества и здоровья, которых нет в исходных данных

3. **Анализ совместимости** комбинирует данные из множества источников для определения, какие компоненты работают вместе

4. **REST API** обеспечивает доступ к каталогу для всех клиентов (web, cli, IDE)

### Масштабы данных

| Экосистема | Компонентов | Обновление |
|------------|-------------|------------|
| WordPress | ~60,000 | Ежедневно |
| npm | ~2,000,000 | Ежечасно |
| PyPI | ~400,000 | Ежедневно |
| GitHub | ~50,000,000 (активных) | Еженедельно |
| Make.com | ~1,500 | Ежедневно |

→ [Раздел 5: Система кластеров](../05-clusters/README.md)
