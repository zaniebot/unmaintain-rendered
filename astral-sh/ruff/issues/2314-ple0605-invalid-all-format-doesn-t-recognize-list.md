```yaml
number: 2314
title: "PLE0605 (invalid-all-format) doesn't recognize list comprehensions"
type: issue
state: closed
author: eriknw
labels:
  - bug
assignees: []
created_at: 2023-01-28T20:48:51Z
updated_at: 2023-01-29T19:27:10Z
url: https://github.com/astral-sh/ruff/issues/2314
synced_at: 2026-01-12T15:54:42Z
```

# PLE0605 (invalid-all-format) doesn't recognize list comprehensions

---

_@eriknw_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->
`ruff` version `v0.0.237` does not recognize list comprehension as a valid "list" type for `__all__` (rule added in https://github.com/charliermarsh/ruff/pull/2241). For example, this code:
```python
names = {"x", "y", "skip"}
__all__ = [name for name in names if name != "skip"]
```
gives
```
PLE0605 Invalid format for `__all__`, must be `tuple` or `list`
```

Note that PyLint does not recognize this as a violation.

---

_Label `bug` added by @charliermarsh on 2023-01-29 18:46_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-29 19:12_

---

_Closed by @charliermarsh on 2023-01-29 19:26_

---

_Comment by @charliermarsh on 2023-01-29 19:27_

Added list comprehensions. I think Pylint is _even_ more permissive, but I'll adjust as issues come up.

---
