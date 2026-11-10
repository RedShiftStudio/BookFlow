# BookFlow — CRM для онлайн-записи

**Коммерческая задача:** автоматизировать расписание сотрудников и запись клиентов небольшого сервисного бизнеса. Портфолио-проект; не является выполненным заказом клиента.

## Возможности

Мультитенантные организации, филиалы, услуги, сотрудники, графики и отпуска, клиенты и записи, просмотр календаря (день/неделя/месяц), статусы, аналитика, экспорт CSV, Telegram Mini App `/book/{slug}`. Сервер проверяет Telegram initData; JWT определяет роль. Защита от одновременного бронирования — serializable транзакция PostgreSQL и повторная проверка свободного времени. Напоминания отправляются через Bot API за час до записи; DEMO_MODE логирует маркированное демо-уведомление.

## Технологии и архитектура

ASP.NET Core 10, EF Core, PostgreSQL, Next.js 15, React 19, TypeScript, Tailwind 4, Docker Compose. `backend/Model.cs` — сущности и контекст; `backend/Program.cs` — API, расчёт слотов и проверяющий записи BackgroundService; `frontend/app` — CRM и интерфейс Mini App; `docs` — архитектура, API и демонстрация.

## Требования, запуск и миграции

Нужны Docker Compose, HTTPS-домен для Telegram Mini App и настроенный BotFather. `cp .env.example .env`, замените пароли и JWT_SECRET. `docker compose up -d db`; затем `docker compose run --build --rm api --migrate` и `docker compose up -d --build api web`. Для локальной работы: экспортируйте `ConnectionStrings__Db`, `JWT_SECRET`, `WEB_ORIGIN`; `cd backend && dotnet restore && dotnet build -c Release && dotnet run -- --migrate` (эта команда применяет миграции и seed); Next: `cd frontend && npm ci && npm run build`. Демо-организация и учётная запись создаются только при `DEMO_MODE=true` во время `--migrate`. Отключите DEMO_MODE в production. Миграция: `cd backend && dotnet ef migrations add NAME`.

## Переменные окружения и интеграции

`DB_PASSWORD`, `JWT_SECRET` (32+ символа), `WEB_ORIGIN`, `WEB_PORT`, `TELEGRAM_BOT_TOKEN`, `DEMO_MODE`, `DEMO_EMAIL`, `DEMO_PASSWORD`, `ConnectionStrings__Db`. Telegram Mini App URL: `https://your-domain/book/<slug>`. В production разместите HTTPS reverse proxy перед web, задайте WEB_ORIGIN и секреты через переменные окружения; не публикуйте `.env` и не используйте demo-пароль. В UI предусмотрено переключение светлой и тёмной темы.

## API

`POST /api/auth/register`, `/login`, `/telegram`; `GET/PUT /api/tenants/me`; `GET/POST /api/branches`, `/services`, `/employees`, `/customers`, `/appointments`, `/members`; `PUT /api/services/{id}`, `/schedules/{employee}`; `GET /api/availability?employee=&service=&date=`, `/schedules/{employee}`, `/analytics`, `/analytics/utilization`, `/export/appointments`; `POST /api/leaves`; `POST /api/appointments/{id}/reschedule`; `PATCH /api/appointments/{id}`. JWT требуется в приватных маршрутах. Списки записей и клиентов выдаются страницами по 50 элементов.

## Сборка и ограничения

`dotnet restore && dotnet build -c Release` в backend; `npm ci && npm run build` в frontend; `docker compose build` при наличии Docker. Сборка не является подтверждением runtime-корректности. Время слотов интерпретируется в UTC: перед применением у конкретного клиента нужно согласовать timezone организации. Для отправки реальных уведомлений требуется Bot Token. Исходники под MIT. Реальные скриншоты добавляются после законного запуска проекта: `docs/screenshots/` (изображения не подменяются макетами).

## Для портфолио

Демонстрация компетенций: календарное бронирование, многопользовательская CRM, Telegram Mini App, RBAC, PostgreSQL, Docker. Не выдавать за заказ настоящего клиента.

## Место для настоящих скриншотов

Добавьте реальные снимки приложения в `docs/screenshots/` только после запуска и получения права на их публикацию.
