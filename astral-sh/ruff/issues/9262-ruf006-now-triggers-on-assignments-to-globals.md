```yaml
number: 9262
title: RUF006 now triggers on assignments to globals
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
assignees: []
created_at: 2023-12-23T19:04:27Z
updated_at: 2023-12-23T21:33:52Z
url: https://github.com/astral-sh/ruff/issues/9262
synced_at: 2026-01-12T15:54:49Z
```

# RUF006 now triggers on assignments to globals

---

_@hauntsaninja_

Since https://github.com/astral-sh/ruff/pull/9060, ruff now complains about:
```
import asyncio

consumer_task: asyncio.Task

class Something:
    @classmethod
    async def start_async(cls, something):
        global consumer_task
        consumer_task = asyncio.create_task(something.consume_messages())
        return something
```
Similar issue applies to assignments to nonlocal

Filing as per https://github.com/astral-sh/ruff/pull/9060#issuecomment-1868345123

---

_Comment by @charliermarsh on 2023-12-23 19:09_

Thanks! Will fix.

---

_Label `bug` added by @charliermarsh on 2023-12-23 19:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-23 19:09_

---

_Closed by @charliermarsh on 2023-12-23 21:33_

---
