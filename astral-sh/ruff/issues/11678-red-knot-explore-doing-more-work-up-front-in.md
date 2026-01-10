---
number: 11678
title: "[red-knot] Explore doing more work up front in semantic indexing"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-06-01T16:00:13Z
updated_at: 2024-08-24T02:19:06Z
url: https://github.com/astral-sh/ruff/issues/11678
synced_at: 2026-01-10T01:22:51Z
---

# [red-knot] Explore doing more work up front in semantic indexing

---

_Issue opened by @carljm on 2024-06-01 16:00_

Instead of building a CFG and lazily traversing it backwards whenever we resolve types, we could instead build use-def links directly up-front, reducing the work to be done in type resolution.

The challenge here is type narrowing. Each use-def link would also need to be associated with a list of potential narrowing effects to be applied in type resolution. And we would need to represent these potential narrowing effects in a way that doesn't depend on resolving types in e.g. an `if` test up-front (resolving types can always cross modules, and we want to ensure that only happens lazily.) But this should be possible; it should be no different than how we would represent the same narrowing effect as a node in the CFG.

This would be pretty similar to transforming to an SSA representation, with the addition of narrowing-effects associated with each use.


---

_Label `red-knot` added by @carljm on 2024-06-01 16:00_

---

_Assigned to @carljm by @carljm on 2024-06-01 16:00_

---

_Comment by @carljm on 2024-08-05 17:36_

This is mostly done, but I'll wait to close it until I've added type narrowing as well.

---

_Closed by @carljm on 2024-08-24 02:19_

---
