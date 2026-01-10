---
number: 2131
title: "Function returning `Awaitable[R]` returns `T | None` on Python 3.13 but `T` on Python 3.12"
type: issue
state: open
author: yamaaaaaa31
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-20T18:02:59Z
updated_at: 2026-01-09T21:30:46Z
url: https://github.com/astral-sh/ty/issues/2131
synced_at: 2026-01-10T01:48:23Z
---

# Function returning `Awaitable[R]` returns `T | None` on Python 3.13 but `T` on Python 3.12

---

_Issue opened by @yamaaaaaa31 on 2025-12-20 18:02_

## Description

When using a function decorated with `@overload` as a method decorator, ty incorrectly infers the return type as `T | None` on Python 3.13, but correctly infers `T` on Python 3.12.

## Reproduction

```python
# log_fn.py
import asyncio
from collections.abc import Awaitable, Callable
from functools import wraps
from typing import Any, ParamSpec, TypeVar, overload

P = ParamSpec("P")
R = TypeVar("R")

@overload
def log_fn(fn: Callable[P, Awaitable[R]]) -> Callable[P, Awaitable[R]]: ...
@overload
def log_fn(fn: Callable[P, R]) -> Callable[P, R]: ...
def log_fn(fn: Any) -> Any:
    if asyncio.iscoroutinefunction(fn):
        @wraps(fn)
        async def async_wrapper(*args: Any, **kwargs: Any) -> Any:
            return await fn(*args, **kwargs)
        return async_wrapper
    @wraps(fn)
    def wrapper(*args: Any, **kwargs: Any) -> Any:
        return fn(*args, **kwargs)
    return wrapper
```

```python
# use_case.py
from dataclasses import dataclass
from injector import inject
from log_fn import log_fn

@dataclass
class Result:
    value: str

@inject
@dataclass
class UseCase:
    @log_fn
    async def run(self) -> Result:
        return Result(value="test")
```

```python
# router.py
from use_case import UseCase, Result

async def main():
    result = await UseCase().run()
    print(result.value)  # Error on 3.13: possibly-missing-attribute
```

## Commands

```bash
ty check --python-version 3.12 router.py  # All checks passed!
ty check --python-version 3.13 router.py  # Error: Attribute 'value' may be missing on object of type 'Result | None'
```

## Expected

No error on Python 3.13 (same behavior as Python 3.12)

## Actual

`possibly-missing-attribute` and `invalid-argument-type` errors on Python 3.13:

```
error[possibly-missing-attribute]: Attribute 'value' may be missing on object of type 'Result | None'
```

## Environment

- ty version: 0.0.4
- OS: macOS

---

_Comment by @MeGaGiGaGon on 2025-12-21 07:42_

Not sure what's going on here, but I made a smaller repro on the playground that shows some interesting things:
https://play.ty.dev/1ddf219d-1ad3-426a-bcb2-cb07fb152237
```py
from collections.abc import Awaitable, Callable

def log_fn[R, **P](fn: Callable[P, Awaitable[R]]) -> Callable[P, Awaitable[R]]:
    raise NotImplementedError

@log_fn
async def run() -> int:
    return 1

async def main():
    reveal_type(await run())  # `Unknown` on 3.12, `int | None` on 3.13
```
The noteworthy thing being type solving is completely not working on 3.12, giving `Unknown`, so this actually has nothing to do with overloads, and is two separate issues depending on the version.

---

_Comment by @carljm on 2025-12-22 20:17_

Thanks for the report, and thanks @MeGaGiGaGon for further minimization!

It looks like the use of `Awaitable[R]` as the return type in `log_fn` is key -- if `log_fn` is changed to be a totally transparent decorator:

```py
def log_fn[R, **P](fn: Callable[P, R]) -> Callable[P, R]:
    raise NotImplementedError
```

then on all Python versions we see `run` as returning `CoroutineType[int, None, None]` and we infer the type of `await run()` as `int`.

With the `Awaitable`, even on 3.13 it's odd that we get the type `int | None`; not sure where the `None` is coming from.

I suspect there is a connection to #1714 here.

---

_Added to milestone `M1` by @carljm on 2025-12-22 20:17_

---

_Comment by @AlexWaygood on 2025-12-22 22:07_

> I suspect there is a connection to https://github.com/astral-sh/ty/issues/1714 here.

Though `types.CoroutineType` explicitly inherits from `typing.Coroutine` (and therefore `typing.Awaitable`) in typeshed, so I'd expect our generics solver to be able to handle this even without #1714

---

_Label `bug` added by @dhruvmanila on 2025-12-23 05:33_

---

_Label `generics` added by @dhruvmanila on 2025-12-23 05:33_

---

_Renamed from "@overload decorated function returns T | None on Python 3.13 but T on Python 3.12" to "Function returning `Awaitable[R]` returns `T | None` on Python 3.13 but `T` on Python 3.12" by @dhruvmanila on 2025-12-23 05:40_

---

_Comment by @yamaaaaaa31 on 2025-12-24 06:25_

Thanks @MeGaGiGaGon for the minimal repro and @carljm @AlexWaygood for investigating!

Good to know it's been added to Pre-stable1. Looking forward to the fix 🙏

---

_Comment by @yilei on 2026-01-06 04:38_

We are running into (I believe) the same issue here, and I have another repro if it's helpful:

```python
from collections.abc import Awaitable, Callable
from typing import TypeVar

S = TypeVar("S")
T = TypeVar("T")


async def call(f: Callable[[S], Awaitable[T]], x: S) -> T:
    return await f(x)


async def f(s: str) -> str:
    return s


async def main() -> str:
    return await call(f, "")
```

It passes when targeting 3.12, but 3.13+ fails with:

```
error[invalid-return-type]: Return type does not match returned value
  --> ty_bug.py:16:21
   |
16 | async def main() -> str:
   |                     --- Expected `str` because of return type
17 |     return await call(f, "")
   |            ^^^^^^^^^^^^^^^^^ expected `str`, found `str | None`
   |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
```

---

_Comment by @carljm on 2026-01-07 01:27_

One interesting note here: `CoroutineType.__await__` returns `Generator[Any, None, _ReturnT_nd_co]` -- that `None` there seems reasonably likely to be the source of the spurious `None` we see in this bug. The root cause may be related to https://github.com/astral-sh/ty/issues/2371

---

_Assigned to @dcreager by @dcreager on 2026-01-09 13:45_

---

_Comment by @carljm on 2026-01-09 21:15_

The 3.12 vs 3.13 distinction here is probably related to the fact that the `Generator` type in typeshed only uses its `ReturnT_co` typevar on 3.13+. This causes us to wrongly think (on 3.12 and lower) that two different `Generator` types that differ only in their `ReturnT_co` are actually equivalent types.

We will probably need to do some special-casing here to work around typeshed's limitations. In fact, for async functions (which return a `CoroutineType` which when awaited returns a `Generator` type), the `ReturnT_co` is crucial -- that's the ultimate return type of the async function. But the mechanism by which it's transmitted at runtime is via `StopIteration` exception (side note: internally the runtime usually optimizes this away, but in principle it's still true), which is not visible as part of the interface of `Generator`.

---

_Comment by @carljm on 2026-01-09 21:30_

I filed https://github.com/astral-sh/ty/issues/2426 to more clearly track the issue with the `Generator` type on Python 3.12, which I'm sure is _related_ to this issue, although there may be more to this issue in addition.

---
