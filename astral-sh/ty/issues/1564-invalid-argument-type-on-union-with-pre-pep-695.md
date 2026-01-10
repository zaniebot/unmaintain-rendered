---
number: 1564
title: "`invalid-argument-type` on union with pre PEP 695 type parameter"
type: issue
state: closed
author: edgarrmondragon
labels:
  - type aliases
assignees: []
created_at: 2025-11-14T19:32:49Z
updated_at: 2025-12-27T19:28:53Z
url: https://github.com/astral-sh/ty/issues/1564
synced_at: 2026-01-10T01:51:14Z
---

# `invalid-argument-type` on union with pre PEP 695 type parameter

---

_Issue opened by @edgarrmondragon on 2025-11-14 19:32_

### Summary

The following is accepted by mypy and was also working in previous versions of ty:

```python
from typing import Any, Generic, TypeAlias, TypeVar, Union

_JsonValue: TypeAlias = str | int | float | bool | list | dict | None

T = TypeVar("T", bound=_JsonValue)


class JSONTypeHelper(Generic[T]):
    def __init__(self) -> None: ...


class StringType(JSONTypeHelper[str]):
    def __init__(self) -> None: ...


W = TypeVar("W", bound=JSONTypeHelper)


class ArrayType(JSONTypeHelper[list], Generic[W]):
    def __init__(self, wrapped_type: W | type[W], **kwargs: Any) -> None: ...


array = ArrayType(StringType)
# Argument to bound method `__init__` is incorrect: Expected `ArrayType[W@ArrayType]`, found `ArrayType[<class 'StringType'>]` (invalid-argument-type) [Ln 23, Col 9]
```

https://play.ty.dev/bddec81e-101a-4c8b-818b-5012af6dec2e

Curiously, using PEP 695 seems to make ty happy, though unfortunately I need to support Python 3.10+:

```python
from typing import Any, TypeAlias, Union

_JsonValue: TypeAlias = str | int | float | bool | list | dict | None


class JSONTypeHelper[T: _JsonValue]():
    def __init__(self) -> None: ...


class StringType(JSONTypeHelper[str]):
    def __init__(self) -> None: ...


class ArrayType[W: JSONTypeHelper](JSONTypeHelper[list]):
    def __init__(self, wrapped_type: W | type[W], **kwargs: Any) -> None: ...


array = ArrayType(StringType)
```

https://play.ty.dev/5369dde9-a05c-4788-86c2-8d7291f6d85f

### Version

ty 0.0.1-alpha.26 (b225fd8b4 2025-11-10)

---

_Comment by @carljm on 2025-11-14 20:40_

