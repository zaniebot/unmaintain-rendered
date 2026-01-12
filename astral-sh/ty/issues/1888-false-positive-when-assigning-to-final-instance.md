```yaml
number: 1888
title: False positive when assigning to Final instance attribute in __init__ and adding generics
type: issue
state: open
author: AndreuCodina
labels:
  - typing semantics
assignees: []
created_at: 2025-12-15T13:10:23Z
updated_at: 2025-12-16T01:25:59Z
url: https://github.com/astral-sh/ty/issues/1888
synced_at: 2026-01-12T15:54:26Z
```

# False positive when assigning to Final instance attribute in __init__ and adding generics

---

_@AndreuCodina_

### Summary

Similar to the closed https://github.com/astral-sh/ty/issues/1409, but when adding generics.

```python
import asyncio
from typing import Final


class AsyncConcurrentDictionary[TKey, TValue]:
    _dict: Final[dict[TKey, TValue]]
    _lock: Final[asyncio.Lock]

    def __init__(self) -> None:
        self._dict = {}  # error[invalid-assignment]: Cannot assign to final attribute `_dict` on type `Self@__init__`
        self._lock = asyncio.Lock()  # error[invalid-assignment]: Cannot assign to final attribute `_lock` on type `Self@__init__`
```

https://play.ty.dev/2cb2f484-013d-4da2-a7f4-32a0144f5273

After some experiments, I figured out that if you remove the generics or repeat the type hints in `__init__`, the errors disappear, i.e.:

```python
import asyncio
from typing import Final


class AsyncConcurrentDictionary:
    _dict: Final[dict[int, int]]
    _lock: Final[asyncio.Lock]

    def __init__(self) -> None:
        self._dict = {}
        self._lock = asyncio.Lock()

# or

class AsyncConcurrentDictionary[TKey, TValue]:
    _dict: Final[dict[TKey, TValue]]
    _lock: Final[asyncio.Lock]

    def __init__(self) -> None:
        self._dict: dict[TKey, TValue] = {}
        self._lock: asyncio.Lock = asyncio.Lock()
```

### Version

0.0.1a34

---

_Added to milestone `Stable` by @carljm on 2025-12-16 01:25_

---

_Label `typing semantics` added by @carljm on 2025-12-16 01:25_

---
