---
number: 2278
title: "Overloads don't work on decorated functions"
type: issue
state: open
author: ncollins-vs
labels:
  - overloads
assignees: []
created_at: 2025-12-30T15:41:36Z
updated_at: 2026-01-09T11:15:46Z
url: https://github.com/astral-sh/ty/issues/2278
synced_at: 2026-01-10T01:48:23Z
---

# Overloads don't work on decorated functions

---

_Issue opened by @ncollins-vs on 2025-12-30 15:41_

### Summary

```python
from typing import overload, ParamSpec, TypeVar, Callable
import functools

PS = ParamSpec("PS")
T = TypeVar("T")
def decorator(f: Callable[PS, T]) -> Callable[PS, T]:
    @functools.wraps(f)
    def wrapper(*args: PS.args, **kwargs: PS.kwargs) -> T:
        return f(*args, **kwargs)
    return wrapper


@overload
def test(x: int) -> int: ...
@overload
def test(*, y: str) -> str: ...
@decorator
def test(x: int | None = None, *, y: str | None = None) -> int | str:
    raise NotImplementedError

x = 1
reveal_type(x)
reveal_type(test(x))
```

If I comment out `@decorator`, ty correctly determines that `test(x)` is of type `int`. But with the decorator it incorrectly determines the type as `int | str` as if the overloads didn't exist.

[Playground link](https://play.ty.dev/24787475-a3a3-4ba9-9450-4bcdebc05f92)

### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Added to milestone `Stable` by @carljm on 2025-12-30 22:10_

---

_Label `overloads` added by @carljm on 2025-12-30 22:12_

---

_Comment by @carljm on 2025-12-30 22:13_

Thanks for the report! Definitely a bug.

---

_Comment by @aidandj on 2026-01-05 21:03_

Found a possibly related case, can open a new issue if it is different. When using overloads, I'm getting invalid argument errors when also using a decorator.

https://play.ty.dev/e7a7f281-da81-466d-ac99-e4f85ad38d4e

```
from contextlib import contextmanager
from typing import overload, Dict, Type, Iterator, TypeVar

class Base: ...

class Child(Base): ...

T = TypeVar("T", bound=Base)

@overload
@contextmanager
def begin(locks: Dict[Type[T], int]) -> Iterator[int]: ...

@overload
@contextmanager
def begin(locks: Dict[str, int]) -> Iterator[str]: ...

@contextmanager
def begin(locks: Dict[Type[T], int] | Dict[str, int]) -> Iterator[str] | Iterator[int]:
    yield 1

begin({Child: 1})

```

---

_Comment by @carljm on 2026-01-05 22:56_

It looks likely related to me, since removing / commenting the `@contextmanager` decorator(s) in that example makes the error go away and bidirectional inference then handles the dict literal argument fine.

---

_Removed from milestone `Stable` by @carljm on 2026-01-08 19:32_

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-08 19:32_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2026-01-09 11:15_

---
