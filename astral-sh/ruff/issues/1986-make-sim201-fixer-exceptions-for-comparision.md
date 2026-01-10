```yaml
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
synced_at: 2026-01-10T11:09:44Z
```

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

_Assigned to @charliermarsh by @charliermarsh on 2023-01-19 15:32_

---

_Comment by @charliermarsh on 2023-01-19 15:33_

Makes sense, we can fix this.

---

_Closed by @charliermarsh on 2023-01-19 16:27_

---
