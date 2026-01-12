```yaml
number: 11781
title: "[red-knot] flatten union types"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-06-06T20:32:04Z
updated_at: 2024-06-06T22:13:42Z
url: https://github.com/astral-sh/ruff/issues/11781
synced_at: 2026-01-12T15:54:51Z
```

# [red-knot] flatten union types

---

_@carljm_

e.g. in
```
class C1: pass
class C2: pass
class C3: pass
x = C1
if flag1:
    x = C2
elif flag2:
    x = C3
```

we should display the type as `(Literal[C1] | Literal[C2] | Literal[C3])`, without extraneous parens (representing extraneous nested unions we didn't flatten.)

---

_Label `red-knot` added by @carljm on 2024-06-06 20:32_

---

_Assigned to @carljm by @carljm on 2024-06-06 21:01_

---

_Closed by @carljm on 2024-06-06 22:13_

---
