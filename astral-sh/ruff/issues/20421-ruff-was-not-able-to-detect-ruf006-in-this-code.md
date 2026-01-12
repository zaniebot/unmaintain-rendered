```yaml
number: 20421
title: "Ruff was not able to detect `RUF006` in this code."
type: issue
state: closed
author: chirizxc
labels:
  - question
assignees: []
created_at: 2025-09-15T17:32:14Z
updated_at: 2025-09-16T08:58:40Z
url: https://github.com/astral-sh/ruff/issues/20421
synced_at: 2026-01-12T15:54:57Z
```

# Ruff was not able to detect `RUF006` in this code.

---

_@chirizxc_

### Summary

[Playground](https://play.ruff.rs/0d674c65-8f9c-499b-9a34-b8735f46469e) | [asyncio-dangling-task (RUF006)](https://docs.astral.sh/ruff/rules/asyncio-dangling-task/#asyncio-dangling-task-ruf006)

```python
import asyncio
from collections.abc import AsyncIterator

import pytest
import uvicorn
from example_app import main


@pytest.fixture(scope="session")
async def backend() -> AsyncIterator[None]:
    app = main()
    task = asyncio.create_task(uvicorn.Server(uvicorn.Config(app=app, port=8080)).serve())
    yield
    task.cancel()
```

### Version

ruff 0.13.0 (a1fdd66f1 2025-09-10)

---

_Renamed from "Ruff was not able to detect RUF006 in this code." to "Ruff was not able to detect `RUF006` in this code." by @chirizxc on 2025-09-15 17:33_

---

_Comment by @chirizxc on 2025-09-15 17:52_

I think it's because of this https://github.com/astral-sh/ruff/blob/main/crates%2Fruff_linter%2Fsrc%2Frules%2Fruff%2Frules%2Fasyncio_dangling_task.rs#L124



---

_Comment by @ntBre on 2025-09-15 20:19_

I'm not that familiar with Python async, but I would kind of expect the `task` variable to maintain a reference even after the yield. Is that not how it works?

---

_Label `question` added by @ntBre on 2025-09-15 20:19_

---

_Comment by @Eclips4 on 2025-09-16 08:03_

> I'm not that familiar with Python async, but I would kind of expect the `task` variable to maintain a reference even after the yield.

You're right. The `task` name here is a strong reference to the object returned by `asyncio.create_task(...)`, so we hold a strong reference for the duration of the function's execution. Now imagine there is no `task.cancel()`. In that case, when the function exits the strong reference disappears and we're left only with a weakref -> a potential footgun.

---

_Comment by @MichaReiser on 2025-09-16 08:26_

>You're right. The task name here is a strong reference to the object returned by asyncio.create_task(...), so we hold a strong reference for the duration of the function's execution. Now imagine there is no task.cancel(). In that case, when the function exits the strong reference disappears and we're left only with a weakref -> a potential footgun.

RUF006 has you covered. It triggers when you remove the `task.cancel` call

---

_Closed by @MichaReiser on 2025-09-16 08:26_

---

_Comment by @chirizxc on 2025-09-16 08:28_

> > You're right. The task name here is a strong reference to the object returned by asyncio.create_task(...), so we hold a strong reference for the duration of the function's execution. Now imagine there is no task.cancel(). In that case, when the function exits the strong reference disappears and we're left only with a weakref -> a potential footgun.
> 
> RUF006 has you covered. It triggers when you remove the `task.cancel` call

what about this code?

https://play.ruff.rs/0f5e4100-41b5-480a-aaec-eb80d1fab532

---

_Comment by @chirizxc on 2025-09-16 08:30_

```python
import asyncio


async def _():
    task = asyncio.create_task(asyncio.sleep(2))
    print(task)
    yield
```
Ruff doesn't detect `RUF006` because of the print, although it should.

---

_Comment by @MichaReiser on 2025-09-16 08:37_

The `task` variable should stay alive until the end of the `_` function because it is captured as part of the generator object. So this should work just fine as far as I can tell.

---

_Comment by @lubaskinc0de on 2025-09-16 08:45_

> The `task` variable should stay alive until the end of the `_` function because it is captured as part of the generator object. So this should work just fine as far as I can tell.

if you remove print then ruff detects an error with dangling tasks, why does print disable this error if it doesn't affect anything and the task will still lose the strong reference when the function exits, i don't think it should work like this

---

_Comment by @Eclips4 on 2025-09-16 08:45_

> The `task` variable should stay alive until the end of the `_` function because it is captured as part of the generator object. So this should work just fine as far as I can tell.

The problem is that the task might not complete during the execution of `_`, so we could end up with a dangling task.

---

_Comment by @MichaReiser on 2025-09-16 08:58_

I see. Yes, Ruff doesn't check that you do something "meaningful" with the task object. It only checks if it is used in some way. 

Checking that a task object is always cancelled or returned (or stored somewhere else) would require a full control flow analysis which Ruff currently doesn't have

This is the same as https://github.com/astral-sh/ruff/issues/16811

---
