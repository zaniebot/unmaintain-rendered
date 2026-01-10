```yaml
number: 14547
title: "[red-knot] support narrowing on bool(E)"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-11-22T23:04:39Z
updated_at: 2024-12-03T03:05:00Z
url: https://github.com/astral-sh/ruff/issues/14547
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] support narrowing on bool(E)

---

_Issue opened by @carljm on 2024-11-22 23:04_

We currently support type narrowing on various kinds of expressions in a test clause, e.g.

```py
x = 1 if flag() else None

reveal_type(x)  # revealed: Literal[1] | None
if x is not None:
    reveal_type(x)  # revealed: Literal[1]
```

This task is to add support for the case where the test expression is wrapped in an explicit call to `bool(...)`, e.g. `if bool(x is not None):`. This is not a common pattern, but pyright supports it, it's always correct, and it's not hard to implement, so there's little reason not to support it.

For any test expression E which we would draw narrowing conclusions from, we should draw the same conclusions from the test expression `bool(E)`.

---

_Label `red-knot` added by @carljm on 2024-11-22 23:04_

---

_Assigned to @carljm by @carljm on 2024-11-22 23:04_

---

_Unassigned @carljm by @carljm on 2024-12-02 18:13_

---

_Closed by @carljm on 2024-12-03 03:05_

---
