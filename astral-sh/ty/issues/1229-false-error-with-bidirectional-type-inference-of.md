```yaml
number: 1229
title: false error with bidirectional type inference of type variable resolution
type: issue
state: closed
author: KotlinIsland
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-09-21T23:57:48Z
updated_at: 2025-11-12T19:22:39Z
url: https://github.com/astral-sh/ty/issues/1229
synced_at: 2026-01-12T15:54:24Z
```

# false error with bidirectional type inference of type variable resolution

---

_@KotlinIsland_

### Summary

```py
from __future__ import annotations

class A[T]:
    def __init__(self, value: T):
        self.t: T = value
    def f[S](self, value: A[S | T], /) -> A[S | T]:
        return A(value.t)


_: A[object] = A(1).f(A(2))  # Argument to bound method `f` is incorrect: Expected `A[object]`, found `A[int]`
```
`A(2)` should be inferred as `A[object]`, not `A[int]`

### Version

_No response_

---

_Label `generics` added by @AlexWaygood on 2025-09-22 07:48_

---

_Renamed from "false error with bi directional type inference of type variable resolution" to "false error with bidirectional type inference of type variable resolution" by @AlexWaygood on 2025-09-22 08:03_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-09-22 13:18_

---

_Comment by @ibraheemdev on 2025-11-12 19:22_

This was fixed by https://github.com/astral-sh/ruff/pull/21210 and no longer errors on main.

---

_Closed by @ibraheemdev on 2025-11-12 19:22_

---
