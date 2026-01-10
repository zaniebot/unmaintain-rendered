```yaml
number: 17582
title: Duplicate E712 diagnostics
type: issue
state: closed
author: whiskeyboy9
labels:
  - rule
assignees: []
created_at: 2025-04-23T12:32:11Z
updated_at: 2025-04-28T07:31:18Z
url: https://github.com/astral-sh/ruff/issues/17582
synced_at: 2026-01-10T11:09:58Z
```

# Duplicate E712 diagnostics

---

_Issue opened by @whiskeyboy9 on 2025-04-23 12:32_

## Duplicate diagnostics for RUF001 in VS Code

**Describe the bug**
I am seeing the same diagnostic reported twice in the VS Code "Problems" tab when using the Ruff extension. Specifically, the `RUF001: Avoid equality comparisons to True; use `is True` for truth checks` error appears two times for the same line of code.

**To Reproduce**
Steps to reproduce the behavior:
1.  Set up a Python project using Poetry.
2.  Add `ruff` to the development dependencies (`poetry add ruff@latest --group dev`).
3.  Ensure the Ruff VS Code extension is installed and enabled.
4.  Create a Python file with code that triggers the `RUF001` rule, e.g., `if variable == True:`.
5.  Open the file in VS Code.
6.  Observe the "Problems" tab where the `RUF001` diagnostic is listed twice for the same location.

**Expected behavior**
I would expect the diagnostic for the `RUF001` error to appear only once in the "Problems" tab.

**Screenshots**
![Image showing duplicated errors](https://github.com/user-attachments/assets/6246c13b-ec62-4e34-8ff5-5bab44f37e20)

**Environment**
* **VS Code:**
    ```
    Version: 1.99.3 (user setup)
    Commit: 17baf841131aa23349f217ca7c570c76ee87b957
    Date: 2025-04-15T23:18:46.076Z (1 wk ago)
    Electron: 34.3.2
    ElectronBuildId: 11161073
    Chromium: 132.0.6834.210
    Node.js: 20.18.3
    V8: 13.2.152.41-electron.0
    OS: Windows_NT x64 10.0.19045
    ```
* **Ruff VS Code Extension:**
    ```
    Installation
    Identifier: charliermarsh.ruff
    Version: 2025.22.0
    Last Updated: 2025-04-23, 13:33:23
    Size: 33.03MB
    ```
* **Ruff Version (from log):** 0.11.6
* **Python Version (from log interpreter path):** Likely 3.11 (based on environment setup with poetry and python = "^3.11")
* **Package Manager:** Poetry

**Relevant Configuration**

`.venv\Scripts\python.exe`

`pyproject.toml`:
```toml
[tool.poetry]
name = "test_cicd"
version = "0.0.1"
description = "testing cicd pipelines"
authors = [""]
readme = "README.md"
packages = [{include = "dummy", from = "src"}]

[tool.poetry.dependencies]
python = "^3.11"
pydantic = "^2.6.4"
python-dotenv = "^1.0.1"

numpy = "^2.1.1"
pandas = "^2.2.3"

[tool.poetry.group.dev]
optional = true

[tool.poetry.group.dev.dependencies]
ipykernel = "^6.29"
pytest = "^8.1"
ruff = "^0.11.6"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"

[tool.pytest.ini_options]
pythonpath = [
  ".", "src",
]

[tool.ruff]
target-version = "py311"
line-length = 120

[tool.ruff.lint] # Add this section
    # select = ["E", "F"]
    # ignore = ["E501"] # Example
    ignore = [
        "E501", # line too long, handled by black
        "B008", # do not perform function calls in argument defaults
        "C901", # too complex
    ]

    select = [
        "E", # pycodestyle errors
        "W", # pycodestyle warnings
        "F", # pyflakes
        "I", # isort
        "C", # flake8-comprehensions
        "B", # flake8-bugbear
        "SIM" # simplify
    ]

---

_Comment by @ntBre on 2025-04-23 13:15_

Thanks for the report! I don't think this is a duplicate diagnostic. You're getting one diagnostic for each `True` value, but each diagnostic highlights the whole comparison expression. If you had `PLR0133` enabled, you'd get a third diagnostic with the same range: https://play.ruff.rs/68219349-d23f-45a8-91b3-1154d97b805f

We could arguably limit the ranges to each `True` separately, but I think it generally makes sense to mark the whole comparison.

---

_Comment by @MichaReiser on 2025-04-23 13:20_

We could try to be a bit more clever and avoid the second diagnostic if both sides are `True`?

---

_Comment by @ntBre on 2025-04-23 13:22_

That sounds reasonable to me. I'll transfer this to the `ruff` repo because it reproduces in the playground and CLI too.

---

_Renamed from "duplicated diagnostics in ruff - vscode" to "Duplicate E712 diagnostics" by @ntBre on 2025-04-23 13:23_

---

_Label `rule` added by @ntBre on 2025-04-23 13:24_

---

_Closed by @MichaReiser on 2025-04-28 07:31_

---
