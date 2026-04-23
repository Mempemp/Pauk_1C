# Цепочка субагентов

Значения для поля `route.workflow` после шага 1.

## Идентификаторы ролей

`task-analyst` → `explore` → (при необходимости) `design` → `implement` → `verify`

## Масштаб

- **trivial:** часто `implement` → `verify`; при необходимости короткий `explore`.
- **full:** с `task-analyst` в начале; `design` — при крупном проектировании реализации (условия принятия blueprint — `pauk/policy/A-INVARIANTS.md`).

Промпты ролей — файлы в этом каталоге.
