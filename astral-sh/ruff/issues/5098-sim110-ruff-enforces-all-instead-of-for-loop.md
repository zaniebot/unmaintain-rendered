```yaml
number: 5098
title: "[SIM110] Ruff enforces `all` instead of for loop inside async function."
type: issue
state: closed
author: real-yfprojects
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2023-06-14T19:29:35Z
updated_at: 2023-06-15T19:42:15Z
url: https://github.com/astral-sh/ruff/issues/5098
synced_at: 2026-01-10T11:09:47Z
```

# [SIM110] Ruff enforces `all` instead of for loop inside async function.

---

_Issue opened by @real-yfprojects on 2023-06-14 19:29_

The following MRE violates the *SIM110* rule. 
```py
async def coroutine():
    ...


async def test():
    for i in range(10):
        if await coroutine():
            return True
    return False
```

Ruff suggests rewriting this as

```py
async def coroutine():
    ...


async def test():
    return any(await coroutine() for i in range(10))
```

However this code doesn't work since `any` doesn't support `AsyncGenerator`s. The same applies for `all` which is also covered by this rule.
Instead ruff should ignore for loops with `await` statements.

---

_Label `bug` added by @charliermarsh on 2023-06-14 19:31_

---

_Comment by @charliermarsh on 2023-06-14 19:31_

Makes sense, thanks for reporting.

---

_Label `good first issue` added by @charliermarsh on 2023-06-14 19:36_

---

_Label `help wanted` added by @charliermarsh on 2023-06-14 19:36_

---

_Closed by @charliermarsh on 2023-06-14 22:00_

---

_Comment by @real-yfprojects on 2023-06-15 19:42_

Thank you for fixing this @tjkuson 

---
