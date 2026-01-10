---
number: 4740
title: "[EM101] Ruff's EM101 and flake8-errms' EM101 results are inconsistent when the string literal is an empty string"
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2023-05-31T00:17:03Z
updated_at: 2023-05-31T04:37:45Z
url: https://github.com/astral-sh/ruff/issues/4740
synced_at: 2026-01-10T01:22:43Z
---

# [EM101] Ruff's EM101 and flake8-errms' EM101 results are inconsistent when the string literal is an empty string

---

_Issue opened by @FishAlchemist on 2023-05-31 00:17_

Ruff's EM101 cannot detect EM101 when the string is empty, but flake8-errms can detect EM101
```
flake8-errmsg .\main.py
.\main.py:11:8 EM101 Exception must not use a string literal, assign to variable first
```
## Ruff Version
**ruff 0.0.270**
## flake-errms version
```
Package       Version
------------- -------
flake8        6.0.0
flake8_errmsg 0.4.0
```
## Command
```powershell
ruff check --select=EM --isolated .\main.py
```
## Settings
not exist

## Python code
```python
from __future__ import annotations

from typing import NoReturn


class Hello:
    def __init__(self: Hello) -> None:
        self.value = 100

    def me(self: Hello) -> NoReturn:
        raise RuntimeError("")

```


---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-31 04:12_

---

_Label `bug` added by @charliermarsh on 2023-05-31 04:12_

---

_Referenced in [astral-sh/ruff#4745](../../astral-sh/ruff/pulls/4745.md) on 2023-05-31 04:18_

---

_Comment by @charliermarsh on 2023-05-31 04:18_

Thanks!

---

_Closed by @charliermarsh on 2023-05-31 04:37_

---
