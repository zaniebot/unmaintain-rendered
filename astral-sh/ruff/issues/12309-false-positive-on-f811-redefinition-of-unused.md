```yaml
number: 12309
title: "False positive on F811 \"Redefinition of unused\" when using contextlib.suppress"
type: issue
state: closed
author: masklinn
labels:
  - bug
assignees: []
created_at: 2024-07-13T12:52:15Z
updated_at: 2024-07-13T19:22:19Z
url: https://github.com/astral-sh/ruff/issues/12309
synced_at: 2026-01-10T11:09:54Z
```

# False positive on F811 "Redefinition of unused" when using contextlib.suppress

---

_Issue opened by @masklinn on 2024-07-13 12:52_

I assume this is a generalised problem with context managers being able to influence control flow but I figure `contextlib.suppress` could be special-cased maybe?
```python
import contextlib

foo = None
with contextlib.suppress(ImportError):
    from some_module import foo


bar = None
try:
    from some_module import bar
except ImportError:
    pass

print(foo, bar)
```
the two bits setting "foo" and "bar" are functionally equivalent, but in the first one Ruff reports
```
test.py:5:29: F811 Redefinition of unused `foo` from line 3
  |
3 | foo = None
4 | with contextlib.suppress(ImportError):
5 |     from some_module import foo
  |                             ^^^ F811
  |
  = help: Remove definition: `foo`
```
while the second one works fine.

---

_Comment by @charliermarsh on 2024-07-13 16:49_

This is interesting, agree that it's a bug.

---

_Label `bug` added by @charliermarsh on 2024-07-13 16:49_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-13 18:24_

---

_Closed by @charliermarsh on 2024-07-13 19:22_

---

_Closed by @charliermarsh on 2024-07-13 19:22_

---
