```yaml
number: 2344
title: "Incorrect type inference for asynccontextmanager's yielding Self"
type: issue
state: closed
author: iyad-f
labels:
  - generics
  - callables
assignees: []
created_at: 2026-01-05T15:05:37Z
updated_at: 2026-01-15T15:47:01Z
url: https://github.com/astral-sh/ty/issues/2344
synced_at: 2026-01-15T16:50:04Z
```

# Incorrect type inference for asynccontextmanager's yielding Self

---

_@iyad-f_

### Summary

async context managers which yield Self are being inferred as yielding None

mcve
```py
from collections.abc import AsyncIterator
from contextlib import asynccontextmanager
from typing import Self


class Foo:
    @classmethod
    @asynccontextmanager
    async def new(cls) -> AsyncIterator[Self]:
        yield cls()


async def test() -> None:
    async with Foo.new() as foo:  # type inferred by ty = None, expected type = Foo
        ...
```
It inferes correctly if the type yielded by the asynccontextmanager is not self, so for example if the new asyncocntextmanger  would yield int or str it inferes that correctly 

### Version

0.0.8

---

_Added to milestone `Stable` by @carljm on 2026-01-05 23:05_

---

_Comment by @carljm on 2026-01-06 00:17_

Thanks for the report! This does look like a bug. It seems to be specific to `Self`, but not to classmethods, or to `@asynccontextmanager` or `AsyncIterator`. Here's an even more simplified repro:

```py
from typing import Self, TypeVar, Callable, Generic

_T = TypeVar("_T")

class Box(Generic[_T]):
    def __init__(self, value: _T) -> None:
        self.value = value

def boxify(func: Callable[..., _T]) -> Callable[..., Box[_T]]:
    def wrapper(*args, **kwargs) -> Box[_T]:
        return Box(func(*args, **kwargs))
    return wrapper

class Foo:
    def no_decorator(self) -> Self:
        return self

    @boxify
    def with_decorator(self) -> Self:
        return self

obj = Foo()
reveal_type(obj.no_decorator())    # Foo
reveal_type(obj.with_decorator())  # ty: Box[Unknown], pyright: Box[Foo]
```

---

_Label `generics` added by @carljm on 2026-01-06 00:17_

---

_Label `callables` added by @carljm on 2026-01-06 00:17_

---

_Comment by @iyad-f on 2026-01-06 12:41_

yes it seems the issue is specific to Self and decorators in general, mb for not checking this properly 



---

_Comment by @carljm on 2026-01-06 16:58_

Not at all, your report was excellent!

---

_Comment by @pwuertz on 2026-01-09 16:42_

Same as https://github.com/astral-sh/ty/issues/2030 ?

---

_Comment by @iyad-f on 2026-01-15 10:57_

i guess [#22407](https://github.com/astral-sh/ruff/pull/22407) fixes this, so can close this issue

---

_Comment by @carljm on 2026-01-15 15:42_

Interesting, it looks like https://github.com/astral-sh/ruff/pull/22407 fixed the OP example here, but did not fix my version. So it seems like there are multiple issues here. I will close this issue and split my snippet into a separate issue.

---

_Closed by @AlexWaygood on 2026-01-15 15:46_

---

_Comment by @iyad-f on 2026-01-15 15:47_

Looks like it was then probably associated with context manager and class method i guess

---
