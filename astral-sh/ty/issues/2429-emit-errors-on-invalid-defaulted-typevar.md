```yaml
number: 2429
title: Emit errors on invalid defaulted TypeVar constructions
type: issue
state: open
author: MeGaGiGaGon
labels:
  - typing semantics
assignees: []
created_at: 2026-01-09T22:19:00Z
updated_at: 2026-01-10T22:36:52Z
url: https://github.com/astral-sh/ty/issues/2429
synced_at: 2026-01-12T15:54:26Z
```

# Emit errors on invalid defaulted TypeVar constructions

---

_@MeGaGiGaGon_

### Summary

When a TypeVar has a default, there are some bounds/constraints that may be invalid. ty should emit an error for these cases. This is probably already part of the typing conformance suite somewhere, but I couldn't find an existing issue for it, so I figured I'd open one.

From these links:
https://typing.python.org/en/latest/spec/generics.html#bound-rules
https://typing.python.org/en/latest/spec/generics.html#constraint-rules

https://play.ty.dev/8b5084ae-3a52-439a-b28a-6aedaad52466
```py
from typing import TypeVar

T1 = TypeVar("T1", bound=int)
Ok = TypeVar("Ok", default=T1, bound=float)     # Valid
AlsoOk = TypeVar("AlsoOk", default=T1, bound=int)   # Valid
Invalid = TypeVar("Invalid", default=T1, bound=str)  # Invalid: int is not assignable to str

T2 = TypeVar("T2", bound=int)
Invalid1 = TypeVar("Invalid1", float, str, default=T2)         # Invalid: upper bound int is incompatible with constraints float or str

T3 = TypeVar("T3", int, str)
AlsoOk1 = TypeVar("AlsoOk1", int, str, bool, default=T3)      # Valid
AlsoInvalid = TypeVar("AlsoInvalid", bool, complex, default=T3)  # Invalid: {bool, complex} is not a superset of {int, str}
```
Currently none of the `Invalid` lines give an error.

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---

_Added to milestone `Stable` by @carljm on 2026-01-09 22:52_

---

_Label `typing semantics` added by @AlexWaygood on 2026-01-10 22:36_

---
