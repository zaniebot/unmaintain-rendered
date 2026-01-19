```yaml
number: 2567
title: "Branching on the value/type of a field of variable whose type is a union doesn't narrow the type of the variable"
type: issue
state: closed
author: Terramorpha
labels: []
assignees: []
created_at: 2026-01-19T21:37:44Z
updated_at: 2026-01-19T22:02:29Z
url: https://github.com/astral-sh/ty/issues/2567
synced_at: 2026-01-19T22:34:58Z
```

# Branching on the value/type of a field of variable whose type is a union doesn't narrow the type of the variable

---

_@Terramorpha_

### Summary

Type narrowing doesn't work correctly when branching on the field of a union. This prevents using `tag: Literal["<type>"]` like is common in typescript.


playground link: https://play.ty.dev/57856d74-0335-4c91-b3cc-4eb77e0fec24

### Version

ty 0.0.12

---

_Comment by @AlexWaygood on 2026-01-19 22:02_

Thanks!

---

_Closed by @AlexWaygood on 2026-01-19 22:02_

---
