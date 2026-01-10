---
number: 11043
title: RUF029 trigger errors on async interators
type: issue
state: closed
author: Greesb
labels:
  - bug
assignees: []
created_at: 2024-04-19T14:07:38Z
updated_at: 2024-04-21T12:17:27Z
url: https://github.com/astral-sh/ruff/issues/11043
synced_at: 2026-01-10T01:22:50Z
---

# RUF029 trigger errors on async interators

---

_Issue opened by @Greesb on 2024-04-19 14:07_

Here's a sample code for reproduction:
```python
async def test():
    return [check async for check in async_func()] 
```
Error:
```RUF029 Function `test` is declared `async`, but doesn't `await` or use `async` features.```

---

_Label `bug` added by @charliermarsh on 2024-04-20 01:50_

---

_Comment by @charliermarsh on 2024-04-20 01:50_

Thanks!

---

_Comment by @charliermarsh on 2024-04-20 01:50_

\cc @plredmond 

---

_Referenced in [astral-sh/ruff#11070](../../astral-sh/ruff/pulls/11070.md) on 2024-04-21 12:04_

---

_Closed by @charliermarsh on 2024-04-21 12:17_

---

_Referenced in [astral-sh/ruff#11218](../../astral-sh/ruff/issues/11218.md) on 2024-04-30 18:00_

---

_Referenced in [astral-sh/ruff#11768](../../astral-sh/ruff/issues/11768.md) on 2024-06-06 00:35_

---
