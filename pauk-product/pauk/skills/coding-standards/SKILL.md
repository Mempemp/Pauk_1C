---
name: coding-standards
description: >-
  Задаёт стиль BSL 1С по ИТС: области модулей, директивы клиент/сервер, строки и НСтр,
  именование, запросы, формы, обработка ошибок. Использовать при написании или ревью
  кода 1С, рефакторинге модулей и модулей форм, проверке перед коммитом.
---

# Стандарты кодирования BSL (1С)

## Когда применять

При любой генерации или правке кода на **BSL**. Для **ревью** или «проверь перед коммитом» сначала открой [checklist-precommit.md](checklist-precommit.md). Для **новых объектов метаданных, префиксов и маркеров типовых доработок** — [reference/project-params-and-modifications.md](reference/project-params-and-modifications.md). Для **границ слоёв, расширений и платформенных ограничений** — [reference/architecture-and-platform.md](reference/architecture-and-platform.md). Для **типичных ошибок производительности в циклах и на границе клиент—сервер** — [reference/runtime-performance-antipatterns.md](reference/runtime-performance-antipatterns.md).

## Критический принцип

Код 1С живёт годами и читается многими разработчиками. Единообразие снижает когнитивную нагрузку и число ошибок. Опирайся на стандарты **ИТС**.

## Быстрая справка: приоритеты

🔴 **Критично** (риск ошибок выполнения):

- Правило 6: не затенять глобальный контекст
- Правило 13: избегать `Выполнить()` / `Вычислить()`
- Правило 18: не проглатывать исключения

🟡 **Важно** (производительность, лишние обращения к серверу):

- Правило 3: предпочитать `&НаСервереБезКонтекста`
- Правило 7: `СтрСоединить()` вместо «+» в циклах
- Правило 19: агрегировать серверные вызовы

🟢 **Рекомендуется** (читаемость, сопровождение):

- Остальные пункты из таблицы ниже (именование, области, `НСтр()`, формы и т.д.)

## Как пользоваться навыком (прогрессивное раскрытие)

1. **Индекс ниже** — по номеру правила видно тему и файл с разбором и примерами.
2. **[templates.md](templates.md)** — заготовки общего модуля, формы, именования, строк, `НСтр()`.
3. **Модуль управляемой формы** (события, динамические списки, ТЧ, оповещения, чеклист формы) — навык [form_patterns/SKILL.md](../form_patterns/SKILL.md); директивы и правило «один вызов вместо нескольких» — в `directives-forms-cs.md` ниже.
4. **Транзакции, блокировки, исключения и журнал** — навык [error_handling/SKILL.md](../error_handling/SKILL.md); краткий запрет проглатывать исключения (правило 18) — в `performance-security-queries.md` ниже.
5. **Крупный рефакторинг** (порядок top-down / bottom-up, влияние по метаданным и коду) — [refactor_workflow/SKILL.md](../refactor_workflow/SKILL.md). **Интеграции** с внешними системами (прототип вне 1С, затем BSL) — [integration_patterns/SKILL.md](../integration_patterns/SKILL.md). **Команды в Windows PowerShell** (пути, `;` вместо `&&`, HTTP-обвязка) — [powershell_windows/SKILL.md](../powershell_windows/SKILL.md).
6. **Детали по темам** (читать выборочно):
   - [reference/structure-style-i18n.md](reference/structure-style-i18n.md) — правила 1–2, 8–12
   - [reference/directives-forms-cs.md](reference/directives-forms-cs.md) — правила 3–5, 16–17, 19
   - [reference/performance-security-queries.md](reference/performance-security-queries.md) — правила 6–7, 13–15, 18
   - [reference/project-params-and-modifications.md](reference/project-params-and-modifications.md) — `.dev.env`, маркеры доработки типового кода, именование метаданных, экспортные комментарии
   - [reference/architecture-and-platform.md](reference/architecture-and-platform.md) — слои кода, расширения, клиент-сервер, безопасность, запросы на уровне архитектуры, запахи кода
   - [reference/runtime-performance-antipatterns.md](reference/runtime-performance-antipatterns.md) — циклы, кэш, клиент-сервер, вложенные циклы поиска пар
   - справка по **встроенному API платформы** (сигнатуры, типы) — при подключённом сервере из **`pauk/routing/MCP-CATALOG.md`**: сначала **`pauk/mcp/route-<mcp_id>.md`**, затем при необходимости [README MCP](../../mcp/README.md); прочие MCP (метаданные, код, граф и т.д.) — по [README MCP](../../mcp/README.md), имена только из схемы сессии
7. **[checklist-precommit.md](checklist-precommit.md)** — чеклист перед фиксацией.

## Таблица правил

| # | Правило | Обоснование | Подробности |
|---|---------|-------------|-------------|
| 1 | CamelCase на русском | Стандарт ИТС, единообразие | structure-style-i18n |
| 2 | Структура модуля с областями | Навигация, читаемость | structure-style-i18n |
| 3 | Предпочитать `&НаСервереБезКонтекста` | Трафик клиент–сервер | directives-forms-cs |
| 4 | `ТекущаяДатаСеанса()` | Часовой пояс сеанса | directives-forms-cs |
| 5 | Сообщения пользователю с привязкой | UX | directives-forms-cs |
| 6 | Не затенять глобальный контекст | Ошибки из-за скрытия коллекций | performance-security-queries |
| 7 | `СтрСоединить()` вместо «+» в циклах | O(N) вместо O(N²) | performance-security-queries |
| 8 | Стандартные `#Область` | Навигация, анализ | structure-style-i18n |
| 9 | Комментарии «зачем», не «что» | Польза документации | structure-style-i18n |
| 10 | `НСтр()` для строк пользователю | Локализация | structure-style-i18n |
| 11 | Одна процедура — одна ответственность | Тестируемость | structure-style-i18n |
| 12 | Типизация в комментариях | Контракт API | structure-style-i18n |
| 13 | Избегать `Выполнить()` / `Вычислить()` | Безопасность, анализ | performance-security-queries |
| 14 | Нет магическим числам | Читаемость, настройка | performance-security-queries |
| 15 | Явные JOIN вместо точечной нотации | N+1, лишние чтения | performance-security-queries |
| 16 | Открывать формы через `ОткрытьФорму()` | Управляемый интерфейс | directives-forms-cs |
| 17 | Бизнес-логика не в модуле формы | Повторное использование | directives-forms-cs |
| 18 | Не проглатывать исключения | Транзакции, диагностика | performance-security-queries |
| 19 | Один серверный вызов вместо нескольких | Меньше ожидания из‑за сети и сериализации формы | directives-forms-cs |
