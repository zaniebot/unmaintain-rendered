---
number: 198
title: Incorrect line number/column in F823 error
type: issue
state: closed
author: Jackenmen
labels:
  - bug
assignees: []
created_at: 2022-09-15T02:21:09Z
updated_at: 2022-09-15T02:51:08Z
url: https://github.com/astral-sh/ruff/issues/198
synced_at: 2026-01-07T13:12:14-06:00
---

# Incorrect line number/column in F823 error

---

_Issue opened by @Jackenmen on 2022-09-15 02:21_

For code:
```py
TOMATO = "black cherry"


def update_tomato():
    print(TOMATO)
    TOMATO = "cherry tomato"


update_tomato()
```

I get output:
```
❯ flake8 repro.py 
repro.py:5:11: F823 local variable 'TOMATO' defined in enclosing scope on line 1 referenced before assignment
repro.py:6:5: F841 local variable 'TOMATO' is assigned to but never used

❯ ruff repro.py   
repro.py:6:5: F823 Local variable `TOMATO` referenced before assignment
repro.py:6:5: F841 Local variable `TOMATO` is assigned to but never used

Found 2 error(s).
```
flake8 shows the correct position while ruff doesn't.

---

_Referenced in [astral-sh/ruff#200](../../astral-sh/ruff/pulls/200.md) on 2022-09-15 02:48_

---

_Label `bug` added by @charliermarsh on 2022-09-15 02:48_

---

_Closed by @charliermarsh on 2022-09-15 02:51_

---
