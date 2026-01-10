---
number: 1235
title: "`__getattr__` causes unsafety on subclass declarations"
type: issue
state: open
author: KotlinIsland
labels:
  - unsoundness
assignees: []
created_at: 2025-09-22T02:40:24Z
updated_at: 2026-01-09T02:56:39Z
url: https://github.com/astral-sh/ty/issues/1235
synced_at: 2026-01-10T01:48:23Z
---

# `__getattr__` causes unsafety on subclass declarations

---

_Issue opened by @KotlinIsland on 2025-09-22 02:40_

### Summary

```py
class A:
    known: str
    def __getattr__(self, attr: str) -> int:
        return 1
class B(A):
    also_known: str

def f(a: A = B()):
    a.also_known  # ty: int, runtime: str
```

an error should be shown on the declaration of `also_known`

### Version

_No response_

---

_Label `attribute access` added by @AlexWaygood on 2025-09-22 07:40_

---

_Renamed from "`__getattr__` is unsafe on subclasses" to "`__getattr__` causes unsafety on subclass declarations" by @KotlinIsland on 2025-09-22 08:55_

---

_Label `attribute access` removed by @carljm on 2025-09-22 21:47_

---

_Label `unsoundness` added by @carljm on 2026-01-09 02:55_

---

_Comment by @carljm on 2026-01-09 02:56_

This is effectively a Liskov check specific to inheriting from classes that implement `__getattr__` -- subclasses should not be allowed to define any named attribute not defined on the base class, with a type that is not assignable to the return type of `__getattr__`.

---
