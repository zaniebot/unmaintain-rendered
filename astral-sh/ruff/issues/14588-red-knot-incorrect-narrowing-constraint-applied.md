```yaml
number: 14588
title: "[red-knot] Incorrect Narrowing Constraint Applied After ExprIf"
type: issue
state: closed
author: cake-monotone
labels:
  - bug
  - ty
assignees: []
created_at: 2024-11-25T17:04:40Z
updated_at: 2024-11-25T18:36:39Z
url: https://github.com/astral-sh/ruff/issues/14588
synced_at: 2026-01-12T15:54:54Z
```

# [red-knot] Incorrect Narrowing Constraint Applied After ExprIf

---

_@cake-monotone_

```py
def bool_instance() -> bool:
    return True

x: Literal[42, "hello"] = 42 if bool_instance() else "hello"
reveal_type(x)  # revealed: Literal[42] | Literal["hello"]

_ = 1234 if isinstance(x, str) else 1234

# The `isinstance` test incorrectly narrows the type of `x`.
# As a result, `x` is revealed as Literal["hello"], but it should remain Literal[42, "hello"].
reveal_type(x)  # revealed: Literal["hello"]
```

This issue is not limited to `isinstance`. Other narrowing expressions or patterns also cause the same issue.

---

_Comment by @cake-monotone on 2024-11-25 17:15_

If this isn't resolved first, #14550 could break some tests.
I'll take a look into it first.

---

_Renamed from "[red-knot] Incorrect Narrowing Constraint Applied After IfExp" to "[red-knot] Incorrect Narrowing Constraint Applied After ExprIf" by @cake-monotone on 2024-11-25 17:22_

---

_Label `red-knot` added by @MichaReiser on 2024-11-25 17:23_

---

_Label `bug` added by @AlexWaygood on 2024-11-25 17:42_

---

_Closed by @carljm on 2024-11-25 18:36_

---

_Closed by @carljm on 2024-11-25 18:36_

---
