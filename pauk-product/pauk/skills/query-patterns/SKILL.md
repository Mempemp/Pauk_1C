---
name: query-patterns
description: >-
  Паттерны запросов 1С: без запросов в цикле, параметры внутри виртуальных таблиц регистров,
  временные таблицы и индексы, пакет через ТаблицаЗначений, ЕСТЬNULL при LEFT JOIN,
  ВЫРАЗИТЬ для составных типов, осознанный выбор полей. При объекте Запрос, виртуальных таблицах
  (Остатки, Обороты, СрезПоследних), отчётах и массовых выборках.
---

# Паттерны запросов 1С

## Когда применять

Применяй при работе с **запросами к базе** (объект `Запрос`), **виртуальными таблицами** регистров, **выборками для обработки**, **отчётами и списками**, **массовой обработке** таблиц данных из запроса. Типичные **антипаттерны текста запроса** (цикл с запросом, подзапрос в полях, фильтр виртуальной таблицы только в `ГДЕ`, отсутствие `ПЕРВЫЕ N` где нужен предел) — в правилах и чеклисте этого навыка.

## Критический принцип

Каждое выполнение запроса — обращение к СУБД с накладными расходами. **Запрос внутри цикла по элементам данных** с тем же смыслом, что можно собрать одним запросом и кэшем в памяти, — типичный источник катастрофической деградации. Правило по умолчанию: **один запрос и последующий обход в памяти** вместо N запросов.

## Быстрая справка: приоритеты

🔴 **Критично** (производительность и масштаб):

- Правило 1: нет запросам в цикле по строкам, если данные можно получить одним запросом
- Правило 5: параметры виртуальных таблиц **внутри** скобок таблицы
- Правило 9: только нужные поля в `ВЫБРАТЬ`
- Правило 14: не злоупотреблять **подзапросом в списке полей** — предпочитать соединение или ВТ

🟡 **Важно**:

- Правило 2: временные таблицы для сложных многошаговых выборок
- Правило 6: пакет входных строк через `ТаблицаЗначений` (или аналог) одним запросом
- Правило 8: `ИНДЕКСИРОВАТЬ ПО` для ВТ в соединениях; осмысленные индексы в метаданных

🟢 **Рекомендуется** (корректность и устойчивость):

- Правило 3: параметризация, без сборки текста значениями
- Правило 4: `ЕСТЬNULL()` при `ЛЕВОЕ СОЕДИНЕНИЕ` там, где NULL ломает расчёты и фильтры
- Правило 12: `ВЫРАЗИТЬ` / `ССЫЛКА` для сужения составных типов

## Как пользоваться навыком

1. **Таблица правил** ниже — по номеру открой соответствующий файл в `reference/`.
2. **[templates.md](templates.md)** — готовые каркасы: один запрос + соответствие, пакет с ВТ, виртуальная таблица с параметрами, пакет через `ТаблицаЗначений`.
3. **[checklist-queries.md](checklist-queries.md)** — перед фиксацией изменений с запросами.
4. Общий стиль модулей BSL и пересечение с формами — навык [coding-standards/SKILL.md](../coding-standards/SKILL.md).

## Таблица правил

| # | Правило | Обоснование | Подробности |
|---|---------|-------------|-------------|
| 1 | Нет запросам в цикле по данным | N обращений к СУБД | [loops-temp-tables-params-null-virtual-tables.md](reference/loops-temp-tables-params-null-virtual-tables.md) |
| 2 | Временные таблицы для сложных запросов | Читаемость, этапы, индексы ВТ | [loops-temp-tables-params-null-virtual-tables.md](reference/loops-temp-tables-params-null-virtual-tables.md) |
| 3 | Параметризация запросов | Планы, безопасность строк | [loops-temp-tables-params-null-virtual-tables.md](reference/loops-temp-tables-params-null-virtual-tables.md) |
| 4 | `ЕСТЬNULL()` при LEFT JOIN | NULL в арифметике и `ГДЕ` | [loops-temp-tables-params-null-virtual-tables.md](reference/loops-temp-tables-params-null-virtual-tables.md) |
| 5 | Параметры виртуальных таблиц внутри | Объём вычисления ВТ | [loops-temp-tables-params-null-virtual-tables.md](reference/loops-temp-tables-params-null-virtual-tables.md) |
| 6 | Пакет через `ТаблицаЗначений` | Один запрос вместо N | [batch-cursors-indexes-select-limits.md](reference/batch-cursors-indexes-select-limits.md) |
| 7 | Выборка vs выгрузка | Память и паттерн обхода | [batch-cursors-indexes-select-limits.md](reference/batch-cursors-indexes-select-limits.md) |
| 8 | Индексация полей соединения и фильтрации | План запроса | [batch-cursors-indexes-select-limits.md](reference/batch-cursors-indexes-select-limits.md) |
| 9 | Только нужные поля | Трафик и стабильность кода | [batch-cursors-indexes-select-limits.md](reference/batch-cursors-indexes-select-limits.md) |
| 10 | `ПЕРВЫЕ N` и лимиты | Защита от огромного результата | [batch-cursors-indexes-select-limits.md](reference/batch-cursors-indexes-select-limits.md) |
| 11 | Не маскировать дубликаты `РАЗЛИЧНЫЕ` | Причина — в JOIN | [distinct-composite-types-left-join-on-where.md](reference/distinct-composite-types-left-join-on-where.md) |
| 12 | `ВЫРАЗИТЬ` для составных типов | Меньше лишних соединений | [distinct-composite-types-left-join-on-where.md](reference/distinct-composite-types-left-join-on-where.md) |
| 13 | ON vs `ГДЕ` в LEFT JOIN | Сохранить семантику LEFT | [distinct-composite-types-left-join-on-where.md](reference/distinct-composite-types-left-join-on-where.md) |
| 14 | Подзапрос в списке полей | Коррелированный подзапрос как N+1 на уровне плана | [subquery-in-select.md](reference/subquery-in-select.md) |
