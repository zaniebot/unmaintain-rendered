```yaml
number: 17432
title: "[red-knot] understand equivalence of specialized generic classes"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-04-16T18:21:17Z
updated_at: 2025-04-18T15:49:23Z
url: https://github.com/astral-sh/ruff/issues/17432
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] understand equivalence of specialized generic classes

---

_@carljm_

Observed by @cake-monotone in https://github.com/astral-sh/ruff/pull/17174#discussion_r2046229239

```py
from knot_extensions import is_subtype_of, static_assert

class A[T]:
    pass

class B(A[int]):
    pass

static_assert(is_subtype_of(B, A[int]))  # fails, but should pass
```

[Playground](https://types.ruff.rs/e3a48a7c-b17d-4a6d-977b-7fbf0d0ec1f3)

---

_Label `red-knot` added by @carljm on 2025-04-16 18:21_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-04-16 18:21_

---

_Comment by @carljm on 2025-04-16 18:21_

cc @dcreager , may be a known issue with an already-planned fix? but seemed worth tracking anyway

---

_Comment by @dcreager on 2025-04-16 18:44_

Yes, we're not handling "specialization via subscript" in a type expression yet

---

_Comment by @dcreager on 2025-04-16 19:29_

@sharkdp also encountered this on a `dataclass` PR: https://github.com/astral-sh/ruff/pull/17406/files#r2045391765



---

_Closed by @dcreager on 2025-04-18 15:49_

---

_Closed by @dcreager on 2025-04-18 15:49_

---
