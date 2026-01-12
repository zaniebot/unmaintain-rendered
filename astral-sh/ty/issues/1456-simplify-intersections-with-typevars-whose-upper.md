```yaml
number: 1456
title: Simplify intersections with typevars whose upper bound is covered by a union of negative elements
type: issue
state: closed
author: sharkdp
labels:
  - narrowing
  - generics
  - control flow
  - set-theoretic types
assignees: []
created_at: 2025-10-30T14:37:38Z
updated_at: 2025-10-31T20:51:13Z
url: https://github.com/astral-sh/ty/issues/1456
synced_at: 2026-01-12T15:54:25Z
```

# Simplify intersections with typevars whose upper bound is covered by a union of negative elements

---

_@sharkdp_

There appear to be lots of other cases where you can have a TypeVar bound to a union (or type equivalent to a union) where we also exhibit similar issues with reachability and narrowing:

```py
from typing import assert_never, Literal

def f[T: bool](x: T) -> T:
    match x:
        case True:
            return x
        case False:
            return x
        case _:
            reveal_type(x) # T@f & ~Literal[True] & ~Literal[False]
            assert_never(x)  # false-positive error

def g[T: Literal["foo", "bar"]](x: T) -> T:
    match x:
        case "foo":
            return x
        case "bar":
            return x
        case _:
            reveal_type(x)  # T@g & ~Literal["foo"] & ~Literal["bar"]
            assert_never(x)  # false-positive error

def h[T: int | str](x: T) -> T:
    if isinstance(x, int):
        return x
    elif isinstance(x, str):
        return x
    else:
        reveal_type(x)  # T@h & ~int & ~str
        assert_never(x)  # false-positive error
```

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/pull/21130#pullrequestreview-3399762902_
            

---

_Renamed from "Simplify intersections with Typevars whose upper bound is covered by a union of negative elements" to "Simplify intersections with typevars whose upper bound is covered by a union of negative elements" by @sharkdp on 2025-10-30 14:37_

---

_Label `set-theoretic types` added by @sharkdp on 2025-10-30 14:38_

---

_Added to milestone `GA` by @sharkdp on 2025-10-30 14:38_

---

_Label `narrowing` added by @AlexWaygood on 2025-10-30 14:40_

---

_Label `generics` added by @AlexWaygood on 2025-10-30 14:40_

---

_Label `control flow` added by @AlexWaygood on 2025-10-30 14:40_

---

_Closed by @AlexWaygood on 2025-10-31 20:51_

---
