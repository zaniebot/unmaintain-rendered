```yaml
number: 677
title: Support generic type aliases
type: issue
state: closed
author: tmke8
labels:
  - generics
  - typing semantics
assignees: []
created_at: 2025-06-18T13:20:06Z
updated_at: 2025-09-08T20:26:22Z
url: https://github.com/astral-sh/ty/issues/677
synced_at: 2026-01-10T02:06:24Z
```

# Support generic type aliases

---

_Issue opened by @tmke8 on 2025-06-18 13:20_

### Summary

Consider

```python
from typing import reveal_type

type L1 = list[int]
type L2[T] = list[T]

def f(x: L1, y: L2[int]) -> None:
    reveal_type(x)  # list[int]
    reveal_type(y)  # @Todo
```

([playground link](https://play.ty.dev/8bd9444e-48f9-4ad8-bd02-a0dc3183451c))

The non-generic type alias works fine, but the generic one results in a `@Todo` type.

I saw there are existing issues on type aliases (like #221 ), but I didn't see an issue directly about generic type aliases.

The explicit `TypeAliasType` construction should maybe also be supported:

```python
from typing import TypeAliasType, TypeVar, reveal_type

L1 = TypeAliasType("L1", list[int])
T = TypeVar("T")
L2 = TypeAliasType("L2", list[T], type_params=(T,))

def f(x: L1, y: L2[int]) -> None:
    reveal_type(x)  # list[int]
    reveal_type(y)  # @Todo
```

### Version

ty 0.0.1-alpha.11

---

_Label `typing semantics` added by @AlexWaygood on 2025-06-18 13:43_

---

_Label `generics` added by @carljm on 2025-06-18 19:07_

---

_Renamed from "Support generic type aliases" to "Support generic (explicit) type aliases" by @tmke8 on 2025-06-25 14:18_

---

_Added to milestone `Beta` by @carljm on 2025-08-13 22:41_

---

_Renamed from "Support generic (explicit) type aliases" to "Support generic PEP 695 type aliases" by @carljm on 2025-08-15 15:24_

---

_Renamed from "Support generic PEP 695 type aliases" to "Support generic type aliases" by @carljm on 2025-08-15 15:26_

---

_Assigned to @ibraheemdev by @carljm on 2025-08-22 14:58_

---

_Closed by @carljm on 2025-09-08 20:26_

---
