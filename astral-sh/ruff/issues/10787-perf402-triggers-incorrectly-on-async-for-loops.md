```yaml
number: 10787
title: "`PERF402` triggers, incorrectly, on async for loops"
type: issue
state: closed
author: sliedes
labels:
  - rule
assignees: []
created_at: 2024-04-05T09:28:35Z
updated_at: 2024-05-02T18:48:54Z
url: https://github.com/astral-sh/ruff/issues/10787
synced_at: 2026-01-12T15:54:50Z
```

# `PERF402` triggers, incorrectly, on async for loops

---

_@sliedes_

Searched keywords: PERF402. #10322 is similar, but more complicated (and closed by the submitter).

This code triggers PERF402 on ruff 0.3.5:

```python
from typing import AsyncIterator


async def some_async_iterable() -> AsyncIterator[int]:
    for i in range(10):
        yield i


async def foo() -> None:
    values: list[int] = []
    async for i in some_async_iterable():
        values.append(i)
```

Result from running `ruff check --isolated --select PERF402`:

```
bug.py:12:9: PERF402 Use `list` or `list.copy` to create a copy of a list
Found 1 error.
```

But `list` and `list.copy` cannot be used on an async iterator. #10322 suggested using an async list comprehension, but I think that's a different animal; in the very least the error message is wrong, and a check that would suggest using a list comprehension instead of a loop would be rather different in spirit.

---

_Label `rule` added by @MichaReiser on 2024-04-05 10:12_

---

_Comment by @CoolCat467 on 2024-04-22 22:32_

I agree the error message is misleading, but yes using an async list comprehension should be the fix here:
```python
from typing import AsyncGenerator


async def some_async_iterable() -> AsyncGenerator[int, None]:
    for i in range(10):
        yield i


async def foo() -> None:
    values: list[int] = [i async for i in some_async_iterable()]
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-02 18:06_

---

_Closed by @charliermarsh on 2024-05-02 18:48_

---
