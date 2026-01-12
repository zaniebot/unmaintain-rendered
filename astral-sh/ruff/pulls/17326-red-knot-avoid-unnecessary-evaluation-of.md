```yaml
number: 17326
title: "[red-knot] avoid unnecessary evaluation of visibility constraint on definitely-unbound symbol"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/unbound
created_at: 2025-04-10T03:55:36Z
updated_at: 2025-04-10T14:05:40Z
url: https://github.com/astral-sh/ruff/pull/17326
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] avoid unnecessary evaluation of visibility constraint on definitely-unbound symbol

---

_@carljm_

This causes spurious query cycles.

This PR also includes an update to Salsa, which gives us db events on cycle iteration, so we can write tests asserting the absence of a cycle.


---

_Label `red-knot` added by @carljm on 2025-04-10 03:55_

---

_Comment by @github-actions[bot] on 2025-04-10 03:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-10 04:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @carljm on 2025-04-10 13:39_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-10 13:39_

---

_Review requested from @sharkdp by @carljm on 2025-04-10 13:39_

---

_Review requested from @dcreager by @carljm on 2025-04-10 13:39_

---

_Review requested from @MichaReiser by @carljm on 2025-04-10 13:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/symbol.rs`:678 on 2025-04-10 13:50_

worth adding a comment here explaining why it's important for this to be only lazily evaluated, rather than just evaluating it once upfront?

---

_@AlexWaygood approved on 2025-04-10 13:50_

---

_@carljm reviewed on 2025-04-10 13:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/symbol.rs`:678 on 2025-04-10 13:56_

Good call, done

---

_Merged by @carljm on 2025-04-10 13:59_

---

_Closed by @carljm on 2025-04-10 13:59_

---

_Branch deleted on 2025-04-10 13:59_

---
