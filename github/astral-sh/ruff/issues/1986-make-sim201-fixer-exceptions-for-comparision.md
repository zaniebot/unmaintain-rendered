---
number: 1986
title: make SIM201 fixer exceptions for comparision methods? 
type: issue
state: closed
author: spaceone
labels: []
assignees: []
created_at: 2023-01-19T00:21:34Z
updated_at: 2023-01-19T16:27:29Z
url: https://github.com/astral-sh/ruff/issues/1986
synced_at: 2026-01-07T13:12:14-06:00
---

# make SIM201 fixer exceptions for comparision methods? 

---

_Issue opened by @spaceone on 2023-01-19 00:21_

`SIM201` makes the following changes:

```
     def __ne__(self, other):
-        return not self == other
+        return self != other
```
This will of course end in a endless recursion.

Maybe the checker or fixer could ignore the occurrence in `__lt__`, `__le__`, `__eq__`, `__ne__`, `__ge__`, `__gt__`.

---

_Referenced in [chatmail/core#3969](../../chatmail/core/pulls/3969.md) on 2023-01-19 10:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-19 15:32_

---

_Comment by @charliermarsh on 2023-01-19 15:33_

Makes sense, we can fix this.

---

_Referenced in [astral-sh/ruff#2001](../../astral-sh/ruff/pulls/2001.md) on 2023-01-19 16:27_

---

_Closed by @charliermarsh on 2023-01-19 16:27_

---
