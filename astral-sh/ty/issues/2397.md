---
number: 2397
title: "When a type variable has two upper bounds, ty can't deduce that they both satisfy a constraint"
type: issue
state: closed
author: mitya57
labels: []
assignees: []
created_at: 2026-01-08T15:48:35Z
updated_at: 2026-01-08T15:54:43Z
url: https://github.com/astral-sh/ty/issues/2397
synced_at: 2026-01-10T01:51:14Z
---

# When a type variable has two upper bounds, ty can't deduce that they both satisfy a constraint

---

_Issue opened by @mitya57 on 2026-01-08 15:48_

### Summary

A very simple example:
```python
def compare[V: (int, float)](a: V, b: V):
    return a < b
```

And `ty check` reports:
```
error[unsupported-operator]: Unsupported `<` operation
 --> main.py:2:12
  |
1 | def compare[V: (int, float)](a: V, b: V):
2 |     return a < b
  |            -^^^-
  |            |
  |            Both operands have type `V@compare`
  |
info: rule `unsupported-operator` is enabled by default
```

Playground: <https://play.ty.dev/faba8849-17f9-45b3-842c-139650b7c9d9>.

### Version

ty 0.0.10

---

_Comment by @AlexWaygood on 2026-01-08 15:54_

thanks! this is the same as #1972

---

_Closed by @AlexWaygood on 2026-01-08 15:54_

---
