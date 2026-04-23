---
name: error_handling
description: >-
  Транзакции вручную (Начать/Зафиксировать/Отменить), блокировки данных и объектов,
  обработка исключений без «проглатывания», запись в журнал регистрации, сообщения пользователю,
  пакетная обработка с накоплением ошибок, внешние вызовы. Подключать при записи в БД,
  явных транзакциях, БлокировкаДанных и ЗаблокироватьДанныеДляРедактирования.
---

# Ошибки, транзакции и блокировки

## Когда применять

- Явные **`НачатьТранзакцию()`** и запись в базу.
- **`БлокировкаДанных`**, **`ЗаблокироватьДанныеДляРедактирования`**.
- **`Попытка` / `Исключение`**, проброс и журналирование.
- Пакетные операции и вызовы внешних систем (HTTP, файлы и т.п.).

## Связь с общими стандартами BSL

Запрет **проглатывать исключения** и краткий пример транзакции — в навыке **[coding-standards](../coding-standards/SKILL.md)** (правило **18**, файл [performance-security-queries.md](../coding-standards/reference/performance-security-queries.md)).

Этот навык даёт **канонический порядок** операций, **блокировки**, **вложенные транзакции**, **ЖР и UX**, пакеты и интеграции.

## Критический принцип

Транзакции в платформе нужно **явно** завершать фиксацией или отменой. Незакрытая или некорректно отменённая транзакция ведёт к **зависшим блокировкам** и остановке работы пользователей. Исключение без записи в журнал и без проброса **скрывает** сбой.

## Быстрая справка: приоритеты

🔴 **Критично** (целостность и блокировки):

- E2: канонический паттерн транзакции — [transactions-canonical.md](reference/transactions-canonical.md)
- E3: вложенные транзакции и `ТранзакцияАктивна()` — [transactions-nested.md](reference/transactions-nested.md)
- E1: не глотать исключения, логировать — [exceptions-journal-user.md](reference/exceptions-journal-user.md)

🟡 **Важно** (конкуренция и взаимные блокировки):

- E4: короткая транзакция — [transactions-canonical.md](reference/transactions-canonical.md)
- E5–E6, E11: управляемые блокировки, объект, единый порядок — [locks-managed-and-deadlock.md](reference/locks-managed-and-deadlock.md)

🟢 **Рекомендуется** (сопровождение и пакеты):

- E7–E9: ЖР, сообщения пользователю, `ВызватьИсключение` — [exceptions-journal-user.md](reference/exceptions-journal-user.md)
- E10, E12: массовые операции и внешние вызовы — [bulk-and-external-calls.md](reference/bulk-and-external-calls.md)

## Индекс (E = тема навыка)

| E | Тема | Файл |
|---|------|------|
| 1, 7–9 | Исключения, ЖР, пользователь, проброс | [exceptions-journal-user.md](reference/exceptions-journal-user.md) |
| 2, 4 | Канон транзакции, короткая транзакция | [transactions-canonical.md](reference/transactions-canonical.md) |
| 3 | Вложенность, `ТранзакцияАктивна()` | [transactions-nested.md](reference/transactions-nested.md) |
| 5–6, 11 | Блокировки, пессимистичная на объект, порядок | [locks-managed-and-deadlock.md](reference/locks-managed-and-deadlock.md) |
| 10, 12 | Пакет, внешние вызовы | [bulk-and-external-calls.md](reference/bulk-and-external-calls.md) |

## Шаблоны кода

[templates.md](templates.md) — каноническая транзакция, логирование в `Исключение`, блокировка перед чтением.

## Чеклист

[checklist-transactions-errors.md](checklist-transactions-errors.md) и общий [checklist-precommit.md](../coding-standards/checklist-precommit.md).

Правила написания **запросов** (в т.ч. `ЕСТЬNULL` при соединениях) — в отдельном навыке по запросам, не дублировать здесь.
