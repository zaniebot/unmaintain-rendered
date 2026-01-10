```yaml
number: 12354
title: "False positive ASYNC100 when pairing `asyncio.timeout` with `asyncio.TaskGroup`"
type: issue
state: closed
author: jamesbraza
labels:
  - bug
assignees: []
created_at: 2024-07-17T00:08:25Z
updated_at: 2024-08-01T02:12:44Z
url: https://github.com/astral-sh/ruff/issues/12354
synced_at: 2026-01-10T11:09:54Z
```

# False positive ASYNC100 when pairing `asyncio.timeout` with `asyncio.TaskGroup`

---

_Issue opened by @jamesbraza on 2024-07-17 00:08_

The below Python 3.12 code with `ruff==0.5.2`:

```python
import asyncio

async def foo() -> None:
    async with asyncio.timeout(2.0), asyncio.TaskGroup() as tg:
        tg.create_task(asyncio.sleep(1))
```

Will throw an ASYNC100 error on the fourth line:

```none
a.py:4:5: ASYNC100 A `with asyncio.timeout(...):` context does not contain any `await` statements. This makes it pointless, as the timeout can only be triggered by a checkpoint.
```

I think this is a false positive, as the inner `asyncio.TaskGroup` has an implicit `await` during its `__exit__`

---

_Label `bug` added by @charliermarsh on 2024-07-17 00:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-01 02:09_

---

_Closed by @charliermarsh on 2024-08-01 02:12_

---
