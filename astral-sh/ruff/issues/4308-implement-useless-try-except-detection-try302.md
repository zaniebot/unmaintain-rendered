```yaml
number: 4308
title: "Implement useless try-except detection (`TRY302`)"
type: issue
state: closed
author: dorschw
labels:
  - good first issue
  - rule
assignees: []
created_at: 2023-05-09T14:53:32Z
updated_at: 2023-05-17T01:36:12Z
url: https://github.com/astral-sh/ruff/issues/4308
synced_at: 2026-01-12T15:54:44Z
```

# Implement useless try-except detection (`TRY302`)

---

_@dorschw_

**Bad code:**
```python
# case 1
try:
    do_thing()
except Exception:
    raise
```

```python
# case 2
try:
    do_thing()
except:
    raise
```

```python
# case 3
try:
    do_thing()
except SomeKindOfException:
    raise
```

**Fix:**
remove the try/except, as it has no effect. 
```python
do_thing()
```
In cases 1&2, all exceptions are raised immediately, and in case 3 only a subset of exceptions is caught, then reraised immediately. 

Notes: 
* This shouldn't run if there's any other code in the `except` block. 
* If there are multiple excpt cases, empty ones (with only `raise`) should be removed, and others should remain.




---

_Comment by @Skylion007 on 2023-05-10 14:13_

This feels like a rule https://github.com/guilatrova/tryceratops/ would love to implement, especially since they already have relevant error codes for using the implied `raise` syntax. @dorschw we should open an issue there. If they add it, we can reimplement it in ruff and use the same error code. If they don't pick it up, https://github.com/MartinThoma/flake8-simplify/ might be interested in implementing it as well.

---

_Label `rule` added by @charliermarsh on 2023-05-10 14:14_

---

_Comment by @JonathanPlasse on 2023-05-13 19:28_

It has been implemented by `tryceratops`.
It would be a good first issue.

---

_Label `good first issue` added by @charliermarsh on 2023-05-13 19:43_

---

_Renamed from "Suggestion: Useless try/except" to "Implement useless try-except detection (`TRY302`)" by @charliermarsh on 2023-05-13 19:43_

---

_Closed by @charliermarsh on 2023-05-17 01:36_

---