Thanks for the report! This is a pre-existing limitation of our generic solver (we don't understand `type[W]` where `W` is a type variable), which we hope to fix shortly. It's revealed in a recent ty release in this example because we recently improved our understanding of type aliases (such as `_JsonValue`).

I'm not entirely sure why the behavior is different when you switch to PEP 695; I suspect it has something to do with how we synthesize the type of `self` in each case? That part needs more exploration to understand.

---

_Added to milestone `Beta` by @carljm on 2025-11-14 20:41_

---

_Removed from milestone `Beta` by @carljm on 2025-11-14 20:42_

---

_Added to milestone `Stable` by @carljm on 2025-11-14 20:42_

---

_Comment by @edgarrmondragon on 2025-11-14 21:07_

Interesting, come to think of it I've also run into https://github.com/astral-sh/ty/issues/783 and I see how both of these have the same underlying cause.

---

_Comment by @yoonthegoon on 2025-11-18 17:10_

+1

Where `Bet().update_or_create() -> Bet` and `Event().update_or_create() -> Event`

This works just fine:

```py
    def process_item(self, item: Bet | Event, spider: Spider) -> Bet | Event:
        return item.update_or_create(self.session)
```

But the following gives me these issues:


```py
    def process_item[T: Bet | Event](self, item: T, spider: Spider) -> T:
        return item.update_or_create(self.session)
```

```
Argument to bound method `update_or_create` is incorrect: Expected `Bet`, found `T@process_item`ty[invalid-argument-type](https://ty.dev/rules#invalid-argument-type)
---
Argument to bound method `update_or_create` is incorrect: Expected `Event`, found `T@process_item`ty[invalid-argument-type](https://ty.dev/rules#invalid-argument-type)
---
Return type does not match returned value: expected `T@process_item`, found `Bet | Event`ty[invalid-return-type](https://ty.dev/rules#invalid-return-type)
pipelines.py(25, 72): Expected `T@process_item` because of return type
```

---

ty 0.0.1-alpha.26
Python 3.14.0


---

_Comment by @dmytro-GL on 2025-11-18 18:21_

Here is another one (though I'm not 100% sure this is the exact same issue):

```
from typing import Callable, Protocol, ParamSpec

P = ParamSpec("P")
T = Callable[[], None]

class Foo:
    def __init__(self, func: T) -> None:
        self.func = func
    def __call__(self) -> None:
        self.func()

class FooFactory1(Protocol[P]):
    def __call__(self, func: T, /, *args: P.args, **kwargs: P.kwargs) -> T: ...

class FooFactory2(Protocol[P]):
    def __call__(self, func: T) -> T: ...

def test1(func: FooFactory1[P]):
    func(lambda: print("1"))()

def test2(func: FooFactory2[P]):
    func(lambda: print("2"))()

def test3(func: FooFactory1):
    func(lambda: print("3"))()

def test4(func: FooFactory2):
    func(lambda: print("4"))()

test1(Foo)
test2(Foo)
test3(Foo)
test4(Foo)
```

0.0.1a25 complains about cases 3 and 4:

```
$ uvx --with-requirements=requirements.txt ty@0.0.1a25 check test.min.py
error[invalid-argument-type]: Argument to function `test3` is incorrect
  --> test.min.py:32:7
   |
30 | test1(Foo)
31 | test2(Foo)
32 | test3(Foo)
   |       ^^^ Expected `FooFactory1`, found `<class 'Foo'>`
33 | test4(Foo)
   |
info: Function defined here
  --> test.min.py:24:5
   |
22 |     func(lambda: print("2"))()
23 |
24 | def test3(func: FooFactory1):
   |     ^^^^^ ----------------- Parameter declared here
25 |     func(lambda: print("3"))()
   |
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to function `test4` is incorrect
  --> test.min.py:33:7
   |
31 | test2(Foo)
32 | test3(Foo)
33 | test4(Foo)
   |       ^^^ Expected `FooFactory2`, found `<class 'Foo'>`
   |
info: Function defined here
  --> test.min.py:27:5
   |
25 |     func(lambda: print("3"))()
26 |
27 | def test4(func: FooFactory2):
   |     ^^^^^ ----------------- Parameter declared here
28 |     func(lambda: print("4"))()
   |
info: rule `invalid-argument-type` is enabled by default

Found 2 diagnostics
```

0.0.1a26 complains about cases 1 and 3:

```
$ uvx --with-requirements=requirements.txt ty@0.0.1a26 check test.min.py
error[invalid-argument-type]: Argument to function `test1` is incorrect
  --> test.min.py:30:7
   |
28 |     func(lambda: print("4"))()
29 |
30 | test1(Foo)
   |       ^^^ Expected `FooFactory1[Unknown]`, found `<class 'Foo'>`
31 | test2(Foo)
32 | test3(Foo)
   |
info: Function defined here
  --> test.min.py:18:5
   |
16 |     def __call__(self, func: T) -> T: ...
17 |
18 | def test1(func: FooFactory1[P]):
   |     ^^^^^ -------------------- Parameter declared here
19 |     func(lambda: print("1"))()
   |
info: rule `invalid-argument-type` is enabled by default

error[invalid-argument-type]: Argument to function `test3` is incorrect
  --> test.min.py:32:7
   |
30 | test1(Foo)
31 | test2(Foo)
32 | test3(Foo)
   |       ^^^ Expected `FooFactory1[Unknown]`, found `<class 'Foo'>`
33 | test4(Foo)
   |
info: Function defined here
  --> test.min.py:24:5
   |
22 |     func(lambda: print("2"))()
23 |
24 | def test3(func: FooFactory1):
   |     ^^^^^ ----------------- Parameter declared here
25 |     func(lambda: print("3"))()
   |
info: rule `invalid-argument-type` is enabled by default

Found 2 diagnostics
```

pyright doesn't like the first case, but for a different reason (?)

```
$ uvx --with-requirements requirements.txt pyright@1.1.407 test.min.py
/tmp/t/test.min.py
  /tmp/t/test.min.py:19:5 - error: Arguments for ParamSpec "P@test1" are missing (reportCallIssue)
1 error, 0 warnings, 0 informations
```

The example above is an attempt to investigate the `invalid-argument-type` issue reported by ty@0.0.1a26 in the following code (ty@0.0.1a25 and pyright@1.1.407 don't report any issues):

```
# /// script
# dependencies = [
#   "uvicorn   == 0.38.0",
#   "fastapi   == 0.121.2",
#   "starlette == 0.49.3",
# ]
# ///

import uvicorn

from fastapi import FastAPI

from starlette.middleware.base import BaseHTTPMiddleware, RequestResponseEndpoint
from starlette.requests import Request
from starlette.responses import Response

class HelloMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next: RequestResponseEndpoint) -> Response:
        print("hello")
        return await call_next(request)

app = FastAPI()
app.add_middleware(HelloMiddleware)

if __name__ == "__main__":
    uvicorn.run("test:app", host="127.0.0.1", port=8000)
```

---

_Label `type aliases` added by @AlexWaygood on 2025-12-19 12:14_

---

_Comment by @edgarrmondragon on 2025-12-24 17:47_

This seems to have been fixed in 0.0.2, at least for the case I described at the top.

---

_Comment by @carljm on 2025-12-27 19:28_

I think the case shown by @dmytro-GL is something different, it looks more like #1970 and #2227.

I think the original report here was fixed in 0.0.2 with the addition of support for `type[T]`.

Thanks for following up, @edgarrmondragon !

---

_Closed by @carljm on 2025-12-27 19:28_

---
