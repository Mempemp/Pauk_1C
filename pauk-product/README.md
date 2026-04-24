# Установка

В корень целевого проекта скопируйте каталоги **`pauk`** и **`.cursor`** (структуру внутри сохранить).

## Выгрузка, загрузка, тестовая ИБ (опционально)

Сценарии **Designer** (`LoadConfigFromFiles`, `UpdateDBCfg`, `DumpConfigToFiles`, лог, `repoobjects.txt`) **не** зашивают путь к `1cv8.exe` и строки подключения: их задаёте **вы** в скриптах или `infobasesettings.md` репозитория. Плейсхолдеры и шаги — в **`pauk/reference/infobase-sync.md`**. Тонкие команды Cursor, если нужны, — **`.cursor/commands/*.md`**, они **ведут** в тот же файл, а не дублируют полный сценарий. Подбор навыков к задаче — по `pauk/routing/SKILLS-CATALOG.md` (команды **не** заменяют `PROTOCOL.md`).
