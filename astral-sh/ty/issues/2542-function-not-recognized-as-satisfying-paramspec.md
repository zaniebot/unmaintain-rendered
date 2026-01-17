```yaml
number: 2542
title: Function not recognized as satisfying ParamSpec Protocol
type: issue
state: open
author: gaborbernat
labels: []
assignees: []
created_at: 2026-01-17T05:40:59Z
updated_at: 2026-01-17T05:41:25Z
url: https://github.com/astral-sh/ty/issues/2542
synced_at: 2026-01-17T06:04:04Z
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
