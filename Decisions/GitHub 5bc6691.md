---
type: decision
tags:
  - development
---

# GitHub 5bc6691

Коміт: 5bc6691d2d44fe86e39ccb122db7ca74bbbbc341
Репозиторій: SashaKuh/studyflow-app
Повідомлення: "PR3: build StudyFlow and MCP vault connector"
Автор: Oleksandr Kukharchuk, 2026-11-04T20:56:25Z
URL: https://github.com/SashaKuh/studyflow-app/commit/5bc6691d2d44fe86e39ccb122db7ca74bbbbc341

Рішення: реалізувати застосунок StudyFlow (пакет studyflow: CLI, domain, storage) та MCP-конектор до vault (harness/vault_server.py), підключений через .mcp.json, з тестами.

Додані файли (+415 рядків, 0 видалень, 19 файлів):
- .gitignore (+7), .mcp.json (+16), README.md (+18), pyproject.toml (+19)
- harness/__init__.py (+1), harness/vault_server.py (+125)
- scripts/run-vault-server.sh (+7)
- studyflow/__init__.py (+1), studyflow/cli.py (+45), studyflow/domain.py (+26), studyflow/storage.py (+70)
- tests/test_studyflow.py (+27), tests/test_vault.py (+18)
- studyflow_course.egg-info/* (6 згенерованих файлів, +35; видалені пізніше в 12a3049)

## Зв’язки
- [[Окремий vault]]
