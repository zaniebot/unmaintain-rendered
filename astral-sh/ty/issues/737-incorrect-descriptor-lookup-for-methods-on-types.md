```yaml
number: 737
title: "Incorrect descriptor lookup for methods on types that are not disjoint from `None`"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - attribute access
  - Protocols
assignees: []
created_at: 2025-07-01T10:55:17Z
updated_at: 2025-11-14T15:31:27Z
url: https://github.com/astral-sh/ty/issues/737
synced_at: 2026-01-12T15:54:23Z
```

# Incorrect descriptor lookup for methods on types that are not disjoint from `None`

---

_@AlexWaygood_

### Summary

Other than `object`, all nominal instance types are disjoint from `None`. But... this is not true for instances of structural (`Protocol`) types -- and it turns out that this causes issues for our current implementation of the descriptor protocol.

For example, compare and contrast the different behaviour of these two protocols. `SupportsFoo` is disjoint from `None`; `SupportsStr` is not:

```py
from typing import Protocol

class SupportsFoo(Protocol):
    def foo(self) -> str: ...

class SupportsStr(Protocol):
    def __str__(self) -> str: ...

def f(f: SupportsFoo, s: SupportsStr):
    reveal_type(f.foo)       # revealed: `bound method SupportsFoo.foo() -> str`
    f.foo()                  # no diagnostic

    reveal_type(s.__str__)   # revealed: `def __str__(self) -> str`
    s.__str__()              # error: No argument provided for required parameter `self` of function `__str__` (missing-argument)
```

https://play.ty.dev/09505473-95b5-4b51-b992-7784595bcf55

Whether or not a type is disjoint from `None` impacts our understanding of the way the descriptor protocol is invoked when a method is accessed on that type. The type that a method is accessed on is passed to `FunctionType.__get__` when we invoke the descriptor protocol; if that type is not disjoint from `None`, we end up having to pick the first overload here, which means that the attribute access is (incorrectly!) evaluated as resolving to the original function object rather than a bound method:

https://github.com/astral-sh/ruff/blob/c6fd11fe3694646c7b5667e37dfc67b114e2f50a/crates/ty_python_semantic/src/types.rs#L3526-L3575

Many thanks to @sharkdp who helped me track this bug down over the last couple of days!

### Version

c6fd11fe3

---

_Label `bug` added by @AlexWaygood on 2025-07-01 10:55_

---

_Label `attribute access` added by @AlexWaygood on 2025-07-01 10:55_

---

_Label `Protocols` added by @AlexWaygood on 2025-07-01 10:55_

---

_Renamed from "Incorrect descriptor lookup for methods on protocols that are not disjoint from `None`" to "Incorrect descriptor lookup for methods on structural types that are not disjoint from `None`" by @AlexWaygood on 2025-07-01 15:27_

---

_Comment by @AlexWaygood on 2025-07-01 15:28_

The bug can also be reproduced when looking up methods on other structural types that are not disjoint from `None`, e.g.

```py
from ty_extensions import AlwaysFalsy

def f(x: AlwaysFalsy):
    x.__hash__()  # error: No argument provided for required parameter `self` of function `__hash__` (missing-argument)
```

https://play.ty.dev/5bb27bef-59f6-4af7-b017-5083aaaa4c33

---

_Comment by @carljm on 2025-07-02 00:40_

This can even repro with `None` itself, without any protocols involved: https://play.ty.dev/80e50768-24d9-4d36-a8dd-347a356f90a9

I vaguely recall running into this in the runtime before, and realizing that even the runtime has to do some special-casing for accessing methods on `None` to avoid this problem.

What this means is that I think everything we are doing is correct here; this is a fundamental issue with the descriptor protocol using `None` as a sentinel. I think it can probably only be fixed with some additional special-casing in the attribute lookup code.

---

_Comment by @pjonsson on 2025-07-02 13:32_

I find it difficult to navigate the issues, my apologies if this is the wrong issue for my comment.

> This can even repro with `None` itself, without any protocols involved: https://play.ty.dev/80e50768-24d9-4d36-a8dd-347a356f90a9

I'm guessing I have an instance of the None issue mentioned in the previous comment because I don't think there are any protocols involved in the minimized reproducer I have:

```python
from typing import Any

def f(g: Any) -> None:
    try:
        g()
    except Exception as e:
        print(e.__cause__.__str__())
```

As a side note, today was my first time trying ty and it looks great, can't wait to use it for real!

---

_Comment by @carljm on 2025-07-02 17:06_

@pjonsson Yes, that does look like an example of this issue! It also highlights the fact that "union with None" is another scenario that can trigger this behavior.

---

_Comment by @AlexWaygood on 2025-07-03 11:51_

Since this bug manifests for any type _not disjoint_ from `None`, it even manifests for method calls on `object` instances:

```py
def f(x: object):
    x.__str__()  # error: No argument provided for required parameter `self` of function `__str__` (missing-argument)
```

https://play.ty.dev/bb76c82a-6a33-43ce-99dd-a4bbca762c7e

---

_Renamed from "Incorrect descriptor lookup for methods on structural types that are not disjoint from `None`" to "Incorrect descriptor lookup for methods on types that are not disjoint from `None`" by @AlexWaygood on 2025-07-03 17:47_

---

_Comment by @AlexWaygood on 2025-07-05 18:36_

https://github.com/astral-sh/ruff/pull/19120 fixed this for protocols that overlap with `None`, `object`, `AlwaysTruthy` and `AlwaysFalsy`. The problem still exists for calling methods on `None` itself, however (and therefore also still exists for calling methods on unions that include `None`).

---

_Added to milestone `Stable` by @carljm on 2025-11-14 15:31_

---
