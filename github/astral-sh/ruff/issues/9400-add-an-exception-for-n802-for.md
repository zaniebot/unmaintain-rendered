---
number: 9400
title: "Add an exception for `N802` for `BaseHTTPRequestHandler` subclasses' methods"
type: issue
state: open
author: ZeeD
labels: []
assignees: []
created_at: 2024-01-05T10:02:40Z
updated_at: 2024-01-05T15:22:03Z
url: https://github.com/astral-sh/ruff/issues/9400
synced_at: 2026-01-07T13:12:15-06:00
---

# Add an exception for `N802` for `BaseHTTPRequestHandler` subclasses' methods

---

_Issue opened by @ZeeD on 2024-01-05 10:02_

According to [the doc](https://docs.python.org/3/library/http.server.html#http.server.BaseHTTPRequestHandler) you should use `BaseHTTPRequestHandler` subclassing it and implementing various `do_SPAM` methods, one for each request method.
But if in this basic snippet
```python
from http.server import BaseHTTPRequestHandler
class MyRequestHandler(BaseHTTPRequestHandler):
    def do_GET(self) -> None: ...
```
`ruff` raises the error
```
N802 Function name `do_GET` should be lowercase
```
I would suggest to add an exception for the various `do_XXX` methods of the subclasses of `BaseHTTPRequestHandler`

---

_Comment by @charliermarsh on 2024-01-05 12:27_

One thing you can do to help Ruff here is add the `@typing.override` or `@typing_extensions.override` decorator to your method to indicate to tools that it's overriding an existing method.

---

_Comment by @ZeeD on 2024-01-05 15:22_

Hi, thanks for the reply.
In this case I don't think it will work, as `BaseHTTPRequestHandler` does not implement such methods.
Instead they are part of an (informal) protocol, and retrieved dynamically via `getattr` - as you can see from https://github.com/python/cpython/blob/e56c53334f1adf5b4ea940036c76d18e80fe809d/Lib/http/server.py#L417-L424

BTW: if I add `@typing.override` I may trick `ruff`, but now `mypy` raises an error (because there is no `do_SPAM` method to override in `BaseHTTPRequestHandler`)

---

_Referenced in [astral-sh/ruff#18907](../../astral-sh/ruff/pulls/18907.md) on 2025-06-24 02:02_

---
