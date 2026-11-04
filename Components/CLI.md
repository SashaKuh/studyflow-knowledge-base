---
type: component
created: 2026-11-04
depends_on: ["SQLite-сховище"]
implements: ["Навчальна задача"]
tags:
  - architecture/interface
---

# CLI

Командний інтерфейс обробляє `add`, `list`, `done`, `summary`, `export`, показує помилки й делегує зберігання [[SQLite-сховище]].

## Зв’язки
- [[CLI]] depends_on [[SQLite-сховище]].
- [[CLI]] implements [[Навчальна задача]].
