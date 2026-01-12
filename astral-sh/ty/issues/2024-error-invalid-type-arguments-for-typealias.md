```yaml
number: 2024
title: "error[invalid-type-arguments] for TypeAlias involving ParamSpec Concatentate"
type: issue
state: open
author: GalHorowitz
labels:
  - generics
assignees: []
created_at: 2025-12-17T17:43:35Z
updated_at: 2025-12-17T18:15:25Z
url: https://github.com/astral-sh/ty/issues/2024
synced_at: 2026-01-12T15:54:26Z
```

# error[invalid-type-arguments] for TypeAlias involving ParamSpec Concatentate

---

_@GalHorowitz_

### Summary

Example code:
```python
from typing import Callable, Concatenate, ParamSpec, TypeAlias, TypeVar

T = TypeVar("T")
P = ParamSpec("P")

A: TypeAlias = Callable[Concatenate[bool, P], T]
B: TypeAlias = T | A[P, T]
C: TypeAlias = B[T, []]
```

Result of running `ty check`:
```
error[invalid-type-arguments]: No type argument provided for required type variable `T`
 --> ty_test.py:8:16
  |
6 | A: TypeAlias = Callable[Concatenate[bool, P], T]
7 | B: TypeAlias = T | A[P, T]
8 | C: TypeAlias = B[T, []]
  |                ^^^^^^^^
  |
info: rule `invalid-type-arguments` is enabled by default
```

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Comment by @AlexWaygood on 2025-12-17 17:46_

Thanks for the report! We don't support `Concatenate` yet; I've added this as a child issue of #1535.

---

_Label `generics` added by @AlexWaygood on 2025-12-17 17:54_

---

_Added to milestone `Stable` by @carljm on 2025-12-17 18:15_

---
