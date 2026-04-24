# Субагенты

Промпты и инструкции ролей из **этой** таблицы. **Включение роли** — через **`route.workflow`** на шаге 1 (**`pauk/routing/PROTOCOL.md`**, оркестратор = исполнитель по правилам) **или** явная просьба человека к конкретному файлу здесь; в режиме обычной задачи каталог **не** обходить без нужды (см. **`PROTOCOL.md`**, § «Субагенты и навыки»). Какие **`SKILL.md`** подмешивать к шагу, задаёт **`route.skills`** в том же маршруте. Цепочка, условия по ролям и **предопределённые сценарии (пресеты)**: **`WORKFLOW.md`**. **Объёмные** задачи с handoff-артефактом — шаблон **`pauk/routing/ARTIFACT-ROUTER.md`**; готовый сценарий большой задачи — **`.cursor/commands/flow-full-pipeline.md`** (см. `WORKFLOW.md`, пресет `full-pipeline`).

| Файл | Роль |
|------|------|
| `task-analyst.md` | Аналитика объёмной задачи, `ARTIFACT-ROUTER`; в пресете **`technical-design`** — вопросы, corner cases и финальная нарезка ТЗ в **`Technical-Design/`** |
| `design.md` | Крупноблочный blueprint; опциональный шаг в **`technical-design`** после **`solution-architect`** |
| `metadata-manager.md` | Многошаговые изменения XML-выгрузки и Form.xml, валидация; ожидает в `route.skills` как минимум `metadata_manage`, при работе с модулем формы — `form_patterns`; см. `WORKFLOW.md` |
| `solution-architect.md` | Архитектурный контракт доработки без дублирования `implement` |
| `arch-review.md` | Архитектурное ревью крупного изменения |
| `code-review.md` | Ревью BSL после существенных правок |
| `code-refactor.md` | Сфокусированный рефакторинг без смены поведения; ожидаемые `refactor_workflow` + `coding-standards` при существенной перестройке — **`WORKFLOW.md`**, подраздел про навыки |
| `error-fix.md` | Узкий цикл исправления ошибок компиляции/рантайма |
| `external-docs.md` | Внешняя документация и codemap (не комментарии в BSL) |
| `deploy-test.md` | Деплой в тестовую ИБ и смоук по регламенту проекта |
| `performance-opt.md` | Оптимизация производительности и запросов |
