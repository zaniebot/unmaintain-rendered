```yaml
number: 1838
title: "Resolve `ParamSpec` with the matching overload"
type: issue
state: closed
author: leddy231
labels:
  - bug
  - calls
  - overloads
assignees: []
created_at: 2025-12-10T12:25:56Z
updated_at: 2026-01-20T04:14:06Z
url: https://github.com/astral-sh/ty/issues/1838
synced_at: 2026-01-20T04:32:04Z
```

# Resolve `ParamSpec` with the matching overload

---

_@leddy231_

### Summary

Upgraded from 0.0.1a26 to 0.0.1a33 and noticed issues with asyncio.to_thread in combination with overloaded functions. Initially noticed with `open` but can be reduced to some simple example:

```python
import asyncio
from typing import overload


def normal(a: str) -> None:
    pass


@overload
def overloaded(a: str) -> None:
    pass


@overload
def overloaded(a: int) -> None:
    pass


def overloaded(a: str | int) -> None:
    pass


t1 = normal("test")
t2 = overloaded("test")
t3 = asyncio.to_thread(normal, "test")
t4 = asyncio.to_thread(overloaded, "test")
```
`ty check` gives:
```
error[invalid-argument-type]: Argument to function `to_thread` is incorrect
  --> test.py:26:36
   |
24 | t2 = overloaded("test")
25 | t3 = asyncio.to_thread(normal, "test")
26 | t4 = asyncio.to_thread(overloaded, "test")
   |                                    ^^^^^^ Expected `Overload[(a: str) -> Unknown, (a: int) -> Unknown]`, found `Literal["test"]`
   |
info: Function defined here
  --> stdlib/asyncio/threads.pyi:12:11
   |
10 | _R = TypeVar("_R")
11 |
12 | async def to_thread(func: Callable[_P, _R], /, *args: _P.args, **kwargs: _P.kwargs) -> _R:
   |           ^^^^^^^^^                            -------------- Parameter declared here
13 |     """Asynchronously run function *func* in a separate thread.
   |
info: rule `invalid-argument-type` is enabled by default
```


### Version

ty 0.0.1a33

---

_Comment by @dhruvmanila on 2025-12-10 12:45_

Thank you for reporting!

The `asyncio.to_thread` contains a ParamSpec in it's callable annotation and overloads are not resolved correctly when ParamSpec are involved.

At least in this case, I think we would need to recognize that `_P` is bound to an overloaded callable, perform overload call resolution to find the matching overload and update our specialization to reflect this. We should only raise an error if none of the overloads match.

---

_Label `overloads` added by @dhruvmanila on 2025-12-10 12:45_

---

_Label `bug` added by @dhruvmanila on 2025-12-10 12:45_

---

_Label `calls` added by @dhruvmanila on 2025-12-10 12:45_

---

_Renamed from "asyncio.to_thread fails on overloaded function" to "Resolve `ParamSpec` with the matching overload" by @dhruvmanila on 2025-12-10 12:46_

---

_Added to milestone `Stable` by @dhruvmanila on 2025-12-10 12:48_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-12-10 12:49_

---

_Comment by @leddy231 on 2026-01-18 23:32_

Any updates to share here? Would love to upgrade from 0.0.1a26 to the latest beta 😄 

---

_Closed by @dhruvmanila on 2026-01-20 04:14_

---
