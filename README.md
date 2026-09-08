# Siabar — Modular Monolith for Pet Services Platform

**Siabar** — платформа для поиска и управления услугами в сфере
домашних животных: ветеринарные клиники, грумеры, зоонянечки, кинологи,
зоомагазины. Платформа соединяет владельцев питомцев с поставщиками услуг
через каталог, систему записей, отзывы и верификацию.

## 🎯 Цель проекта

Создать локальную (Беларусь) платформу с фокусом на:
- **Доверие:** верификация организаций, модерация отзывов, прозрачные рейтинги
- **Удобство:** запись онлайн, напоминания, медицинские карты питомцев
- **Масштабируемость:** модульный монолит с прицелом на микросервисы
- **Безопасность:** multi-tenant изоляция, RBAC, привилегии на уровне данных

## 🏗️ Архитектура

**Модульный монолит** (Modular Monolith) с разделением на:

| Слой | Назначение |
|---|---|
| **Domain** | Бизнес-сущности (Branches, Appointments, Reviews, Pets) |
| **Backend** | API, авторизация, данные, репозитории |
| **Frontend** | Angular приложения (каталог, кабинет бизнеса, профиль) |
| **Infrastructure** | БД, деплой, мониторинг |

**Ключевые паттерны:**
- **Per-module facades** (BE-10) — Unit of Work для каждого модуля
- **Two-level repositories** (BE-11) — RepositoryBase (изоляция) + SecureRepositoryBase (изоляция + привилегии)
- **Thin data providers** (BE-12) — ISP для application services
- **Event-driven architecture** (ARCH-07) — MassTransit + EF Outbox (In-Memory → RabbitMQ)
- **No-modal UX** (FE-13) — drawers, inline blocks, undo-delete

**Будущее:** извлечение модулей в микросервисы через брокер (RabbitMQ),
host-based routing (subdomains), database-per-service.

## 🛠️ Технологический стек

### Backend
- **Runtime:** .NET 10 / C# 12
- **Framework:** ASP.NET Core Web API
- **ORM:** Entity Framework Core 10
- **Database:** PostgreSQL 18+ (с PostGIS для гео-запросов)
- **Auth:** JWT + Refresh tokens, multi-tenant RBAC
- **Messaging:** MassTransit (In-Memory с EF Outbox, future: RabbitMQ)
- **Jobs:** Hangfire (лидерство через PostgreSQL advisory locks)
- **Logging:** Serilog → Seq

### Frontend
- **Framework:** Angular 22+
- **UI:** Angular Material, custom components
- **State:** RxJS + NgRx (опционально)
- **Forms:** Reactive Forms
- **HTTP:** Angular HttpClient + interceptors
- **Testing:** Jest, Cypress

### Infrastructure
- **Reverse Proxy:** Nginx (TLS termination, routing)
- **Web Server:** IIS (Windows Server)
- **CI/CD:** Azure DevOps (current) → GitHub Actions (future)
- **Monitoring:** Grafana + Prometheus (future)
- **Installer:** PowerShell-based deployment engine (INF-07)

### Documentation
- **Format:** Markdown
- **Structure:** 18 folders by domain (Architecture, Backend, Frontend, Security, Domain, Infrastructure, Future Scope)
- **Cross-references:** Explicit document IDs (ARCH-01, BE-11, etc.)

## 📚 Документация

Полная документация в папке `docs/`:

### Архитектура (`01_ARCHITECTURE/`)
- Модульный монолит и извлечение в микросервисы
- Multi-tenancy и изоляция данных
- Event-driven архитектура (MassTransit + Outbox)
- Error handling и observability

### Backend (`02_BACKEND/`)
- Паттерны доступа к данным (фасады, репозитории)
- Аутентификация и авторизация
- Тонкие провайдеры данных

### Frontend (`13_FRONTEND_APPLICATION/`)
- Структура компонентов (3 файла: .ts, .html, .scss)
- Поверхности UX (drawers, inline blocks, no-modal paradigm)
- Кабинет бизнеса (роли, привилегии, страницы)

### Безопасность (`12_SECURITY/`)
- Авторизация и tenancy
- Каталог привилегий (SEC-13)
- Защита данных (PII, гео-данные)

### Домен (`11_DOMAIN/`)
- Схема таблиц (DOM-17)
- Staff memberships и BusinessUnits (DOM-16)
- Приватность локации (DOM-18)

### Инфраструктура (`05_INFRASTRUCTURE/`)
- Deployment engine и миграции
- Nginx конфигурация
- IIS и Windows Server

