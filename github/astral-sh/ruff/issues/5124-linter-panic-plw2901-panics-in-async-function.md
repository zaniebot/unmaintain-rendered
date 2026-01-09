---
number: 5124
title: "[Linter panic] PLW2901 panics in async function"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-06-15T18:23:11Z
updated_at: 2023-06-15T19:00:20Z
url: https://github.com/astral-sh/ruff/issues/5124
synced_at: 2026-01-07T13:12:15-06:00
---

# [Linter panic] PLW2901 panics in async function

---

_Issue opened by @konstin on 2023-06-15 18:23_

The following code panics in rule PLW2901 redefined-loop-name:
```python
async def f():
    async with a:
        return await b
```
Command:
```shell
ruff --no-cache --select PLW2901 tasks.py
```
This is an explicit panic, so i'm omitting the backtrace https://github.com/astral-sh/ruff/blob/66089e1a2e4b8b6ad6de3dc9543cd9a8d2a764e3/crates/ruff/src/rules/pylint/rules/redefined_loop_name.rs#L378

---

_Label `bug` added by @konstin on 2023-06-15 18:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-15 18:39_

---

_Referenced in [astral-sh/ruff#5125](../../astral-sh/ruff/pulls/5125.md) on 2023-06-15 18:46_

---

_Closed by @charliermarsh on 2023-06-15 19:00_

---
