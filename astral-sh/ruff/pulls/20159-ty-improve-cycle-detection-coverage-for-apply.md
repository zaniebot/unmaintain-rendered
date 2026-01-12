```yaml
number: 20159
title: "[ty] improve cycle-detection coverage for apply_type_mapping"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/recursive-callable
created_at: 2025-08-29T18:38:12Z
updated_at: 2025-08-29T23:20:08Z
url: https://github.com/astral-sh/ruff/pull/20159
synced_at: 2026-01-12T15:56:55Z
```

# [ty] improve cycle-detection coverage for apply_type_mapping

---

_@carljm_

## Summary

Thread visitors through the rest of `apply_type_mapping`: callable and protocol types.

## Test Plan

Added mdtest that previously stack overflowed.


---

_Label `ty` added by @carljm on 2025-08-29 18:38_

---

_Comment by @github-actions[bot] on 2025-08-29 18:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-29 18:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-29 20:37_

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

_Marked ready for review by @carljm on 2025-08-29 20:43_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-29 20:43_

---

_Review requested from @sharkdp by @carljm on 2025-08-29 20:43_

---

_Review requested from @dcreager by @carljm on 2025-08-29 20:43_

---

_@AlexWaygood approved on 2025-08-29 23:11_

---

_Merged by @carljm on 2025-08-29 23:20_

---

_Closed by @carljm on 2025-08-29 23:20_

---

_Branch deleted on 2025-08-29 23:20_

---
