```yaml
number: 13637
title: "[flake8-async] `ASYNC100` false positive in async generator expressions"
type: issue
state: closed
author: autinerd
labels:
  - bug
  - good first issue
  - rule
assignees: []
created_at: 2024-10-05T06:27:30Z
updated_at: 2024-10-07T12:25:55Z
url: https://github.com/astral-sh/ruff/issues/13637
synced_at: 2026-01-12T15:54:53Z
```

# [flake8-async] `ASYNC100` false positive in async generator expressions

---

_@autinerd_

Hi!

`ASYNC100` doesn't trigger with this code:

```python
import asyncio

async def long_running_range():
    for i in range(10):
        await asyncio.sleep(2)
        yield i

async def main():
    async with asyncio.timeout(7):
        async for i in long_running_range():
            print(i)

asyncio.run(main())
```

but it does with this code:

```python
import asyncio

async def long_running_range():
    for i in range(10):
        await asyncio.sleep(2)
        yield i

async def main():
    async with asyncio.timeout(7):
        print({i async for i in long_running_range()})

asyncio.run(main())
```

Thanks!

---

_Label `bug` added by @zanieb on 2024-10-05 15:14_

---

_Label `rule` added by @zanieb on 2024-10-05 15:14_

---

_Label `good first issue` added by @zanieb on 2024-10-05 15:14_

---

_Closed by @zanieb on 2024-10-07 12:25_

---
