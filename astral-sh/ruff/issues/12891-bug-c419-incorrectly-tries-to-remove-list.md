```yaml
number: 12891
title: "Bug: `C419` incorrectly tries to remove list comprehension in `async for`"
type: issue
state: closed
author: MarkusSintonen
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-08-14T10:33:51Z
updated_at: 2024-08-15T01:00:11Z
url: https://github.com/astral-sh/ruff/issues/12891
synced_at: 2026-01-12T15:54:52Z
```

# Bug: `C419` incorrectly tries to remove list comprehension in `async for`

---

_@MarkusSintonen_

Following code gives an incorrect linting error:
```python
from collections.abc import AsyncGenerator


async def test() -> None:
    async def async_gen() -> AsyncGenerator[bool, None]:
        yield True

    assert all([v async for v in async_gen()])  # C419 Unnecessary list comprehension
```

Removing the list comprehension here is incorrect as `all` can not take an async generator.
Changing it into `all(v async for v in async_gen())` gives: `TypeError: 'async_generator' object is not iterable`

Using Ruff version `0.5.7`.



---

_Label `bug` added by @AlexWaygood on 2024-08-14 10:45_

---

_Label `help wanted` added by @AlexWaygood on 2024-08-14 10:46_

---

_Closed by @charliermarsh on 2024-08-15 01:00_

---

_Closed by @charliermarsh on 2024-08-15 01:00_

---
