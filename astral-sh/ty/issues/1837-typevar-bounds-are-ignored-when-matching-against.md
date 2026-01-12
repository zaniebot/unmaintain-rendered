```yaml
number: 1837
title: Typevar bounds are ignored when matching against generic union
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - generics
assignees: []
created_at: 2025-12-10T11:45:37Z
updated_at: 2025-12-10T13:58:59Z
url: https://github.com/astral-sh/ty/issues/1837
synced_at: 2026-01-12T15:54:25Z
```

# Typevar bounds are ignored when matching against generic union

---

_@sharkdp_

This is a heavily modified MRE for a bug that I observed in `pandas`, while calling `np.zeros(…)`. The `accepts_dtype` call in the following example should not succeed:

```py
from typing import TypeVar, Protocol

class dtype: ...

_DTypeT_co = TypeVar("_DTypeT_co", bound=dtype, covariant=True)
_DTypeT = TypeVar("_DTypeT", bound=dtype)

class _SupportsDType(Protocol[_DTypeT_co]):
    @property
    def dtype(self) -> _DTypeT_co: ...

def accepts_dtype(dtype: _DTypeT | _SupportsDType[_DTypeT]) -> _DTypeT:
    raise NotImplementedError

reveal_type(accepts_dtype([1, 2]))  # revealed: list[Unknown | int]
```

I haven't figured out what is going on here. The union in the `dtype` parameter annotation is required. If we remove either side of the union, the call fails as expected.

The call also doesn't succeed with an argument like `1`, so it seems to be specific to `list`?

---

_Label `bug` added by @sharkdp on 2025-12-10 11:45_

---

_Label `generics` added by @sharkdp on 2025-12-10 11:45_

---

_Label `Protocols` added by @sharkdp on 2025-12-10 11:45_

---

_Comment by @sharkdp on 2025-12-10 11:57_

This is not related to protocols, and not related to `list` (just needs to be something that is not disjoint). Probably a bug in union handling in the generics solver?
```py
class Unrelated: ...

def accepts_str_or_unrelated[T: str](x: T | Unrelated) -> T:
    raise NotImplementedError

class C: ...

reveal_type(accepts_str_or_unrelated(C()))  # revealed: C
```

---

_Label `Protocols` removed by @sharkdp on 2025-12-10 11:57_

---

_Assigned to @sharkdp by @sharkdp on 2025-12-10 12:11_

---

_Renamed from "Incorrect succeeding assignment to union of typevar and generic protocol" to "Incorrect succeeding assignment to union of typevar and a non-disjoint other type" by @AlexWaygood on 2025-12-10 12:15_

---

_Renamed from "Incorrect succeeding assignment to union of typevar and a non-disjoint other type" to "Typevar bounds are ignored when matching against generic union" by @sharkdp on 2025-12-10 12:27_

---

_Closed by @sharkdp on 2025-12-10 13:58_

---
