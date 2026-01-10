---
number: 2299
title: asyncio.Task parameters are incorrect (because outdated typeshed)
type: issue
state: closed
author: kevinderuijter
labels: []
assignees: []
created_at: 2026-01-02T00:34:09Z
updated_at: 2026-01-02T02:27:29Z
url: https://github.com/astral-sh/ty/issues/2299
synced_at: 2026-01-10T01:51:14Z
---

# asyncio.Task parameters are incorrect (because outdated typeshed)

---

_Issue opened by @kevinderuijter on 2026-01-02 00:34_

### Summary

Ty says: Argument `eager_start` does not match any known parameter of function `create_task`
However, the argument does exist but ty is looking at python 3.11 signature.

This bug can be reproduced in the UV playground.
The share button in [https://play.ty.dev](https://play.ty.dev) doesn't work for me so the code is below.

```py
import asyncio

async def connect():
    ...

async def main():
    # incorrect ty error about argument eager_start here.
    asyncio.create_task(connect(), eager_start=True)
```

See the tasks.pyi as the cause.

```py
if sys.version_info >= (3, 11):
    def create_task(coro: _CoroutineLike[_T], *, name: str | None = None, context: Context | None = None) -> Task[_T]:
        """Schedule the execution of a coroutine object in a spawn task.

        Return a Task object.
        """

else:
    def create_task(coro: _CoroutineLike[_T], *, name: str | None = None) -> Task[_T]:
        """Schedule the execution of a coroutine object in a spawn task.

        Return a Task object.
        """
```

eager_start has been available as a keyword argument since python 3.14: https://docs.python.org/3/library/asyncio-task.html#creating-tasks

### Version

ty 0.0.8

---

_Renamed from "Task parameters are incorrect (because python version is wrongly assumed)" to "asyncio.Task parameters are incorrect (because outdated typeshed)" by @kevinderuijter on 2026-01-02 00:40_

---

_Comment by @kevinderuijter on 2026-01-02 02:27_

Issue has to do with typeshed, not ty.
https://github.com/python/typeshed/issues/15210

---

_Closed by @kevinderuijter on 2026-01-02 02:27_

---
