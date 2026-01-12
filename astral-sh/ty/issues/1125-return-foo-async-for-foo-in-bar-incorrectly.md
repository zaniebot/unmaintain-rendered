```yaml
number: 1125
title: return (foo async for foo in bar) incorrectly marked as GeneratorType instead of AsyncGenerator type
type: issue
state: closed
author: hinthornw
labels: []
assignees: []
created_at: 2025-09-03T23:36:14Z
updated_at: 2025-09-03T23:40:30Z
url: https://github.com/astral-sh/ty/issues/1125
synced_at: 2026-01-12T15:54:24Z
```

# return (foo async for foo in bar) incorrectly marked as GeneratorType instead of AsyncGenerator type

---

_@hinthornw_

### Summary

(or could be some different root cause. in any case it was unexpected)

ty flags both of the following as incorrect in alpha.20 regardless of whether it's typed as AsyncIterator or AsyncGenerator, while it passes in previous versions.

```python
# foo.py
import asyncio
from collections.abc import AsyncIterator, AsyncGenerator


async def consume():
    for i in range(10):
        yield i


async def fn1() -> AsyncGenerator:
    return (bar async for bar in consume())


async def fn2() -> AsyncIterator:
    return (bar async for bar in consume())


async def both():
    async for i in await fn1():
        print(f"foo: {i}")
    async for i in await fn2():
        print(f"foo: {i}")


asyncio.run(both())

```


```shell
➜  code git:(main) ✗ uvx --from "ty==0.0.1-alpha.19" ty check foo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                  All checks passed!
```
but:

```shell
➜  code git:(main) ✗ uvx --from "ty==0.0.1-alpha.20" ty check foo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                  error[invalid-return-type]: Return type does not match returned value
  --> foo.py:10:20
   |
10 | async def foo() -> AsyncGenerator:
   |                    -------------- Expected `AsyncGenerator[Unknown, None]` because of return type
11 |     return (bar async for bar in consume())
   |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ expected `AsyncGenerator[Unknown, None]`, found `GeneratorType[@Todo, @Todo, @Todo]`
   |
info: rule `invalid-return-type` is enabled by default

error[invalid-return-type]: Return type does not match returned value
  --> foo.py:14:20
   |
14 | async def foo() -> AsyncIterator:
   |                    ------------- Expected `AsyncIterator[Unknown]` because of return type
15 |     return (bar async for bar in consume())
   |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ expected `AsyncIterator[Unknown]`, found `GeneratorType[@Todo, @Todo, @Todo]`
   |
info: rule `invalid-return-type` is enabled by default

Found 2 diagnostics
```


### Version

ty==0.0.1-alpha.20

---

_Closed by @hinthornw on 2025-09-03 23:40_

---

_Comment by @hinthornw on 2025-09-03 23:40_

lol premature apologies for the noise


---
