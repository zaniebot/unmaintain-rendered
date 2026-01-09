---
number: 17595
title: "[red-knot] Failed to narrow in nested function"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
created_at: 2025-04-23T22:11:15Z
updated_at: 2025-05-07T15:21:04Z
url: https://github.com/astral-sh/ruff/issues/17595
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Failed to narrow in nested function

---

_Issue opened by @dhruvmanila on 2025-04-23 22:11_

### Summary

Red knot fails to narrow in the following code:

```py
def foo(x: str | None):
    def inner():
        if x is not None:
            # Red knot is not narrowing here
            reveal_type(x)  # revealed: str | None

    return inner
```

### Version

00e73dc33

---

_Label `bug` added by @dhruvmanila on 2025-04-23 22:11_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-23 22:11_

---

_Added to milestone `Red Knot Beta` by @carljm on 2025-04-23 22:22_

---

_Referenced in [astral-sh/ruff#17591](../../astral-sh/ruff/pulls/17591.md) on 2025-04-23 22:24_

---

_Comment by @mtshiba on 2025-04-24 10:50_

The current implementation of narrowing only covers cases where the predicate scope and the definition scope are the same. We need to be able to use predicates of different scopes.

This seems to duplicate some of the work I am doing in astral-sh/ty#164. Because when we want to do instance attribute narrowing, we use instance attribute definition and predicate of attribute access to the variable to which the instance is assigned, but the scopes of the two are generally different.

```python
class C:
    def __init__(self):
        # definition
        self.x: int | None = None

c = C()
# predicate
if c.x is not None:
    reveal_type(c.x)  # int
```

---

_Referenced in [astral-sh/ruff#17630](../../astral-sh/ruff/pulls/17630.md) on 2025-04-25 15:33_

---

_Referenced in [astral-sh/ty#164](../../astral-sh/ty/issues/164.md) on 2025-04-25 15:44_

---

_Closed by @carljm on 2025-05-05 23:28_

---

_Closed by @carljm on 2025-05-05 23:28_

---
