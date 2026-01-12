```yaml
number: 1266
title: Bidirectional type inference with unions
type: issue
state: closed
author: ibraheemdev
labels:
  - bidirectional inference
assignees: []
created_at: 2025-09-26T23:08:40Z
updated_at: 2025-10-16T19:17:39Z
url: https://github.com/astral-sh/ty/issues/1266
synced_at: 2026-01-12T15:54:24Z
```

# Bidirectional type inference with unions

---

_@ibraheemdev_

This snippet currently type-checks:
```py
from typing import TypedDict, overload, reveal_type

class T(TypedDict):
  x: int


def f(x: T): ...

x: T | int = { "y": 1 }
reveal_type(x)
```

Bi-directional type-inference doesn't see through the union, so we infer `dict[Unknown | str, Unknown | int]`. We also don't end up validating the `TypedDict` constructor.

---

_Label `bidirectional inference` added by @ibraheemdev on 2025-09-26 23:08_

---

_Renamed from "Bi-directional type inference with unions" to "Bidirectional type inference with unions" by @AlexWaygood on 2025-10-06 17:03_

---

_Closed by @ibraheemdev on 2025-10-16 19:17_

---
