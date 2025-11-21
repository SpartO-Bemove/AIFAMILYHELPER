# API Endpoints (REST + GraphQL)

## 1. Auth

### REST
- `POST /auth/register`
- `POST /auth/login`
- `POST /auth/refresh`
- `POST /auth/logout`

### GraphQL
- `mutation signup(input: SignupInput): AuthPayload`
- `mutation login(input: LoginInput): AuthPayload`
- `mutation refreshToken(token: String!): AuthPayload`
- `mutation logout: Boolean`

---

## 2. Users & Families

### REST
- `GET /me` — текущий пользователь
- `PATCH /me` — обновление профиля
- `GET /families` — список семей пользователя
- `POST /families` — создать семью
- `GET /families/:id` — получить семью
- `PATCH /families/:id` — обновить семью
- `POST /families/:id/invite` — пригласить пользователя в семью
- `POST /families/join` — принять приглашение (по коду/линку)
- `GET /families/:id/members` — члены семьи
- `PATCH /families/:id/members/:memberId` — обновить роль/статус члена

### GraphQL
- `query me: User`
- `query myFamilies: [Family!]!`
- `query family(id: ID!): Family`
- `mutation createFamily(input: CreateFamilyInput): Family`
- `mutation updateFamily(id: ID!, input: UpdateFamilyInput): Family`
- `mutation inviteToFamily(input: InviteToFamilyInput): FamilyInvite`
- `mutation acceptFamilyInvite(code: String!): Family`

---

## 3. Kids

### REST
- `GET /families/:familyId/kids`
- `POST /families/:familyId/kids`
- `GET /kids/:id`
- `PATCH /kids/:id`
- `DELETE /kids/:id`
- `GET /kids/:id/medical` — медкарта, аллергии, прививки
- `PATCH /kids/:id/medical`
- `GET /kids/:id/medications`
- `POST /kids/:id/medications`
- `PATCH /kids/medications/:medId`
- `DELETE /kids/medications/:medId`
- `GET /kids/:id/day-routine`
- `PATCH /kids/:id/day-routine`

### GraphQL
- `query kids(familyId: ID!): [Kid!]!`
- `query kid(id: ID!): Kid`
- `mutation createKid(familyId: ID!, input: CreateKidInput): Kid`
- `mutation updateKid(id: ID!, input: UpdateKidInput): Kid`
- `mutation deleteKid(id: ID!): Boolean`
- `mutation updateKidMedical(id: ID!, input: KidMedicalInput): Kid`
- `mutation upsertKidRoutine(id: ID!, input: KidRoutineInput): Kid`

---

## 4. Calendar (CalendarEvent)

### REST
- `GET /families/:familyId/calendar/events`
  - фильтры: `?from=…&to=…&kidId=…`
- `POST /families/:familyId/calendar/events`
- `GET /calendar/events/:id`
- `PATCH /calendar/events/:id`
- `DELETE /calendar/events/:id`
- `POST /calendar/events/:id/reminders` — настроить напоминания

### GraphQL
- `query calendarEvents(familyId: ID!, from: DateTime!, to: DateTime!, kidId: ID): [CalendarEvent!]!`
- `query calendarEvent(id: ID!): CalendarEvent`
- `mutation createCalendarEvent(familyId: ID!, input: CreateCalendarEventInput): CalendarEvent`
- `mutation updateCalendarEvent(id: ID!, input: UpdateCalendarEventInput): CalendarEvent`
- `mutation deleteCalendarEvent(id: ID!): Boolean`

---

## 5. Tasks

### REST
- `GET /families/:familyId/tasks?status=&assigneeId=&kidId=&type=`
- `POST /families/:familyId/tasks`
- `GET /tasks/:id`
- `PATCH /tasks/:id`
- `DELETE /tasks/:id`
- `POST /tasks/:id/complete`
- `POST /tasks/:id/reopen`

### GraphQL
- `query tasks(familyId: ID!, filter: TasksFilter): [Task!]!`
- `query task(id: ID!): Task`
- `mutation createTask(familyId: ID!, input: CreateTaskInput): Task`
- `mutation updateTask(id: ID!, input: UpdateTaskInput): Task`
- `mutation completeTask(id: ID!): Task`
- `mutation reopenTask(id: ID!): Task`
- `mutation deleteTask(id: ID!): Boolean`

