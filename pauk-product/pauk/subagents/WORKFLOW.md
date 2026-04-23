# Цепочка субагентов

Значения для поля `route.workflow` при сборке маршрута. Порядок подключения ролей — по `pauk/routing/PROTOCOL.md` (для `trivial` цепочка выполняется в одном ответе с `route`; для `full` — начиная со следующего сообщения пользователя).

## Идентификаторы ролей

`task-analyst` → `explore` → (при необходимости) `design` → `implement` → `verify`

## Масштаб

- **trivial:** часто `implement` → `verify`; при необходимости короткий `explore`.
- **full:** с `task-analyst` в начале; `design` — при крупном проектировании реализации (условия принятия blueprint — `pauk/policy/A-INVARIANTS.md`).

Промпты ролей — файлы в этом каталоге.
