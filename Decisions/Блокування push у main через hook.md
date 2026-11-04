---
type: decision
tags:
  - development
---

# Блокування push у main через hook

## Контекст
Правило проєкту: агент не пушить напряму в `main` (`CLAUDE.md`, guardrail B1 у `docs/guardrails.md`). Claude Code виконує команди через інструмент `Bash`, тому текстова інструкція в `CLAUDE.md` не гарантує дотримання — потрібна технічна перевірка до виконання дії.

## Рішення
Забезпечувати правило B1 через Claude Code `PreToolUse` hook: `.claude/settings.json` реєструє команду `python3 "$CLAUDE_PROJECT_DIR/.claude/hooks/guardrail.py"` з matcher `Bash|Write|Edit`; скрипт повертає `permissionDecision: deny` для прямого push у `main`.

## Поточна реалізація
- `.claude/hooks/guardrail.py`, `check_action`: для `Bash` регулярний вираз `\bgit\s+push\b[^\n;]*\bmain\b` → `deny` з причиною «Прямий push у main заборонено для агента».
- `main()` читає одну JSON-подію зі stdin і друкує `hookSpecificOutput` (`hookEventName: PreToolUse`, `permissionDecision`, `permissionDecisionReason`); без збігу нічого не друкує, і діє звичайний режим дозволів.
- Той самий hook реалізує B2 (`rm -rf` → `deny`), B3 (запис у `Decisions/` → `deny`), W1–W3 (`ask`).
- Тест: `tests/test_guardrails.py`, сценарій `git push origin main` → `deny`.

## Причина
Hook спрацьовує на рівні harness до виконання команди, тож не залежить від того, чи модель «пам’ятає» інструкцію. Правила зібрані в одному Python-файлі, який можна юніт-тестувати через `check_action`, а конфігурація комітиться разом із репозиторієм.

## Альтернативи
- Лише інструкція в `CLAUDE.md` — відхилено: не є технічним блокуванням.
- Git `pre-push` hook — відхилено як основний механізм: не комітиться в `.git/hooks` за замовчуванням і блокує також людину, а не лише агента.
- Захист гілки на GitHub — доповнення, а не заміна: діє лише на remote і поза контролем сесії агента.
- `permissions.deny` у `settings.json` — відхилено: префіксні шаблони менш гнучкі, ніж регулярний вираз, і гірше тестуються.

## Наслідки
- Навчальний механізм, не повна ізоляція (`docs/guardrails.md`): регулярний вираз вимагає явного слова `main`, тому `git push` без аргументів на гілці `main` або `git push origin HEAD` не блокуються; можливі хибні `deny`, напр. для гілки `feature-main` чи ланцюжка `git push origin x && git checkout main`.
- Для production потрібні захист гілки на GitHub і права ОС.
- План на майбутнє (ще не реалізовано): перевіряти поточну гілку для `git push` без refspec і додати тести на обхідні форми.

## Зв’язки
- [[Блокування push у main через hook]] implements [[Guardrail]].
- [[Блокування push у main через hook]] complements [[Окремий vault]].
