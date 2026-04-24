---
name: powershell_windows
description: >-
  Правила PowerShell в Windows: разделители команд, кавычки путей, HTTP без curl,
  Docker, паузы, JSON. Подключать при генерации или отладке команд терминала,
  скриптов .ps1, CI-шагов и обвязки вокруг 1cv8/Designer; не для синтаксиса BSL.
---

# Windows PowerShell для обвязки и сценариев

## Когда применять

- Запуск **команд** в **Windows** (в т.ч. `1cv8.exe`, `docker-compose`, HTTP к локальным сервисам) **из** чата агента, скриптов или **репозитория** (`.ps1`, CI).
- Путь с **пробелами**, цепочка **нескольких** команд, ожидание, разбор **JSON** от `Invoke-WebRequest`.
- **Не** заменяет знание параметров **платформы 1С** (ключи `DESIGNER`, `/F`, `/S`) — они в шаблонах **вашей** команды, см. [reference/infobase-sync.md](../../reference/infobase-sync.md).

## Критический принцип

**PowerShell ≠ bash:** неиспользование `&&` как в bash, корректные **кавычки** для путей с пробелами, для HTTP в стандартной среде — **Invoke-WebRequest** / `Invoke-RestMethod`, а не ожидание `curl` как в Unix-шпаргалках.

## Краткие правила

### Разделение команд

- **Неверно:** `cd "path" && command` (синтаксис bash).
- **Верно:** `cd "path"; command` или отдельные строки.

### Кавычки и пути

- **Двойные** кавычки для путей с пробелами, полный путь к `1cv8.exe` и к каталогу выгрузки — из **ваших** файлов настроек, не из «примера с другой машины»:

```powershell
cd "D:\мои проекты\репо"; & "C:\Program Files\1cv8\8.3.xx\bin\1cv8.exe" ...
```

### Запуск скриптов и .cmd

```powershell
.\build.ps1
.\gradlew.bat clean build
```

### Docker

- Полный путь к `docker-compose.yml` в кавычках; например: `docker-compose -f "D:\repo\docker-compose.yml" up -d`.

### HTTP (локальные проверки, не BSL-обмен)

- **Не** опирайтесь на `curl` как в учебниках Linux. В **Windows PowerShell**:

```powershell
Invoke-WebRequest -Uri "http://localhost:8080/health" -UseBasicParsing
# или разбор тела
$response = Invoke-WebRequest -Uri "http://localhost:8080/api" -UseBasicParsing
$json = $response.Content | ConvertFrom-Json
```

Прикладной **обмен 1С с внешними системами** в коде — навык [integration_patterns/SKILL.md](../integration_patterns/SKILL.md); здесь — только **оболочка** для скрипта/агента.

### Пауза

- Вместо `timeout 10`: `Start-Sleep -Seconds 10`

### JSON и процессы

- `ConvertFrom-Json` / `ConvertTo-Json -Depth` для разбора и вывода.
- `Get-Process -Name "…" -ErrorAction SilentlyContinue` при проверке процессов; `-ErrorAction` для подавляемых ошибок по политике.

## Типичные ошибки

| Симптом | Причина | Что сделать |
|---------|---------|-------------|
| `The token '&&' is not a valid statement separator` | Синтаксис bash | Заменить на `;` или отдельные вызовы |
| `curl` не распознана / не то поведение | Среда без `curl` или старые привычки | `Invoke-WebRequest` / `Invoke-RestMethod` |
| `timeout` не сработал | Команда cmd, не PowerShell | `Start-Sleep -Seconds N` |
| путь «не найден» | Пробелы в пути без кавычек | Взять путь в **двойные** кавычки |

## С чем сочетать

| Сценарий | Куда смотреть |
|----------|----------------|
| Загрузка/выгрузка с ИБ, `1cv8` | [../../reference/infobase-sync.md](../../reference/infobase-sync.md) |
| Политика готовности, опасные правки | [../../policy/A-INVARIANTS.md](../../policy/A-INVARIANTS.md) |

При необходимости **точного** батника под paths с пробелами вне PowerShell — см. внешние Windows-гайдлайны; в оснастке Pauk **канон** для встроенного термина Windows — этот навык.
