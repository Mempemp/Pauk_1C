---
name: form_patterns
description: >-
  Паттерны модуля управляемой формы: обмен между клиентом и сервером, несколько данных за один
  серверный вызов, реквизит формы в значение, обработчики и жизненный цикл, динамические списки,
  условное оформление, асинхронные диалоги, видимость, ТЧ, оповещения, типы данных при передаче
  между клиентом и сервером. Подключать при работе с модулем формы и вызовами на сервер из формы.
---

# Паттерны модуля управляемой формы

## Когда применять

- Код в **модуле управляемой формы** (клиент и сервер).
- Обработчики событий элементов и записи формы.
- Серверные вызовы из клиентского кода, **табличные части** на форме.
- Открытие форм, диалоги, **динамические списки**, условное оформление.
- Синхронизация открытых форм (**оповещения**).

## Связь с общими стандартами BSL

Общие правила стиля, директив компиляции, агрегации серверных вызовов, открытия форм и разнесения логики — в навыке **[coding-standards](../coding-standards/SKILL.md)**:

- правила **3, 4, 5, 16, 17, 19** и файл [directives-forms-cs.md](../coding-standards/reference/directives-forms-cs.md);
- общий чеклист перед коммитом: [checklist-precommit.md](../coding-standards/checklist-precommit.md).

Этот навык дополняет его **практикой модуля формы**: порядок событий, динамические списки, программное оформление, ТЧ, оповещения, какие типы значений можно передавать между клиентом и сервером.

## Критический принцип

Каждый вызов с контекстом формы **`&НаСервере`** заставляет платформу **упаковать и передать на сервер весь контекст формы** (реквизиты, таблицы на форме и т.д.) и после выполнения — **вернуть обратно**. Для формы с большой табличной частью это критично по объёму и времени. **Минимизируй** число таких вызовов; где возможно — **`&НаСервереБезКонтекста`** и передача только параметров (подробнее в `coding-standards`).

## Быстрая справка: приоритеты

🔴 **Критично** (производительность форм):

- F1–F2: вызовы без контекста формы и **один серверный вызов вместо цепочки** — см. [form-client-server-and-batching.md](reference/form-client-server-and-batching.md) и `coding-standards`
- F9: группировать изменения видимости — [visibility-tabular-notifications.md](reference/visibility-tabular-notifications.md)

🟡 **Важно** (корректность и веб-клиент):

- F3: `РеквизитФормыВЗначение` / `ЗначениеВРеквизитФормы` — [form-data-requisite-value.md](reference/form-data-requisite-value.md)
- F7–F8: асинхронные диалоги и `ОткрытьФорму` с оповещением — [async-dialogs-navigation.md](reference/async-dialogs-navigation.md)
- F12: какие типы значений можно передавать между клиентом и сервером — [serializable-types-client-server.md](reference/serializable-types-client-server.md)

🟢 **Рекомендуется**:

- F4: обработчики событий — [form-events-lifecycle.md](reference/form-events-lifecycle.md)
- F5–F6: динамические списки и условное оформление — [dynamic-lists-conditional-formatting.md](reference/dynamic-lists-conditional-formatting.md)
- F10–F11: пересчёт на клиенте, оповещения — [visibility-tabular-notifications.md](reference/visibility-tabular-notifications.md)

## Индекс тем (F = фокус навыка)

| F | Тема | Файл |
|---|------|------|
| 1–2 | Директивы и объединение данных в один вызов (напоминание + ссылки) | [form-client-server-and-batching.md](reference/form-client-server-and-batching.md) |
| 3 | Данные объекта на сервере формы: реквизит ↔ значение | [form-data-requisite-value.md](reference/form-data-requisite-value.md) |
| 4 | Порядок обработчиков открытия и записи | [form-events-lifecycle.md](reference/form-events-lifecycle.md) |
| 5–6 | Динамические списки, условное оформление | [dynamic-lists-conditional-formatting.md](reference/dynamic-lists-conditional-formatting.md) |
| 7–8 | Асинхронные диалоги, открытие форм и результат | [async-dialogs-navigation.md](reference/async-dialogs-navigation.md) |
| 9–11 | Видимость, ТЧ на форме, оповещения | [visibility-tabular-notifications.md](reference/visibility-tabular-notifications.md) |
| 12 | Типы, передаваемые между клиентом и сервером | [serializable-types-client-server.md](reference/serializable-types-client-server.md) |

## Шаблоны кода

Готовые фрагменты для копирования и адаптации — [templates.md](templates.md). Имена общих модулей в примерах (например `ОбщегоНазначения`) типичны для конфигураций на БСП; в своём проекте используйте фактические API.

## Чеклист модуля формы

Перед фиксацией изменений в модуле формы — [checklist-form-module.md](checklist-form-module.md) **вместе** с общим [checklist-precommit.md](../coding-standards/checklist-precommit.md).
