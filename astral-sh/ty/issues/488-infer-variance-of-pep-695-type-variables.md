```yaml
number: 488
title: infer variance of PEP 695 type variables
type: issue
state: closed
author: ilius
labels:
  - generics
assignees: []
created_at: 2025-05-22T14:05:47Z
updated_at: 2025-08-21T19:25:40Z
url: https://github.com/astral-sh/ty/issues/488
synced_at: 2026-01-12T15:54:23Z
```

# infer variance of PEP 695 type variables

---

_@ilius_

### Summary

Before Python 3.12, we would write this

```py
from typing import Generic, TypeVar

T_co = TypeVar("T_co", covariant=True)

class A(Generic[T_co]):
    def __init__(self, value: T_co):
        pass


a: A[int | str] = A(1)
```

But starting from 3.12, this is equivalent:
```py
class A[T]:
    def __init__(self, value: T):
        pass


a: A[int | str] = A(1)
```

The type should be covariant according to https://peps.python.org/pep-0695/

But `ty` will say:
```
error[invalid-assignment]: Object of type `A[int]` is not assignable to `A[int | str]`
 --> ty-generic.py:6:1
  |
6 | a: A[int | str] = A(1)
  | ^
  |
info: rule `invalid-assignment` is enabled by default

Found 1 diagnostic
```

**mypy and pyright do not report any errors for this.**

### Version

ty 0.0.1-alpha.6


---

_Renamed from "New Generic syntax is not covariant" to "New Generic syntax is not used as covariant" by @ilius on 2025-05-22 14:08_

---

_Label `generics` added by @AlexWaygood on 2025-05-22 14:10_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-05-22 14:10_

---

_Label `bidirectional inference` removed by @carljm on 2025-05-22 14:17_

---

_Renamed from "New Generic syntax is not used as covariant" to "infer variance of PEP 695 type variables" by @carljm on 2025-05-22 14:17_

---

_Comment by @dcreager on 2025-05-23 14:09_

Variance inference is a known missing feature, but we did not yet have an issue to track that. So thank you for reporting this!

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:44_

---

_Comment by @ericmarkmartin on 2025-06-13 19:42_

I plan to work on this!

---

_Assigned to @ericmarkmartin by @carljm on 2025-06-13 19:45_

---

_Comment by @InSyncWithFoo on 2025-07-09 08:14_

One minor thing that we might need to look out for is "private" attributes:

```python
class C[T]:
    def __init__(self, val: T) -> None:
        self._val = val  # _val: T
```

[Mypy, Pyright and Pyrefly all say that `T` is covariant](https://stackoverflow.com/a/79695291), since they consider `_val` to be non-public. Change that to `val`, and `T` becomes invariant.


---

_Comment by @sharkdp on 2025-07-22 14:34_

I'm closing https://github.com/astral-sh/ty/issues/158, so I'm adding a note here that attributes declared `Final` might affect variance inference (covariant instead of invariant). The information whether a attribute is `Final` or not should already be available in the corresponding `qualifiers: TypeQualifiers` field when accessing a member on a class.

---

_Closed by @carljm on 2025-08-21 19:25_

---
