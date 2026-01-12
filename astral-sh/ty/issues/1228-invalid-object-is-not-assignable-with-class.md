```yaml
number: 1228
title: "invalid \"object is not assignable\" with class-scoped generic"
type: issue
state: closed
author: KotlinIsland
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2025-09-21T23:47:35Z
updated_at: 2025-11-10T21:49:50Z
url: https://github.com/astral-sh/ty/issues/1228
synced_at: 2026-01-12T15:54:24Z
```

# invalid "object is not assignable" with class-scoped generic

---

_@KotlinIsland_

### Summary

```py
from __future__  import annotations

class A[T]:
    def __init__(self, value: T) -> None:
        self.t: T = value
    def f(self, value: A[T]):
        _: A[T | int] = A(value.t)  # Object of type `A[T@A]` is not assignable to `A[T@A | int]`
```

### Version

_No response_

---

_Label `generics` added by @AlexWaygood on 2025-09-22 07:50_

---

_Comment by @sharkdp on 2025-09-22 07:55_

Thank you for reporting this.

Isn't `A` invariant in `T` here? So should that assignment really be allowed?

---

_Comment by @AlexWaygood on 2025-09-22 07:58_

I think ideally we would use the type context of the l.h.s. annotation to inform the way we solve the TypeVar in the constructor call to `A`, and therefore our inferred type of the r.h.s.. Done correctly, that would mean that we never actually infer `A[T@A]` for the r.h.s. at all — we would instead infer the broader type `A[T@A | int]`, which is of course the same type as the l.h.s. annotation and therefore assignable to the l.h.s. annotation even though the TypeVar is invariant here

---

_Comment by @sharkdp on 2025-09-22 08:04_

Ah yes, of course. Thanks.

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-09-22 13:18_

---

_Renamed from "invalid "object is not assignable" with class scoped generic" to "invalid "object is not assignable" with class-scoped generic" by @AlexWaygood on 2025-09-22 16:23_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-10-17 14:40_

---

_Unassigned @ibraheemdev by @ibraheemdev on 2025-10-17 14:40_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2025-10-17 14:40_

---

_Added to milestone `Beta` by @ibraheemdev on 2025-10-17 14:40_

---

_Closed by @ibraheemdev on 2025-11-10 21:49_

---
