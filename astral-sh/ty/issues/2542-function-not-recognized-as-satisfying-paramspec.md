```yaml
number: 2542
title: Function not recognized as satisfying ParamSpec Protocol
type: issue
state: closed
author: gaborbernat
labels: []
assignees: []
created_at: 2026-01-17T05:40:59Z
updated_at: 2026-01-17T15:11:38Z
url: https://github.com/astral-sh/ty/issues/2542
synced_at: 2026-01-17T16:15:01Z
```

# Function not recognized as satisfying ParamSpec Protocol

---

_@gaborbernat_

A function with matching signature is not recognized as satisfying a Protocol that uses ParamSpec.

## Minimal reproduction

```python
from typing import Protocol, ParamSpec, Callable, Awaitable, Any

P = ParamSpec("P")

class ASGIApp(Protocol):
    async def __call__(self, scope: Any, receive: Any, send: Any) -> None: ...

class MiddlewareFactory(Protocol[P]):
    def __call__(self, app: ASGIApp, /, *args: P.args, **kwargs: P.kwargs) -> ASGIApp: ...

def my_middleware(app: ASGIApp) -> ASGIApp:
    async def wrapper(scope: Any, receive: Any, send: Any) -> None:
        await app(scope, receive, send)
    return wrapper

def add_middleware(factory: MiddlewareFactory[...]) -> None:
    pass

add_middleware(my_middleware)
```

## Error

```
error[invalid-argument-type]: Argument to function `add_middleware` is incorrect
  --> issue.py:19:16
   |
17 |     pass
18 |
19 | add_middleware(my_middleware)
   |                ^^^^^^^^^^^^^ Expected `MiddlewareFactory[(...)]`, found `def my_middleware(app: ASGIApp) -> ASGIApp`
```

## Expected behavior

`my_middleware` has signature `(app: ASGIApp) -> ASGIApp` which should satisfy `MiddlewareFactory[...]` since it takes
`app` as first argument and returns `ASGIApp`, with no additional args/kwargs (matching empty ParamSpec).


### Version

0.0.12

---

_Comment by @AlexWaygood on 2026-01-17 11:38_

Thanks! Is this the same as #1635?

---

_Comment by @gaborbernat on 2026-01-17 15:07_

- Indeed, just here my repro doesn't have any dependency 😅

---

_Comment by @AlexWaygood on 2026-01-17 15:11_

Thanks! We know what the cause is here, though (https://github.com/astral-sh/ty/issues/1714) -- the reason #1635 has not been closed as a duplicate is because people are more likely to find it on the issue tracker than #1714, which has a bit of an abstract title. And #1635 has a link to https://github.com/astral-sh/ty/issues/157#issuecomment-3554317390 in the issue thread, which is a repro without a dependency

---

_Closed by @AlexWaygood on 2026-01-17 15:11_

---
