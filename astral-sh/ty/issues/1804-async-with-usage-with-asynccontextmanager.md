```yaml
number: 1804
title: "`async with` usage with `asynccontextmanager`-decorated function results in `Unknown`"
type: issue
state: closed
author: sharkdp
labels:
  - generics
assignees: []
created_at: 2025-12-08T11:58:35Z
updated_at: 2025-12-09T21:49:02Z
url: https://github.com/astral-sh/ty/issues/1804
synced_at: 2026-01-12T15:54:25Z
```

# `async with` usage with `asynccontextmanager`-decorated function results in `Unknown`

---

_@sharkdp_

Consider the following example, where the usage of a `asynccontextmanager`-decorated function in an `async with` statement results in an `Unknown` type:
```py
from contextlib import asynccontextmanager
from typing import AsyncGenerator

class Session: ...

@asynccontextmanager
async def connect() -> AsyncGenerator[Session]:
    yield Session()


async def main():
    async with connect() as session:
        # TODO: should be `Session`
        reveal_type(session)  # revealed: Unknown
```
https://play.ty.dev/8f142494-1d12-48fd-8ea5-005b1bdf62dd


The problem is that we currently infer `() -> _AsyncGeneratorContextManager[Unknown, None]` for `connect`. The signature of `asynccontextmanager` is:

```py
def asynccontextmanager(func: Callable[_P, AsyncIterator[_T_co]]) -> Callable[_P, _AsyncGeneratorContextManager[_T_co]]
```

so this requires generics-solver support for `Callable` and generic protocols.


Note that we have a test for this here: https://github.com/astral-sh/ruff/blob/a364195335eb920ff429c43832c4ee464051ea9a/crates/ty_python_semantic/resources/mdtest/with/async.md?plain=1#L203-L222

---

_Label `generics` added by @sharkdp on 2025-12-08 11:58_

---

_Added to milestone `Beta` by @sharkdp on 2025-12-08 11:58_

---

_Assigned to @AlexWaygood by @carljm on 2025-12-08 17:11_

---

_Assigned to @dcreager by @carljm on 2025-12-08 17:11_

---

_Closed by @sharkdp on 2025-12-09 21:49_

---
