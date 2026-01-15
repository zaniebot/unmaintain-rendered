```yaml
number: 2521
title: Advanced generic call argument inference
type: issue
state: open
author: ibraheemdev
labels:
  - generics
  - bidirectional inference
assignees: []
created_at: 2026-01-15T16:51:25Z
updated_at: 2026-01-15T16:55:22Z
url: https://github.com/astral-sh/ty/issues/2521
synced_at: 2026-01-15T17:50:07Z
```

# Advanced generic call argument inference

---

_@ibraheemdev_

We currently only account for the call expression type annotation when inferring the arguments of a generic call. This means we cannot currently solve calls where arguments are dependent on one another, e.g.,
```py
def lst[T](x: T) -> list[T]:
    return [x]

def f[T](x: T, y: list[T]) -> T:
    return x

def _(x: int, y: int | str):
    _: int | str = f(y, lst(x))
    f(y, lst(x)) # Argument to function `f` is incorrect: Expected `list[int | str]`, found `list[int]`
```

Pyright handles the above case fine, but mypy seems to give up. I suspect pyright is using a heuristic where it infers "simple" argument expressions first, i.e., arguments that cannot be influenced by type context. However, this means it is not able to solve more advanced cases, e.g.,
```py
def lst[T](x: T) -> list[T]:
    return [x]

def f[T](x: T, y: list[T], z: list[T]) -> T:
    return x

def _(x: int, y: int | str, z: int | str | None):
    _: int | str | None = f(y, lst(x), lst(z))
    f(y, lst(x), lst(z)) # Argument to function `f` is incorrect: Expected `list[int | str | None]`, found `list[int]`
```

Another problem is that we consider the call expression type annotation even if it is covariant position, which can lead to false positives, e.g.,
```py
from typing import Sequence

def lst[T](x: T) -> list[T]:
    return [x]

def f[T](x: T, y: list[T], z: list[T]) -> Sequence[T]:
    return [x]

def _(x: int, z: list[int]):
    _: Sequence[int] = f(x, lst(x), z)
    _: Sequence[int | str] = f(x, lst(x), z) # Argument to function `f` is incorrect: Expected `list[int | str]`, found `list[int]`
```

Another example with `TypedDict`:
```py
from typing import TypedDict

class A(TypedDict):
    a: int
    b: int

def lst[T](x: T) -> list[T]:
    return [x]

def f[T](x: T, y: list[T]) -> T:
    return x

def _(a: A):
    _: A = f(a, lst({ "a": 1, "b": 2 }))
    f(a, lst({ "a": 1, "b": 2 })) # Argument to function `f` is incorrect: Expected `list[B | dict[Unknown | str, Unknown | int]]`, found `list[dict[Unknown | str, Unknown | int]]`
```

We may be able to solve this problem completely by lazily inferring arguments as constraint sets, and unifying them after all arguments and type context has been inferred (cc @dcreager). We could also implement a simpler heuristic similar to pyright.

Note that some of these false positives are being hidden by our `Unknown` unioning, and might start popping up more frequently after https://github.com/astral-sh/ty/issues/1240 is resolved.

---

_Label `generics` added by @ibraheemdev on 2026-01-15 16:51_

---

_Label `bidirectional inference` added by @ibraheemdev on 2026-01-15 16:51_

---

_Added to milestone `Stable` by @ibraheemdev on 2026-01-15 16:55_

---
