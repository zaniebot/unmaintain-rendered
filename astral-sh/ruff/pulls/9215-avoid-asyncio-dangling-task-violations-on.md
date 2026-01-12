```yaml
number: 9215
title: "Avoid `asyncio-dangling-task` violations on shadowed bindings"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/defer
created_at: 2023-12-20T16:57:42Z
updated_at: 2023-12-20T17:10:21Z
url: https://github.com/astral-sh/ruff/pull/9215
synced_at: 2026-01-12T15:55:28Z
```

# Avoid `asyncio-dangling-task` violations on shadowed bindings

---

_@charliermarsh_

## Summary

Ensures that we avoid flagging cases like:

```python
async def f(x: int):
    if x > 0:
        task = asyncio.create_task(make_request())
    else:
        task = asyncio.create_task(make_request())
    await task
```

Closes https://github.com/astral-sh/ruff/issues/9133.


---

_Label `bug` added by @charliermarsh on 2023-12-20 16:57_

---

_Merged by @charliermarsh on 2023-12-20 17:07_

---

_Closed by @charliermarsh on 2023-12-20 17:07_

---

_Branch deleted on 2023-12-20 17:07_

---

_Comment by @github-actions[bot] on 2023-12-20 17:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
