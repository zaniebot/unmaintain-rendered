---
number: 2351
title: How to correctly type generic await_if_awaitable
type: issue
state: open
author: ThomasPinna
labels:
  - question
assignees: []
created_at: 2026-01-05T17:30:21Z
updated_at: 2026-01-06T09:48:25Z
url: https://github.com/astral-sh/ty/issues/2351
synced_at: 2026-01-10T01:51:14Z
---

# How to correctly type generic await_if_awaitable

---

_Issue opened by @ThomasPinna on 2026-01-05 17:30_

### Question

In my code, I sometimes support both sync and async callbacks. For this, I typically use a util "await_if_awaitable". I would like to correctly type it with ty (but I have no clue how)

this is the source code

```python
async def await_if_awaitable[T](
    func: Callable[[], Awaitable[T]] | Callable[[], T]
) -> T:
    maybe_awaitable: Awaitable[T] | T = func()
    if isinstance(maybe_awaitable, Awaitable):
        return await maybe_awaitable
    return maybe_awaitable

```

In the `if` statement, `ty` correctly deduces that maybe_awaitable could be `Awaitable[T]` or `T & Awaitable[object]` (I first thought I encountered a bug). However, after trying many things it feels like an unsolvable issue in this correct mode (there is no way I can ensure whether something complies to `T`, right?)

[playground link](https://play.ty.dev/ee0151ae-cfec-4482-9d6d-340774078bb0)


### Version

4712503c6

---

_Label `question` added by @ThomasPinna on 2026-01-05 17:30_

---

_Comment by @AlexWaygood on 2026-01-05 17:47_

Thanks for the nice write-up!

Yeah... this is tricky. Ideally I feel like you'd be able to do something like this:

```py
import typing
from typing import Unpack, cast, Any, reveal_type
from collections.abc import Callable, Awaitable, Coroutine
from asyncio import iscoroutinefunction
from ty_extensions import Not


async def run_sync_or_async[T: Not[Awaitable[object]]](
    func: Callable[[], Awaitable[T]| T]
) -> T:
    maybe_awaitable: Awaitable[T] | T = func()
    if isinstance(maybe_awaitable, Awaitable):
        return await maybe_awaitable
    return maybe_awaitable
```

But:
1. That requires using `ty_extensions.Not`, which doesn't exist at runtime (you'd have to import inside an `if TYPE_CHECKING` block)
2. We currently complain (bafflingly) that "`Awaitable[T@run_sync_or_async]` is not awaitable" on the `return await maybe_awaitable` line there, which I think is a known bug (https://github.com/astral-sh/ty/issues/1880)

---

_Comment by @MichaReiser on 2026-01-05 17:50_

Another solution is to use `cast`:

```py
import typing
from typing import Unpack, cast, Any, reveal_type
from collections.abc import Callable, Awaitable, Coroutine
from asyncio import iscoroutinefunction


async def run_sync_or_async[T](
    func: Callable[[], Awaitable[T]| T]
) -> T:
    maybe_awaitable: Awaitable[T] | T = func()
    if isinstance(maybe_awaitable, Awaitable):
        return await cast(Awaitable[T], maybe_awaitable)
    return maybe_awaitable
```

Which seems fine to me in a helper function like this. 

---

_Comment by @kkpattern on 2026-01-06 07:19_

Can we use `TypeIs` here?

```python
from typing import Callable, Awaitable, TypeIs


def is_awaitable[T](v: Awaitable[T] | T) -> TypeIs[Awaitable[T]]:
    return isinstance(v, Awaitable)

async def await_if_awaitable[T](
    func: Callable[[], Awaitable[T]] | Callable[[], T]
) -> T:
    maybe_awaitable: Awaitable[T] | T = func()
    if is_awaitable(maybe_awaitable):
        return await maybe_awaitable
    else:
        return maybe_awaitable
```

---
