```yaml
number: 1342
title: Type relations for function-like callables are too strict
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - calls
assignees: []
created_at: 2025-10-13T11:03:34Z
updated_at: 2025-10-13T13:21:56Z
url: https://github.com/astral-sh/ty/issues/1342
synced_at: 2026-01-12T15:54:25Z
```

# Type relations for function-like callables are too strict

---

_@sharkdp_

Assignability, subtyping, and maybe also equivalence are currently too strict for function-like callables. For example, it should be possible to assign `Mock()` to a function-like callable, but we currently get:


```py
from dataclasses import dataclass
from unittest.mock import Mock

@dataclass(order=True)
class Data:
    value: int

def test_t():
    d = Data(1)
    # error: Object of type `Mock` is not assignable to
    # attribute `__lt__` of type `(self: Data, other: Data) -> bool`
    d.__lt__ = Mock()
```
https://play.ty.dev/8148ed96-48fa-4706-9e50-2ff61f0f5a05

---

_Label `bug` added by @sharkdp on 2025-10-13 11:03_

---

_Label `calls` added by @sharkdp on 2025-10-13 11:03_

---

_Assigned to @sharkdp by @sharkdp on 2025-10-13 11:03_

---

_Closed by @sharkdp on 2025-10-13 13:21_

---
