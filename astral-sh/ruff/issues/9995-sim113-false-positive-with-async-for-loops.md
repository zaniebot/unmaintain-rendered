```yaml
number: 9995
title: SIM113 false positive with async for loops
type: issue
state: closed
author: adrienball
labels:
  - bug
assignees: []
created_at: 2024-02-15T10:42:26Z
updated_at: 2024-02-16T03:40:02Z
url: https://github.com/astral-sh/ruff/issues/9995
synced_at: 2026-01-12T15:54:49Z
```

# SIM113 false positive with async for loops

---

_@adrienball_

### Steps to reproduce
- Create a debug.py file with the following content:
```python
async def my_async_gen():
    for i in range(5):
        yield i


async def my_async_func():
    idx = 0
    async for v in my_async_gen():
        idx += 1
        print(f"{idx}: {v}")
```
- Run `ruff check debug.py`

### Expected behavior
No error

### Actual behavior
```console
debug.py:9:9: SIM113 Use `enumerate()` for index variable `idx` in `for` loop
Found 1 error.
```

### Environment
```console
> ruff --version
ruff 0.2.1
```



---

_Label `bug` added by @charliermarsh on 2024-02-16 03:39_

---

_Comment by @charliermarsh on 2024-02-16 03:39_

Ahh, nice catch!

---

_Closed by @charliermarsh on 2024-02-16 03:40_

---
