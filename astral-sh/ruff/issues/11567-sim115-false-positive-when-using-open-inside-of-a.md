```yaml
number: 11567
title: "`SIM115` false positive when using `open()` inside of a custom context manager `__enter__`"
type: issue
state: closed
author: p-adema
labels:
  - bug
assignees: []
created_at: 2024-05-27T14:42:35Z
updated_at: 2024-05-28T01:44:25Z
url: https://github.com/astral-sh/ruff/issues/11567
synced_at: 2026-01-10T11:09:53Z
```

# `SIM115` false positive when using `open()` inside of a custom context manager `__enter__`

---

_Issue opened by @p-adema on 2024-05-27 14:42_

Keywords: `SIM115`, `__enter__`, context manager

When writing your own context manager, which internally uses another context manager (like `open`), SIM115 suggests using a `with` block when this is not possible (as the goal is to open the resource in `__enter__`, and keep it open).

I would expect/hope the behaviour be to automatically suppress this check within an `__enter__` block, as (in my experience) it is often the case that these blocks delegate some of the handling to e.g. `open()`

Code snippet:
```python
class MyFile:
    def __init__(self, filename: str):
        self.filename = filename

    def __enter__(self):
        self.file = open(self.filename) # SIM115 : Use context handler for opening files

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()
```
Command invoked:
```fish
ruff check context.py --isolated --select SIM115
```
```
context.py:7:21: SIM115 Use context handler for opening files
Found 1 error.
```
Ruff version: 0.4.5
Configuration:
```toml
[lint]
select = [
    "SIM",
]
```

---

_Label `bug` added by @charliermarsh on 2024-05-27 16:32_

---

_Comment by @charliermarsh on 2024-05-27 16:32_

Seems reasonable to me...

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-28 01:35_

---

_Closed by @charliermarsh on 2024-05-28 01:44_

---
