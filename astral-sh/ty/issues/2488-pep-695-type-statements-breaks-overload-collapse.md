```yaml
number: 2488
title: "PEP 695 `type` statements breaks overload collapse"
type: issue
state: closed
author: flying-sheep
labels: []
assignees: []
created_at: 2026-01-14T10:29:05Z
updated_at: 2026-01-15T06:25:10Z
url: https://github.com/astral-sh/ty/issues/2488
synced_at: 2026-01-15T06:49:13Z
```

# PEP 695 `type` statements breaks overload collapse

---

_@flying-sheep_

### Summary

https://play.ty.dev/26ba098e-7e20-472d-a2ba-ec5cbfd73e71

I could reduce my example all the way to this.
The `evaluate(x, eager=eager)` will only typecheck if I remove the “type” from line 5

```py
from typing import overload, Literal

class Eager: ...
class Lazy: ...
type Array = Eager | Lazy  # these work: `Array = …`, `Array: TypeAlias = …`

@overload
def evaluate(x: Eager, /, *, eager: bool = False) -> Eager: ...
@overload
def evaluate(x: Lazy, /, *, eager: Literal[False] = False) -> Lazy: ...
@overload
def evaluate(x: Lazy, /, *, eager: Literal[True]) -> Eager: ...
def evaluate(*args, **kw): ...

def foo(x: Array, *, eager: bool) -> None:
    evaluate(x, eager=eager)
```

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Renamed from "PEP 695 `type` statements break overload collapse" to "PEP 695 `type` statements breaks overload collapse" by @flying-sheep on 2026-01-14 10:29_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-15 00:11_

---

_Removed from milestone `Pre-stable 1` by @carljm on 2026-01-15 00:11_

---

_Added to milestone `Stable` by @carljm on 2026-01-15 00:11_

---

_Closed by @carljm on 2026-01-15 06:25_

---
