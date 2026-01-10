```yaml
number: 17189
title: "[red-knot] don't remove negations when simplifying constrained typevars"
type: pull_request
state: merged
author: carljm
labels: []
assignees: []
merged: true
base: main
head: cjm/tvinter
created_at: 2025-04-03T23:15:55Z
updated_at: 2025-04-03T23:30:59Z
url: https://github.com/astral-sh/ruff/pull/17189
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] don't remove negations when simplifying constrained typevars

---

_Pull request opened by @carljm on 2025-04-03 23:15_

For two non-disjoint types `P` and `Q`, the simplification of `(P | Q) & ~Q` is not `P`, but `P & ~Q`. In other words, the non-empty set `P & Q` is also excluded from the type.

The same applies for a constrained typevar `[T: (P, Q)]`: `T & ~Q` should simplify to `P & ~Q`, not just `P`.

Implementing this is actually purely a matter of removing code from the constrained typevar simplification logic; we just need to not bother removing the negations. If the negations are actually redundant (because the constraint types are disjoint), normal intersection simplification will already eliminate them (as shown in the added test.)

---

_Review requested from @AlexWaygood by @carljm on 2025-04-03 23:15_

---

_Review requested from @sharkdp by @carljm on 2025-04-03 23:15_

---

_Review requested from @dcreager by @carljm on 2025-04-03 23:15_

---

_Comment by @github-actions[bot] on 2025-04-03 23:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dcreager approved on 2025-04-03 23:27_

Thanks!

---

_Merged by @carljm on 2025-04-03 23:30_

---

_Closed by @carljm on 2025-04-03 23:30_

---

_Branch deleted on 2025-04-03 23:30_

---
