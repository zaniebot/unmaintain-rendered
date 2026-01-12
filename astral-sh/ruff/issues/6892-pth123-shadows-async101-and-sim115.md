```yaml
number: 6892
title: PTH123 shadows ASYNC101 and SIM115
type: issue
state: closed
author: Olegt0rr
labels:
  - bug
  - rule
assignees: []
created_at: 2023-08-26T07:52:09Z
updated_at: 2024-01-30T17:41:02Z
url: https://github.com/astral-sh/ruff/issues/6892
synced_at: 2026-01-12T15:54:46Z
```

# PTH123 shadows ASYNC101 and SIM115

---

_@Olegt0rr_

Let's ruff:

```python
async def foo() -> None:
    """Open something."""
    open("foo")
```

Result:
```
ASYNC101 Async functions should not call `open`, `time.sleep`, or `subprocess` methods
SIM115 Use context handler for opening files
PTH123 `open()` should be replaced by `Path.open()`

```

Let's apply fix for `PTH123` and ruff it again:

```python
async def foo() -> None:
    """Open something."""
    path = Path("foo")
    path.open("foo")

```

Result: `success`

Expected:

- ASYNC101 (cause `path.open` is also blocking as `open` for async.)
- SIM115 (cause `path.open` works the same as `open`)


---

_Comment by @zanieb on 2023-08-26 14:43_

Sounds like `PTH123` needs to be updated to be async aware and `ASYNC101` should probably raise on `Path.open` too?

---

_Label `bug` added by @zanieb on 2023-08-26 14:43_

---

_Label `rule` added by @zanieb on 2023-08-26 14:43_

---

_Comment by @Olegt0rr on 2023-08-26 15:33_

That's right, but it doesn't seem so easy to catch `path.open()`, since the `Path` object can be named differently.

---

_Comment by @charliermarsh on 2024-01-30 17:41_

Gonna call this one closed by https://github.com/astral-sh/ruff/pull/9703.

---

_Closed by @charliermarsh on 2024-01-30 17:41_

---
