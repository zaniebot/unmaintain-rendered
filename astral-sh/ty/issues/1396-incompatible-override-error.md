```yaml
number: 1396
title: incompatible override error
type: issue
state: closed
author: vlashada
labels: []
assignees: []
created_at: 2025-10-18T22:31:39Z
updated_at: 2025-10-19T06:56:23Z
url: https://github.com/astral-sh/ty/issues/1396
synced_at: 2026-01-10T02:06:25Z
```

# incompatible override error

---

_Issue opened by @vlashada on 2025-10-18 22:31_

### Summary

In the example below, the type of `reveal_type(some_method(b))` is `str` and not `int`. I think there should be an incompatible override error.

```python
from typing import Self, reveal_type


class A[T]:
    item: T

    def method(self: Self) -> T:
        return self.item


class B(A[str]):
    def method(self: Self) -> int:
        return 42


def some_method[T](a: A[T]) -> T:
    return a.method()


reveal_type(some_method(B()))
```

### Version

ty 0.0.1-alpha.23 (5c51b8480 2025-10-16)

---

_Renamed from "Warn on incompatible override of method" to "incompatible override error" by @vlashada on 2025-10-18 22:31_

---

_Comment by @sharkdp on 2025-10-19 06:55_

Thank you for reporting this. We don't support this yet, but plan to do so soon. See #166 for details.

---

_Closed by @sharkdp on 2025-10-19 06:56_

---
