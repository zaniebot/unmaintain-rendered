```yaml
number: 157
title: "Support `ParamSpec`"
type: issue
state: closed
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-03-26T18:04:49Z
updated_at: 2025-12-05T16:37:30Z
url: https://github.com/astral-sh/ty/issues/157
synced_at: 2026-01-10T01:56:40Z
```

# Support `ParamSpec`

---

_Issue opened by @carljm on 2025-03-26 18:04_

_No description provided._

---

_Renamed from "[red-knot] support ParamSpec" to "support ParamSpec" by @MichaReiser on 2025-05-07 15:25_

---

_Comment by @danielkelshaw on 2025-05-07 16:43_

Just a quick example that doesn't work:

```python
import typing as tp


def my_decorator[**P, T](fn: tp.Callable[P, T]) -> tp.Callable[P, T]:

    def wrapped(*args: P.args, **kwargs: P.kwargs) -> T:
        print('in decorator..')
        return fn(*args, **kwargs)
    
    return wrapped


@my_decorator
def my_adder(a: int, b: int) -> int:
    return a + b

print(my_adder(1, 3))
```

This gives the errors:

```bash
error: lint:invalid-type-form: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
 --> main.py:4:42
  |
4 | def my_decorator[**P, T](fn: tp.Callable[P, T]) -> tp.Callable[P, T]:
  |                                          ^
5 |
6 |     def wrapped(*args: P.args, **kwargs: P.kwargs) -> T:
  |

error: lint:invalid-type-form: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
 --> main.py:4:64
  |
4 | def my_decorator[**P, T](fn: tp.Callable[P, T]) -> tp.Callable[P, T]:
  |                                                                ^
5 |
6 |     def wrapped(*args: P.args, **kwargs: P.kwargs) -> T:
  |

Found 2 diagnostics

```

---

_Comment by @dhruvmanila on 2025-05-07 16:46_

I think it would be useful to not error in this case even without the support of `ParamSpec`, not sure how hard that would be.

---

_Comment by @sharkdp on 2025-05-10 09:50_

> I think it would be useful to not error in this case even without the support of `ParamSpec`, not sure how hard that would be.

See https://github.com/astral-sh/ruff/pull/18001

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:55_

---

_Removed from milestone `GA` by @carljm on 2025-06-11 00:46_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:46_

---

_Comment by @Dr-Irv on 2025-06-30 00:24_

Here's another example where `ParamSpec` is not accepted as an argument to `Protocol`.  This example works fine with `pyright`, `mypy` and `pyrefly`:

```python
from typing import Protocol, TypeVar, ParamSpec

# Define a TypeVar for the return type of the callable
R = TypeVar('R', covariant=True)

# Define a ParamSpec to capture the parameters of the callable
P = ParamSpec('P')

class LoggableFunction(Protocol[P, R]):
    def __call__(self, *args: P.args, **kwargs: P.kwargs) -> R:
        ...
```

---

_Assigned to @dhruvmanila by @carljm on 2025-08-19 15:00_

---

_Comment by @dmytro-GL on 2025-09-04 10:46_

Here is another one:

```
from collections.abc import Callable
from typing import ParamSpec, TypeVar

T = TypeVar("T")
P = ParamSpec("P")

# case #1
def wrapper(fn: Callable[P, T]) -> Callable[P, T]:
    def wrapped(*args: P.args, **kwargs: P.kwargs) -> T:
        print("msg")
        return fn(*args, **kwargs)
    return wrapped

# case #2
def ex2(msg: str):
    def wrapper(fn: Callable[P, T]) -> Callable[P, T]:
        def wrapped(*args: P.args, **kwargs: P.kwargs) -> T:
            print(msg)
            return fn(*args, **kwargs)
        return wrapped
    return wrapper

# case #3
def ex3(msg: str):
    P = ParamSpec("P")
    def wrapper(fn: Callable[P, T]) -> Callable[P, T]:
        def wrapped(*args: P.args, **kwargs: P.kwargs) -> T:
            print(msg)
            return fn(*args, **kwargs)
        return wrapped
    return wrapper
```

```
$ uvx ty@0.0.1a20 check x.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                                 error[invalid-type-form]: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
  --> x.py:16:30
   |
14 | # case #2
15 | def ex2(msg: str):
16 |     def wrapper(fn: Callable[P, T]) -> Callable[P, T]:
   |                              ^
17 |         def wrapped(*args: P.args, **kwargs: P.kwargs) -> T:
18 |             print(msg)
   |
info: See the following page for a reference on valid type expressions:
info: https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions
info: rule `invalid-type-form` is enabled by default

error[invalid-type-form]: The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
  --> x.py:16:49
   |
14 | # case #2
15 | def ex2(msg: str):
16 |     def wrapper(fn: Callable[P, T]) -> Callable[P, T]:
   |                                                 ^
17 |         def wrapped(*args: P.args, **kwargs: P.kwargs) -> T:
18 |             print(msg)
   |
info: See the following page for a reference on valid type expressions:
info: https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions
info: rule `invalid-type-form` is enabled by default

Found 2 diagnostics
```

Case 2 doesn't work. pyright doesn't report any issues in all three cases.

---

_Comment by @merlinz01 on 2025-11-19 19:32_

An example from Starlette middleware that gives this error:

```python
import sys
from typing import Any, Awaitable, Callable, MutableMapping, Protocol

if sys.version_info >= (3, 10):
    from typing import ParamSpec
else:
    from typing_extensions import ParamSpec

Scope = MutableMapping[str, Any]
Message = MutableMapping[str, Any]
Receive = Callable[[], Awaitable[Message]]
Send = Callable[[Message], Awaitable[None]]

ASGIApp = Callable[[Scope, Receive, Send], Awaitable[None]]
P = ParamSpec("P")


class _MiddlewareFactory(Protocol[P]):
    def __call__(
        self, app: ASGIApp, /, *args: P.args, **kwargs: P.kwargs
    ) -> ASGIApp: ...


class Starlette:
    def add_middleware(
        self,
        middleware_class: _MiddlewareFactory[P],
        *args: P.args,
        **kwargs: P.kwargs,
    ) -> None: ...


class MyMiddleware:
    def __init__(self, app: ASGIApp) -> None: ...

    async def __call__(self, scope: Scope, receive: Receive, send: Send) -> None: ...


app = Starlette()
app.add_middleware(MyMiddleware)
```
```
error[invalid-argument-type]: Argument to bound method `add_middleware` is incorrect
  --> test.py:40:20
   |
39 | app = Starlette()
40 | app.add_middleware(MyMiddleware)
   |                    ^^^^^^^^^^^^ Expected `_MiddlewareFactory[Unknown]`, found `<class 'MyMiddleware'>`
   |
info: Method defined here
  --> test.py:25:9
   |
24 | class Starlette:
25 |     def add_middleware(
   |         ^^^^^^^^^^^^^^
26 |         self,
27 |         middleware_class: _MiddlewareFactory[P],
   |         --------------------------------------- Parameter declared here
28 |         *args: P.args,
29 |         **kwargs: P.kwargs,
   |
info: rule `invalid-argument-type` is enabled by default

Found 1 diagnostic
```

---

_Renamed from "support ParamSpec" to "Support `ParamSpec`" by @dhruvmanila on 2025-12-05 15:06_

---

_Closed by @dhruvmanila on 2025-12-05 16:30_

---

_Comment by @dhruvmanila on 2025-12-05 16:37_

> An example from Starlette middleware that gives this error:

This is still unresolved and is being tracked as a separate issue (https://github.com/astral-sh/ty/issues/1635).

---
