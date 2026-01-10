```yaml
number: 9420
title: "PYI053 autofix removes `@deprecated` message"
type: issue
state: closed
author: hamdanal
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-01-07T10:01:07Z
updated_at: 2024-01-07T23:41:16Z
url: https://github.com/astral-sh/ruff/issues/9420
synced_at: 2026-01-10T11:09:51Z
```

# PYI053 autofix removes `@deprecated` message

---

_Issue opened by @hamdanal on 2024-01-07 10:01_

This concerns `@deprecated` from [PEP-702](https://peps.python.org/pep-0702/)

* A minimal code snippet that reproduces the bug.
Create a `t.pyi` file with the following content:
```python
from typing_extensions import deprecated

@deprecated("Function deprecated_func is deprecated. Use method new_func instead.")
def deprecated_func(self) -> None: ...
```
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
```console
$ # ruff generates an error
$ ruff check --select=PYI t.pyi --isolated
t.pyi:3:13: PYI053 [*] String and bytes literals longer than 50 characters are not permitted
Found 1 error.
[*] 1 fixable with the `--fix` option.
$ # While flake8-pyi does not
$ pip list | grep pyi
flake8-pyi  24.1.0
$ flake8 --select=Y t.pyi
$ # Even worse, with --fix it removes the message
$ ruff check --select=PYI t.pyi --isolated --diff
--- t.pyi
+++ t.pyi
@@ -1,4 +1,4 @@
 from typing_extensions import deprecated

-@deprecated("Function deprecated_func is deprecated. Use method new_func instead.")
+@deprecated(...)
 def deprecated_func(self) -> None: ...

Would fix 1 error.
```
* The current Ruff settings (any relevant sections from your `pyproject.toml`): N/A
* The current Ruff version (`ruff --version`): `ruff 0.1.11`

---

_Comment by @AlexWaygood on 2024-01-07 10:40_

Ah yes, we had to introduce some special-casing over at flake8-pyi recently for exactly this issue: https://github.com/PyCQA/flake8-pyi/commit/c7d988ef50f8582aad114a1fdc487dd75ff4f8e9

---

_Label `bug` added by @charliermarsh on 2024-01-07 15:05_

---

_Label `good first issue` added by @charliermarsh on 2024-01-07 15:06_

---

_Assigned to @AlexWaygood by @charliermarsh on 2024-01-07 17:38_

---

_Closed by @charliermarsh on 2024-01-07 23:41_

---
