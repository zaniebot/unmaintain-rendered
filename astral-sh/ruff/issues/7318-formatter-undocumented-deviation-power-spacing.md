```yaml
number: 7318
title: "Formatter undocumented deviation: power spacing"
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
  - good first issue
  - formatter
  - help wanted
assignees: []
created_at: 2023-09-12T20:55:56Z
updated_at: 2023-09-14T15:36:23Z
url: https://github.com/astral-sh/ruff/issues/7318
synced_at: 2026-01-12T15:54:47Z
```

# Formatter undocumented deviation: power spacing

---

_@JonathanPlasse_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Black formatting
```python
10 ** (2)
```
Ruff formatting
```python
10**(2)
```
Use Ruff 0.0.289 with line length 100

---

_Label `bug` added by @MichaReiser on 2023-09-13 06:54_

---

_Label `formatter` added by @MichaReiser on 2023-09-13 06:54_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-13 06:55_

---

_Comment by @MichaReiser on 2023-09-13 06:56_

Thanks. This should be easy to fix. We need to ensure that both left and right aren't parenthesized (using `is_expression_parenthesized`)

https://github.com/astral-sh/ruff/blob/41f0aad7b3e163436d870dd936e0cdd05cf4b80b/crates/ruff_python_formatter/src/expression/binary_like.rs#L364-L366

---

_Label `help wanted` added by @MichaReiser on 2023-09-13 06:56_

---

_Label `good first issue` added by @MichaReiser on 2023-09-13 09:20_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-13 22:54_

---

_Closed by @charliermarsh on 2023-09-14 15:36_

---
