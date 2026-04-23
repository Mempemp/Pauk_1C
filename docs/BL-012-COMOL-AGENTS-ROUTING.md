# BL-012: агенты Comol → маршрутизация Pauk

Источник снимка: `Материалы к конвертации/cursor_rules_1c-main/.cursor/agents/`.  
**Принцип:** роли не вызывает пользователь вручную; **`route.workflow`** + **`route.skills`** на шаге 1 (`pauk/routing/PROTOCOL.md`).  
`metadata-manager.md` вынесен в **BL-011** и не дублируется здесь.

## Сводная таблица

| Файл Comol | Суть роли | В Pauk | `route.workflow` role id | Типовые `route.skills` |
|------------|-----------|--------|---------------------------|-------------------------|
| `developer.md` | BSL-разработка, workflow от шаблонов до проверки | Покрыто шагами **`implement`** / **`verify`** + навыки; отдельный промпт **не** вводим | — | `coding-standards`, по задаче `form_patterns`, `query-patterns`, … |
| `architect.md` | Архитектура доработок, ADR, trade-off, Mermaid | Тонкий промпт **`solution-architect`** | `solution-architect` | `coding-standards`, при метаданных `metadata_manage`, при запросах `query-patterns` |
| `analytic.md` | PRD/ТЗ, без кода, структура документов | **Только фабрика:** шаблоны и запреты ниже в §2; отдельный `skill_id` в пакете **не** добавляли | — (или явный шаг `task-analyst` без нового файла) | при необходимости только `coding-standards` для терминов |
| `code-reviewer.md` | Ревью BSL, MCP review, Approve/Block | Промпт **`code-review`** | `code-review` | `coding-standards`, `error_handling` при ошибках контура |
| `arch-reviewer.md` | Арх. ревью до/после крупных изменений | Промпт **`arch-review`** | `arch-review` | `coding-standards`, `query-patterns` при данными/перф |
| `doc-writer.md` | Внешняя документация, codemap | Промпт **`external-docs`** | `external-docs` | `coding-standards`; структура каталогов — из проекта |
| `planner.md` | План фичи, фазы, риски | Смысл **встроен** в шаг **`design`** / начало **`task-analyst`**; отдельный файл **не** обязателен | — | + каталог по сигналам |
| `tester.md` | Деплой в ИБ, UI через браузер | Промпт **`deploy-test`** (пути и команды — только из проекта) | `deploy-test` | `coding-standards`, при выгрузке `metadata_manage`; регламент деплоя — **BL-014** / README проекта |
| `refactoring.md` | Безопасный рефакторинг, MCP, антипаттерны | Промпт **`code-refactor`** | `code-refactor` | `coding-standards`, `query-patterns`, часто `error_handling` |
| `error-fixer.md` | Узкий фикс ошибок LS/рантайм | Промпт **`error-fix`** | `error-fix` | `coding-standards`, `error_handling` |
| `performance-optimizer.md` | Профиль перфоманса, ITS | Промпт **`performance-opt`** | `performance-opt` | `query-patterns`, `coding-standards`; MCP по `pauk/mcp/README.md` |

## §2. Analytic — заготовки (фабрика, не в `pauk-product`)

Использовать при отдельной сессии «спека до кода»:

- Документ без кода: цели, термины 1С, измеримые критерии, границы, вопросы к заказчику.
- Запрет: выдумывать сроки внедрения и менять типовые объекты без обоснования.
- Диаграммы — по соглашению проекта (Mermaid и т.п.).

Полный PRD-шаблон остаётся в снимке `analytic.md`; при переносе в продукт — выносить **кусочно** в репозиторий заказчика, не раздувая пакет.

## §3. Итерации субагентов (как делили работу)

1. **Сессия A:** `developer`, `architect`, `analytic` — решения в таблице.  
2. **Сессия B:** `code-reviewer`, `arch-reviewer`, `doc-writer`, `planner`.  
3. **Сессия C:** `tester`, `refactoring`, `error-fixer`, `performance-optimizer`.

## §4. Итерация 2 (хвосты)

- Добавлены промпты в `pauk/subagents/`: **`external-docs.md`**, **`deploy-test.md`**, **`performance-opt.md`** и строки в **`WORKFLOW.md`** / **`README.md`**.  
- **`deploy-test`** по-прежнему требует **локального регламента** (команды, путь к платформе, ИБ) — см. **BL-014** и файлы проекта.

## §5. Следующие шаги

- Автоматический выбор роли по **BL-002** (классификация → `workflow` + `skills`).
