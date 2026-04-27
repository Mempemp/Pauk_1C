# Чеклист: конвертация тем из cc-1c-skills в Pauk

Документ фабрики. Локальная папка `Материалы к конвертации/` в git не трекается; при переносе ориентиры сверялись с публичным репозиторием [cc-1c-skills](https://github.com/Nikolay-Shirokov/cc-1c-skills).

## Выбранные темы и целевые навыки Pauk

| Тема cc (группы / навыки) | Результат в пакете | Статус |
|---------------------------|-------------------|--------|
| MXL: `mxl-info`, `mxl-validate`, `mxl-compile`, `mxl-decompile`, спецификация табличного документа | Новый **`mxl_layouts`**: `SKILL.md` + `reference/template-xml-structure.md`, `areas-parameters-and-fill.md`, `json-dsl-outline.md` | Выполнено |
| Формы: структура Form.xml, DSL (без модульной части уже в `form_patterns`) | Дополнение **`form_patterns`**: индекс F16 и ссылка из «Когда применять» на **`reference/form-xml-structure-generation.md`** (файл добавлен субагентом) | Выполнено |
| EPF/ERF: `epf-*`, `erf-*`, `template-*`, `form-add/remove`, `help-add` (концептуально дерево XML) | Новый **`epf_erf_external`**: `SKILL.md` + `reference/directory-layout.md`, `root-metadata-and-forms.md`, `build-and-dump-cycle.md` | Выполнено |
| Корень конфигурации: `cf-info`, спецификация Configuration.xml | Новый **`configuration_structure`**: `SKILL.md` + `reference/root-file-overview.md` | Выполнено |

## Обязательные сопутствующие правки пакета

- [x] Строки в **`pauk-product/pauk/routing/SKILLS-CATALOG.md`** (таблица + блок «Сигналы → навыки»)
- [x] Согласование границ с **`metadata_manage`** (`SKILL.md`: описание и таблица «Не входит»)
- [x] **`docs/CONCEPT.md`**, ось 1 — отражение расширения каталога
- [x] Корневой **`README.md`** фабрики — перечисления `skill_id` нет (только отсылка к каталогу); правки не нужны

## Редакционная политика (проверено)

- В **`pauk-product/`** нет ссылок на фабрику, `docs/` фабрики, внешний git, синхронизацию.
- Имена файлов в `reference/` — тематические англоязычные **kebab-case**, как в правилах фабрики.
- Исполняемые скрипты cc (`.ps1`, `switch.py`) **не** копируются; в текстах указано оперирование через Конфигуратор / утилиты **проекта**.

## Исключено из этого шага (на будущее)

- Навыки БД (`db-*`), веб-публикации (`web-*`), СКД (`skd-*`), роли (`role-*`), подсистемы — другие домены.
- Slash-команды и пути `.claude/skills/` — не переносятся в прод-тексты.
