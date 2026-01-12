```yaml
number: 11780
title: "[red-knot] prototype type narrowing"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-06-06T20:28:50Z
updated_at: 2024-06-12T04:38:51Z
url: https://github.com/astral-sh/ruff/issues/11780
synced_at: 2026-01-12T15:54:51Z
```

# [red-knot] prototype type narrowing

---

_@carljm_

e.g. in

```
x = 1 if flag else None
if x is not None:
    x
```

we should know that the final `x` reference has type `Literal[1]`

---

_Label `red-knot` added by @carljm on 2024-06-06 20:28_

---

_Closed by @carljm on 2024-06-12 04:38_

---
