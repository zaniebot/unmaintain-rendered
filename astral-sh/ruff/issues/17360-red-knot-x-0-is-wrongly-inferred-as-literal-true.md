```yaml
number: 17360
title: "[red-knot] `x != 0` is wrongly inferred as `Literal[True]` if type of `x` is any subtype of `~Literal[0]`"
type: issue
state: closed
author: MatthewMckee4
labels:
  - bug
  - ty
assignees: []
created_at: 2025-04-11T17:54:56Z
updated_at: 2025-04-16T05:27:28Z
url: https://github.com/astral-sh/ruff/issues/17360
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] `x != 0` is wrongly inferred as `Literal[True]` if type of `x` is any subtype of `~Literal[0]`

---

_@MatthewMckee4_

### Summary

From `type_api.md`

```py
from knot_extensions import static_assert

def f(x: int) -> None:
    if x != 0:
        static_assert(x != 0)
```

This test is incorrect. An [explanation by example](https://playknot.ruff.rs/13023a56-4887-4a29-83dd-201570ba6dde) thanks to @AlexWaygood 

We cannot make any assumptions about `int & ~Literal[0]` here. I'm not yet sure where this error comes from.

### Version

_No response_

---

_Renamed from "Incorrect static assert on constrained type" to "[red-knot] Incorrect static assert on constrained type" by @MatthewMckee4 on 2025-04-11 17:58_

---

_Comment by @sharkdp on 2025-04-11 18:01_

Thank you.

The core problem is that the revealed type here is wrong:
```py
from typing import reveal_type

def f(x: int) -> None:
    if x != 0:
        reveal_type(x != 0)  # Literal[True]; should be bool.
```

---

_Label `bug` added by @sharkdp on 2025-04-11 18:01_

---

_Label `red-knot` added by @sharkdp on 2025-04-11 18:01_

---

_Comment by @AlexWaygood on 2025-04-11 18:19_

I'd consider this a minimal repro:

```py
from knot_extensions import Not
from typing import Literal

def f(x: Not[Literal[0]]):
    reveal_type(x == 0)
```

The `reveal_type` call on the last line reveals `Literal[False]`, but there are many objects that compare equal to `0` that inhabit the type `~Literal[0]`. `False` and `0.0` are two examples.

---

_Comment by @sharkdp on 2025-04-11 18:20_

It's probably this code that needs to be fixed (or simply removed?)

https://github.com/astral-sh/ruff/blob/e5026c08776164cc39dce36c0e657f781de7184d/crates/red_knot_python_semantic/src/types/infer.rs#L5258-L5285

---

_Comment by @AlexWaygood on 2025-04-11 18:25_

> It's probably this code that needs to be fixed (or simply removed?)

I _think_ we can keep the branches for `CmpOp::Is` and `CmpOp::IsNot`, but the branches for `CmpOp::Eq` and `CmpOp::NotEq` look like they need to go, yeah

---

_Renamed from "[red-knot] Incorrect static assert on constrained type" to "[red-knot] `x != 0` is wrongly inferred as `Literal[True]` if type of `x` is `int & ~Literal[0]`" by @carljm on 2025-04-12 00:44_

---

_Renamed from "[red-knot] `x != 0` is wrongly inferred as `Literal[True]` if type of `x` is `int & ~Literal[0]`" to "[red-knot] `x != 0` is wrongly inferred as `Literal[True]` if type of `x` is any subtype of `~Literal[0]`" by @AlexWaygood on 2025-04-13 15:10_

---

_Closed by @carljm on 2025-04-16 05:27_

---

_Closed by @carljm on 2025-04-16 05:27_

---
