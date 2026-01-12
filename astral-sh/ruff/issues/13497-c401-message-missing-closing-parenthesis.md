```yaml
number: 13497
title: C401 message missing closing parenthesis
type: issue
state: closed
author: Toaster192
labels:
  - documentation
assignees: []
created_at: 2024-09-24T12:22:05Z
updated_at: 2024-09-24T12:55:34Z
url: https://github.com/astral-sh/ruff/issues/13497
synced_at: 2026-01-12T15:54:53Z
```

# C401 message missing closing parenthesis

---

_@Toaster192_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Can be seen in https://docs.astral.sh/ruff/rules/#flake8-comprehensions-c4 , the C401 message says `Unnecessary generator (rewrite using set()` compared to C400 above it which correctly has the closing parenthesis - `Unnecessary generator (rewrite using list())`

---

_Label `documentation` added by @MichaReiser on 2024-09-24 12:25_

---

_Comment by @Toaster192 on 2024-09-24 12:37_

Y'all Fast AF ❤️ 

---

_Closed by @MichaReiser on 2024-09-24 12:55_

---
