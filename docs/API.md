# API BookFlow
Административная авторизация `/api/auth/login` или регистрация `/api/auth/register`, Telegram `/api/auth/telegram` с `{tenant,initData}`. JWT Bearer для `/api/*`. Подробные маршруты и DTO — `backend/Program.cs`. Запись передаёт `{employeeId,serviceId,customerId,starts}`; дата и время — ISO UTC. Слоты `/api/availability` возвращают массив ISO timestamps.