### Будущее (`09_FUTURE_SCOPE/`)
- План извлечения в микросервисы
- Масштабирование (scale-out, read replicas)
- Feature flags и A/B testing

## 🚀 Текущий статус

**Фаза 0:** Модульный монолит (текущий этап)
- ✅ Архитектура и паттерны задокументированы
- ✅ Схема БД определена (DOM-17)
- ✅ Каталог привилегий (SEC-13)
- ✅ Event-driven архитектура (ARCH-07)
- ✅ UX-паттерны (FE-13)
- ✅ Реализация backend (C# / ASP.NET Core)
- 🔄 Реализация frontend (Angular)
- 🔄 Интеграционное тестирование
- ⏳ MVP (минимально жизнеспособный продукт)

**Фаза 1:** Broker swap
- ⏳ Смена In-Memory → RabbitMQ (ARCH-07)

**Фаза 2:** Первое извлечение модуля
- ⏳ Catalog или Booking → отдельный сервис
- ⏳ Host-based routing (Nginx)
- ⏳ Database-per-service

**Фаза 3:** Полная микросервисная архитектура
- ⏳ 3+ сервиса
- ⏳ Service discovery (Consul / K8s)
- ⏳ Kubernetes (опционально)

### 🔐 Доступ

Этот репозиторий является **публичным для просмотра**, но имеет **ограниченный доступ к исходному коду (Limited Access)**. Вносить изменения и скачивать определенные связанные материалы могут только официальные соавторы (*Collaborators*).

Для запроса доступа к разработке или приватным материалам свяжитесь с автором (см. контакты ниже).

## 📝 Соглашения

### Документация
- Каждый документ имеет уникальный ID (например, `BE-11`, `SEC-13`)
- Cross-references через явные ссылки на document IDs
- Addenda для эволюции без переписывания (append-only)

### Код
- Repository pattern: RepositoryBase (изоляция) + SecureRepositoryBase (изоляция + привилегии)
- Facade pattern: per-module Unit of Work
- Thin data providers: ISP для application services
- No-modal UX: drawers, inline blocks, undo-delete

### Коммиты
- Conventional Commits (feat:, fix:, docs:, refactor:, etc.)
- Each commit references document ID if applicable

## 👤 Автор

**Виктор Лахтырев**
- Email: victor.lahti@hotmail.com
- LinkedIn: https://www.linkedin.com/in/viktor-lahtyrev-08531333a/
- GitHub: https://github.com/VictorLahtyrev

Проект Siabar — соло-фаундерский проект, разрабатываемый в Могилёве, Беларусь.

## 📄 Лицензия

**Proprietary** — все права защищены. Использование, копирование, модификация
и распространение без письменного разрешения автора запрещено.

---

## 🗺️ Roadmap (детально)

### 2026 Q3-Q4: MVP (Фаза 0)
- [ ] Backend: API endpoints для каталога, записей, отзывов
- [ ] Frontend: каталог, профиль, кабинет бизнеса
- [ ] Auth: JWT + refresh tokens, multi-tenant RBAC
- [ ] Deploy: Nginx + IIS на Windows Server
- [ ] Testing: unit + integration tests
- [ ] **Goal:** Закрытое тестирование с 5-10 организациями

### 2027 Q1: Public Launch (Фаза 0 завершение)
- [ ] Верификация организаций (базовый уровень)
- [ ] Модерация отзывов
- [ ] Notifications (email, SMS)
- [ ] Mobile-responsive bottom sheet UX
- [ ] **Goal:** Публичный запуск, 50+ организаций

### 2027 Q2: Broker Swap (Фаза 1)
- [ ] MassTransit: In-Memory → RabbitMQ
- [ ] Outbox delivery validation
- [ ] Retry policies + error queues
- [ ] **Goal:** Асинхронная связность между модулями

### 2027 Q3: First Extraction (Фаза 2)
- [ ] Извлечение Catalog в отдельный сервис
- [ ] Host-based routing (catalog.domain.com)
- [ ] Database-per-service (catalog DB)
- [ ] SSO через субдомены
- [ ] **Goal:** Первый микросервис в продакшене

### 2027 Q4: Full Microservices (Фаза 3)
- [ ] Извлечение Booking, Identity, Notifications
- [ ] Service discovery (Consul / K8s)
- [ ] Kubernetes (опционально)
- [ ] **Goal:** Полная микросервисная архитектура

---