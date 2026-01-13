```yaml
number: 2456
title: Django async_to_sync returns None
type: issue
state: closed
author: andre-dasilva
labels: []
assignees: []
created_at: 2026-01-12T08:29:25Z
updated_at: 2026-01-13T01:17:41Z
url: https://github.com/astral-sh/ty/issues/2456
synced_at: 2026-01-13T01:22:08Z
```

# Django async_to_sync returns None

---

_@andre-dasilva_

### Summary

First of all thank you for making ty. i would love to use it as a replacement or addition for mypy in the future 😄.

I tried to use ty in my django app. 

Since django is not yet fully async compatiable i am using the asgiref [async_to_sync](https://docs.djangoproject.com/en/6.0/topics/async/#asgiref.sync.async_to_sync) helper function, for parts of my application.

ty says, that every function with async_to_sync could return `None`:

<img width="511" height="210" alt="Image" src="https://github.com/user-attachments/assets/911d3cbb-4c53-4189-882a-c0e913e35caa" />

if i look at the implementation:

<img width="721" height="545" alt="Image" src="https://github.com/user-attachments/assets/c3825cf5-a308-4c59-ba7d-cd7a47f42dfb" />

i dont see that `None` is returned anywhere? is this a bug in uv? or does this come from asgiref?


### Version

0.0.11

---

_Comment by @carljm on 2026-01-13 01:17_

Thanks for the report! I'm pretty sure this has the same root cause as https://github.com/astral-sh/ty/issues/2131 -- going to close it as duplicate, but we should double-check this case once that is fixed.

Transcribed code (by Claude) for future reference:

```py
import asyncio
from typing import Any, Awaitable, Callable, Coroutine, Optional, TypeVar, Union
from typing_extensions import ParamSpec

_P = ParamSpec("_P")
_R = TypeVar("_R")


# Simplified async_to_sync signature based on asgiref
def async_to_sync(
    awaitable: Optional[
        Union[
            Callable[_P, Coroutine[Any, Any, _R]],
            Callable[_P, Awaitable[_R]],
        ]
    ] = None,
    *,
    force_new_loop: bool = False,
) -> Union[
    Callable[
        [Union[Callable[_P, Coroutine[Any, Any, _R]], Callable[_P, Awaitable[_R]]]],
        Callable[_P, _R],
    ],
    Callable[_P, _R],
]:
    # Stub implementation
    raise NotImplementedError()


async def example() -> int:
    await asyncio.sleep(delay=1)
    return 42


def call_example() -> int:
    result = async_to_sync(awaitable=example)
    reveal_type(result)
    result = result()
    reveal_type(result)
    return result + 12

```

---

_Closed by @carljm on 2026-01-13 01:17_

---
