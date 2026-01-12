```yaml
number: 17456
title: "[red-knot] class bases are not affected by __future__.annotations"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/classbases
created_at: 2025-04-18T00:34:35Z
updated_at: 2025-04-18T13:46:24Z
url: https://github.com/astral-sh/ruff/pull/17456
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] class bases are not affected by __future__.annotations

---

_@carljm_

## Summary

We were over-conflating the conditions for deferred name resolution. `from __future__ import annotations` defers annotations, but not class bases. In stub files, class bases are also deferred. Modeling this correctly also reduces likelihood of cycles in Python files using `from __future__ import annotations` (since deferred resolution is inherently cycle-prone). The same cycles are still possible in `.pyi` files, but much less likely, since typically there isn't anything in a `pyi` file that would cause an early return from a scope, or otherwise cause visibility constraints to persist to end of scope. Usually there is only code at module global scope and class scope, which can't have `return` statements, and `raise` or `assert` statements in a stub file would be very strange. (Technically according to the spec we'd be within our rights to just forbid a whole bunch of syntax outright in a stub file, but  I kinda like minimizing unnecessary differences between the handling of Python files and stub files.)

## Test Plan

Added mdtests.


---

_Label `red-knot` added by @carljm on 2025-04-18 00:34_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-18 00:34_

---

_Review requested from @sharkdp by @carljm on 2025-04-18 00:34_

---

_Review requested from @dcreager by @carljm on 2025-04-18 00:34_

---

_Comment by @github-actions[bot] on 2025-04-18 00:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood approved on 2025-04-18 09:49_

Great catch!

---

_Merged by @carljm on 2025-04-18 13:46_

---

_Closed by @carljm on 2025-04-18 13:46_

---

_Branch deleted on 2025-04-18 13:46_

---
