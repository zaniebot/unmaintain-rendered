```yaml
number: 11018
title: "`unused-async` (`RUF029`) - false positive in `.pyi` files"
type: issue
state: closed
author: DetachHead
labels:
  - bug
assignees: []
created_at: 2024-04-19T00:18:56Z
updated_at: 2024-04-19T04:03:53Z
url: https://github.com/astral-sh/ruff/issues/11018
synced_at: 2026-01-10T11:09:53Z
```

# `unused-async` (`RUF029`) - false positive in `.pyi` files

---

_Issue opened by @DetachHead on 2024-04-19 00:18_

```py
async def foo() -> int: ... # error: Function `foo` is declared `async`, but doesn't `await` or use `async` features.
```

this is a stub for a function that returns `Coroutine[Any, Any, int]`. removing the `async` will result in an incorrect return type

---

_Label `bug` added by @charliermarsh on 2024-04-19 03:41_

---

_Comment by @charliermarsh on 2024-04-19 03:41_

I think we should omit stubs entirely.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-19 03:42_

---

_Closed by @charliermarsh on 2024-04-19 04:03_

---
