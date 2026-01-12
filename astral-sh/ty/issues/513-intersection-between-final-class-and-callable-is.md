```yaml
number: 513
title: "Intersection between @final class and Callable is not reduced to Never"
type: issue
state: closed
author: JelleZijlstra
labels:
  - bug
  - set-theoretic types
assignees: []
created_at: 2025-05-26T01:27:47Z
updated_at: 2025-05-29T23:27:28Z
url: https://github.com/astral-sh/ty/issues/513
synced_at: 2026-01-12T15:54:23Z
```

# Intersection between @final class and Callable is not reduced to Never

---

_@JelleZijlstra_

### Summary

An intersection like `bool & Callable[..., int]` should be Never, because bool can't have any subclasses that are callable. But ty treats it as irreducible.

```python
from typing import Callable, reveal_type, Literal, final
from ty_extensions import Intersection

@final
class C:
    pass

def f(
    x: Intersection[bool, Callable[..., int]],
    y: Intersection[Literal[1], Callable[..., int]],
    z: Intersection[C, Callable[..., int]],
):
    reveal_type(x)  # bool & ((...) -> int), expect Never
    reveal_type(y)  # Never (correct)
    reveal_type(z)  # C & (...) -> int), expect Never
```

https://play.ty.dev/979ab089-7157-44ad-a94a-868260108090

An intersection between a final class and a non-applicable Protocol is correctly reduced to Never though.

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-05-26 06:38_

---

_Label `set-theoretic types` added by @AlexWaygood on 2025-05-26 06:38_

---

_Comment by @sharkdp on 2025-05-26 12:44_

Thank you for reporting this!

The underlying reason is that we don't yet model disjointness for `Callable` types correctly ([playground](https://play.ty.dev/2470c350-48de-4330-88c7-d358a3fbfa86)).

The corresponding TODO in the code is here:

https://github.com/astral-sh/ruff/blob/d8216fa3282c89b0e5827b8da5f5f2aefd55ad4a/crates/ty_python_semantic/src/types.rs#L2176-L2186

---

_Closed by @carljm on 2025-05-29 23:27_

---
