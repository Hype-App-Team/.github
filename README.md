# Где Хайп? · Hype-App-Team Governance

Этот репозиторий задаёт публичный профиль организации ([`profile/README.md`](profile/README.md)) и единые инженерные стандарты для всех репозиториев `Hype-App-Team`.

## Карта репозиториев организации

| Репозиторий | Контур | Доступ | Назначение и артефакты релиза |
| --- | --- | --- | --- |
| **`hype-frontend`** | Клиентский веб и Telegram Mini App | 🔒 Private | React 19 + TypeScript + FSD (`apps/web`, `packages/core`). Публикует `hype-frontend-<tag>.tar.gz` и контейнер `ghcr.io/hype-app-team/hype-frontend`. |
| **`hype-backend`** | Продуктовый API и Telegram-бот | 🔒 Private | FastAPI (`services/api`) + `@where_is_hype_bot` (`services/bot`) + контракт `contracts/openapi.v1.json`. Публикует `ghcr.io/hype-app-team/hype-api` и `hype-bot`. |
| **`hype-ml`** | Мультимодальное ML-ядро и лаборатория | 🔒 Private | Пакет `gde-hype-ml` (`packages/runtime`), HTTP-воркер извлечения (`services/extractor`) и стенд оценки (`lab/extract-lab`). Публикует wheel `gde_hype_ml-*.whl` и `ghcr.io/hype-app-team/hype-ml-extractor`. |
| **`.github`** | Стандарты организации | 🌐 Public | Профиль команды, политика безопасности (`SECURITY.md`), правила совместной работы (`CONTRIBUTING.md`) и шаблоны PR/Issue. |

## Принципы независимой сборки и надёжности (SRE)

1. **Никаких межрепозиторных runtime-сборок из `main`:** каждый репозиторий собирается и тестируется полностью автономно. Связь между контурами идёт только через версионированные артефакты (pinned wheel + SHA256, `openapi.v1.json`, `extraction.schema.json`) и защищённые HTTP-границы.
2. **Нельзя сломать прод одним коммитом:** изменения проходят через ветки, локальные хуки `.githooks`, архитектурные линтеры, автотесты и контрактные проверки; выкатка проверяет `/health` до переключения трафика и автоматически откатывается при сбое.
3. **Единый стандарт для людей и ИИ-агентов:** в корне каждого закрытого репозитория лежат `AGENTS.md` и `make verify` — единая команда полной проверки перед любым PR.
