---
number: 11685
title: SIM103 creates double negative
type: issue
state: closed
author: charliermarsh
labels:
  - fixes
assignees: []
created_at: 2024-06-01T23:16:18Z
updated_at: 2024-06-01T23:21:12Z
url: https://github.com/astral-sh/ruff/issues/11685
synced_at: 2026-01-07T13:12:15-06:00
---

# SIM103 creates double negative

---

_Issue opened by @charliermarsh on 2024-06-01 23:16_

here's another example of an un-pleasing suggestion from SIM103.  I wavered over whether to raise this separately but decided is is related enough: easy to split things apart later if desired.
```python
def foo(num: int) -> bool:
    if not 10 < num < 100:
        return False

    return True
```
```
foo.py:2:5: SIM103 Return the condition `not not 10 < num < 100` directly
  |
1 |   def foo(num: int) -> bool:
2 |       if not 10 < num < 100:
  |  _____^
3 | |         return False
4 | |
5 | |     return True
  | |_______________^ SIM103
  |
  = help: Replace with `return not not 10 < num < 100`
```
`not not`? yuk!

_Originally posted by @dimbleby in https://github.com/astral-sh/ruff/issues/6184#issuecomment-2143492291_
            

---

_Referenced in [astral-sh/ruff#11684](../../astral-sh/ruff/pulls/11684.md) on 2024-06-01 23:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-01 23:17_

---

_Label `fixes` added by @charliermarsh on 2024-06-01 23:17_

---

_Closed by @charliermarsh on 2024-06-01 23:21_

---