---

## 6. Shopping (ShoppingItem)

### REST
- `GET /families/:familyId/shopping-items?week=YYYY-WW`
- `POST /families/:familyId/shopping-items`
- `PATCH /shopping-items/:id`
- `DELETE /shopping-items/:id`
- `POST /shopping-items/:id/check` — отметить как купленное
- `POST /families/:familyId/shopping-items/generate-week-basket` — генерация корзины недели (AI)

### GraphQL
- `query shoppingItems(familyId: ID!, week: String): [ShoppingItem!]!`
- `mutation addShoppingItem(familyId: ID!, input: AddShoppingItemInput): ShoppingItem`
- `mutation updateShoppingItem(id: ID!, input: UpdateShoppingItemInput): ShoppingItem`
- `mutation deleteShoppingItem(id: ID!): Boolean`
- `mutation toggleShoppingItem(id: ID!): ShoppingItem`
- `mutation generateWeekBasket(familyId: ID!, input: WeekBasketInput): [ShoppingItem!]!`

---

## 7. Home (HomeTask)

### REST
- `GET /families/:familyId/home-tasks?active=`
- `POST /families/:familyId/home-tasks`
- `GET /home-tasks/:id`
- `PATCH /home-tasks/:id`
- `DELETE /home-tasks/:id`
- `POST /home-tasks/:id/complete` — отметить выполнение (обновить `lastDone`/`nextDue`)

### GraphQL
- `query homeTasks(familyId: ID!, filter: HomeTasksFilter): [HomeTask!]!`
- `mutation createHomeTask(familyId: ID!, input: CreateHomeTaskInput): HomeTask`
- `mutation updateHomeTask(id: ID!, input: UpdateHomeTaskInput): HomeTask`
- `mutation completeHomeTask(id: ID!): HomeTask`
- `mutation deleteHomeTask(id: ID!): Boolean`

---

## 8. Finance (Budget / Expense)

### REST
- `GET /families/:familyId/budgets?year=&month=`
- `POST /families/:familyId/budgets`
- `PATCH /budgets/:id`
- `DELETE /budgets/:id`
- `GET /families/:familyId/expenses?from=&to=&category=&kidId=`
- `POST /families/:familyId/expenses`
- `GET /expenses/:id`
- `PATCH /expenses/:id`
- `DELETE /expenses/:id`

### GraphQL
- `query budgets(familyId: ID!, filter: BudgetFilter): [Budget!]!`
- `query expenses(familyId: ID!, filter: ExpenseFilter): [Expense!]!`
- `mutation createBudget(familyId: ID!, input: CreateBudgetInput): Budget`
- `mutation updateBudget(id: ID!, input: UpdateBudgetInput): Budget`
- `mutation deleteBudget(id: ID!): Boolean`
- `mutation createExpense(familyId: ID!, input: CreateExpenseInput): Expense`
- `mutation updateExpense(id: ID!, input: UpdateExpenseInput): Expense`
- `mutation deleteExpense(id: ID!): Boolean`

---

## 9. AI Orchestrator

### REST
- `POST /ai/command` — свободный текст/голос → структурированное действие
- `POST /ai/plan-day` — сгенерировать план дня
- `POST /ai/plan-week` — сгенерировать план недели семьи
- `POST /ai/shopping-suggestions` — предложения к списку покупок

### GraphQL
- `mutation aiHandleCommand(input: AiCommandInput): AiCommandResult`
- `mutation aiPlanDay(familyId: ID!, date: DateTime!): AiPlanResult`
- `mutation aiPlanWeek(familyId: ID!, from: DateTime!, to: DateTime!): AiPlanResult`

---

## 10. Notifications

### REST
- `GET /notifications` — список уведомлений пользователя
- `PATCH /notifications/:id/read` — отметить прочитанным
- `POST /devices` — регистрация устройства для пушей

### GraphQL
- `query notifications: [Notification!]!`
- `mutation markNotificationRead(id: ID!): Notification`
- `mutation registerDevice(input: RegisterDeviceInput): Device`
