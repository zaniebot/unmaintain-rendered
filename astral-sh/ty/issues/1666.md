```yaml
number: 1666
title: Simplification of unions involving type variables
type: issue
state: open
author: sharkdp
labels:
  - type properties
assignees: []
created_at: 2025-11-28T14:13:15Z
updated_at: 2025-12-10T21:50:57Z
url: https://github.com/astral-sh/ty/issues/1666
synced_at: 2026-01-10T01:56:40Z
```

# Simplification of unions involving type variables

---

_Issue opened by @sharkdp on 2025-11-28 14:13_

Consider the following example:
```py
from typing import TypeVar, Any

type IntOr[T: int] = int | T

def _(x: IntOr[Any]):
    reveal_type(x)
```
https://play.ty.dev/b378c2a3-d007-428d-8d59-a3a69f5614c0

ty reveals `int` here, but every other type checker reveals `int | Any`, which seems more reasonable? ty eagerly simplifies the `int | T` union to `int`, which is correct for every static `T <: int`... but leads to surprising results when `T` is explicitly specialized to a dynamic type.


This was prompted by a much more complex example in numpy's codebase:

```py
class _SupportsDType(Protocol[_DTypeT_co]):
    @property
    def dtype(self) -> _DTypeT_co: ...

_DTypeLike = type[_ScalarT] | _SupportsDType[dtype[_ScalarT]]
```
`_ScalarT` has an upper bound of `np.generic` (= `np.generic[Any]`). And `np.generic[…]` does have a `dtype` property member. This currently leads us to treat `type[_Scalar]` as a subtype of `_SupportsDType[…]`, and so that union gets simplified to `_SupportsDType[dtype[_ScalarT]]`. Later, `_DTypeLike` is explicitly specialized with `_DTypeLike[Any]`, and it is expected that `<class 'object'>` should be assignable to `_DTypeLike[Any]` (which would be the case if `type[Any]` were still part of the union.

---

_Label `question` added by @sharkdp on 2025-11-28 14:13_

---

_Label `type properties` added by @sharkdp on 2025-11-28 14:13_

---

_Label `question` removed by @carljm on 2025-12-02 03:05_

---

_Added to milestone `Stable` by @carljm on 2025-12-02 03:05_

---

_Comment by @carljm on 2025-12-02 03:07_

Yes, it seems to me we probably need to account for the possibility of specialization to a dynamic type, and avoid doing this kind of simplification. Which would be achieved by considering `T: int` to be assignable-to, but not subtype-of or redundant-with, `int` (or `object`).

---

_Comment by @AlexWaygood on 2025-12-10 21:50_

> Which would be achieved by considering `T: int` to be assignable-to, but not subtype-of or redundant-with, `int` (or `object`).

As discussed in https://github.com/astral-sh/ty/issues/1846#issuecomment-3639019092, I agree that we probably shouldn't automatically treat `TypeVar`s as subtypes of (or redundant with) their upper bound. But I do think that we should continue to see all `TypeVar`s as subtypes of (and redundant with) `object`, because _all_ types (even `Any`!) are subtypes of `object`. So accounting for the possibility that a `TypeVar` might be solved to `Any` is immaterial there.

---
