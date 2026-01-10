---
number: 17364
title: "[red-knot] `T & ~X` should be assignable to `T`"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2025-04-12T00:50:25Z
updated_at: 2025-04-15T20:34:09Z
url: https://github.com/astral-sh/ruff/issues/17364
synced_at: 2026-01-10T01:22:58Z
---

# [red-knot] `T & ~X` should be assignable to `T`

---

_Issue opened by @carljm on 2025-04-12 00:50_

[This](https://types.ruff.rs/327f3b66-7d85-452b-9af7-66c1ede9913f) does not type check, but it should:

```py
def union_param[T](x: T | None) -> T:
    if x is None:
        raise ValueError("don't give me that none")
    return x  # bogus error: expected T, found T & ~None
```

[The equivalent](https://types.ruff.rs/9e7dc33b-168b-44c3-b06d-e5ce004523c2) where we replace `T` with a concrete type, does type check:

```py
def union_param(x: int | None) -> int:
    if x is None:
        raise ValueError("don't give me that none")
    return x
```

Placing this in alpha milestone because I've run into it already several times, I think it will be a common source of false positives in generic functions.

---

_Label `red-knot` added by @carljm on 2025-04-12 00:50_

---

_Added to milestone `Red Knot Alpha` by @carljm on 2025-04-12 00:50_

---

_Referenced in [astral-sh/ruff#17301](../../astral-sh/ruff/pulls/17301.md) on 2025-04-12 01:53_

---

_Assigned to @dcreager by @dcreager on 2025-04-15 18:07_

---

_Referenced in [astral-sh/ruff#17413](../../astral-sh/ruff/pulls/17413.md) on 2025-04-15 19:27_

---

_Closed by @dcreager on 2025-04-15 20:34_

---
