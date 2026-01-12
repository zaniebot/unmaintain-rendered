```yaml
number: 1361
title: "`from aiohttp import web` leads to `web` inferred as `object`"
type: issue
state: closed
author: sanga
labels:
  - imports
assignees: []
created_at: 2025-10-15T12:29:56Z
updated_at: 2025-10-15T20:22:22Z
url: https://github.com/astral-sh/ty/issues/1361
synced_at: 2026-01-12T15:54:25Z
```

# `from aiohttp import web` leads to `web` inferred as `object`

---

_@sanga_

### Summary

See the repo here for a reproducer https://github.com/sanga/ty_import_bug

But essentially: `aiohttp.web.*` isn't resolved properly.

With the `aiohttp` example server code:

```python
# the snippet below is copy pasted from here:
# https://docs.aiohttp.org/en/stable/#server-example
from aiohttp import web

async def handle(request):
    name = request.match_info.get("name", "Anonymous")
    text = "Hello, " + name
    return web.Response(text=text)

app = web.Application()
app.add_routes([web.get("/", handle), web.get("/{name}", handle)])

if __name__ == "__main__":
    web.run_app(app)
```

`ty` version 0.0.1-alpha.22 reports:

```
error[unresolved-attribute]: Type `object` has no attribute `Response`
  --> import_bug.py:16:12
   |
14 |     name = request.match_info.get("name", "Anonymous")
15 |     text = "Hello, " + name
16 |     return web.Response(text=text)
   |            ^^^^^^^^^^^^
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Type `object` has no attribute `Application`
  --> import_bug.py:19:7
   |
19 | app = web.Application()
   |       ^^^^^^^^^^^^^^^
20 | app.add_routes([web.get("/", handle), web.get("/{name}", handle)])
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Type `object` has no attribute `get`
  --> import_bug.py:20:17
   |
19 | app = web.Application()
20 | app.add_routes([web.get("/", handle), web.get("/{name}", handle)])
   |                 ^^^^^^^
21 |
22 | if __name__ == "__main__":
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Type `object` has no attribute `get`
  --> import_bug.py:20:39
   |
19 | app = web.Application()
20 | app.add_routes([web.get("/", handle), web.get("/{name}", handle)])
   |                                       ^^^^^^^
21 |
22 | if __name__ == "__main__":
   |
info: rule `unresolved-attribute` is enabled by default

error[unresolved-attribute]: Type `object` has no attribute `run_app`
  --> import_bug.py:23:5
   |
22 | if __name__ == "__main__":
23 |     web.run_app(app)
   |     ^^^^^^^^^^^
   |
info: rule `unresolved-attribute` is enabled by default

Found 5 diagnostics
```

The `web` module in `aiohttp` is basically a bunch of reexports ala:

```python
from .abc import AbstractAccessLogger
from .helpers import AppKey as AppKey
from .log import access_logger
from .typedefs import PathLike
from .web_app import Application as Application, CleanupError as CleanupError
from .web_exceptions import (
    HTTPAccepted as HTTPAccepted,
    HTTPBadGateway as HTTPBadGateway,

```

For the record:

- `basedpyright` resolves these just fine
- e.g. `from aiohttp.web_app import Application` works fine with `ty`

Apologies if this is already captured by a previous bug. I didn't spot anything that looked like it  matched either by searching for `aiohttp` or  glancing through the bugs tagged with "import"

### Version

0.0.1-alpha.22

---

_Comment by @sanga on 2025-10-15 12:34_

Oh this may be https://github.com/astral-sh/ty/issues/133 I guess. Apologies if so

---

_Renamed from "imports not resolved for `aiohttp.web`" to "`from aiohttp import web` leads to `web` inferred as `object`" by @AlexWaygood on 2025-10-15 12:39_

---

_Label `imports` added by @sharkdp on 2025-10-15 13:30_

---

_Comment by @sharkdp on 2025-10-15 13:30_

FYI @Gankra 

---

_Comment by @AlexWaygood on 2025-10-15 20:22_

Thanks for the report! This is a duplicate of #1053. We can see that `aiohttp/__init__.py` has a module-level `__getattr__` function [here](https://github.com/aio-libs/aiohttp/blob/149a8105e7de73888df11ab1689315f9482aaeae/aiohttp/__init__.py#L244-L258) annotated as returning `object`, and we currently give that precedence over the package's submodules when resolving the import

---

_Closed by @AlexWaygood on 2025-10-15 20:22_

---
