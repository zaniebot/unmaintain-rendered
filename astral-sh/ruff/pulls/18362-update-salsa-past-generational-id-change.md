```yaml
number: 18362
title: Update salsa past generational id change
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/update-salsa-generational
created_at: 2025-05-29T09:10:32Z
updated_at: 2025-05-30T13:31:36Z
url: https://github.com/astral-sh/ruff/pull/18362
synced_at: 2026-01-12T15:56:17Z
```

# Update salsa past generational id change

---

_@MichaReiser_

## Summary

This PR updates salsa past the generational ID changes.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-05-29 09:10_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-29 09:10_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-29 09:10_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-29 09:10_

---

_Comment by @github-actions[bot] on 2025-05-29 09:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `internal` added by @MichaReiser on 2025-05-29 09:13_

---

_@MichaReiser reviewed on 2025-05-29 09:21_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:9029 on 2025-05-29 09:21_

`salsa::Id` is now a u64 and not a u32 anymore

---

_Comment by @github-actions[bot] on 2025-05-29 09:23_

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

_@dcreager approved on 2025-05-29 13:12_

---

_Merged by @MichaReiser on 2025-05-30 13:31_

---

_Closed by @MichaReiser on 2025-05-30 13:31_

---

_Branch deleted on 2025-05-30 13:31_

---
