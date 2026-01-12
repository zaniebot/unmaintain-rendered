```yaml
number: 11681
title: "[red-knot] store child scopes using a Range instead of a Vec"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-06-01T16:36:42Z
updated_at: 2024-07-17T06:54:36Z
url: https://github.com/astral-sh/ruff/issues/11681
synced_at: 2026-01-12T15:54:51Z
```

# [red-knot] store child scopes using a Range instead of a Vec

---

_@carljm_

(idea from Micha)

Instead of `Scope::children` being a vec of scope IDs, it could just be a Range of descendants (because descendant scopes of a given scope should all be interned contiguously). This would mean that finding direct children would require filtering on the descendants' parent scope ID, but it would avoid allocating (and deallocating) all those vectors.


---

_Label `red-knot` added by @carljm on 2024-06-01 16:36_

---

_Assigned to @carljm by @carljm on 2024-06-04 19:28_

---

_Comment by @MichaReiser on 2024-07-17 06:54_

I did this in the salsa refactor

---

_Closed by @MichaReiser on 2024-07-17 06:54_

---
