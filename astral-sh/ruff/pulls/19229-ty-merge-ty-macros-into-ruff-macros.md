```yaml
number: 19229
title: "[ty] Merge `ty_macros` into `ruff_macros`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/merge-ty-macros-into-ruff-macros
created_at: 2025-07-09T11:22:07Z
updated_at: 2025-07-09T11:37:30Z
url: https://github.com/astral-sh/ruff/pull/19229
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Merge `ty_macros` into `ruff_macros`

---

_Pull request opened by @MichaReiser on 2025-07-09 11:22_

## Summary

All ty macros except the one for deriving the environment variable documentation currently are defined in `ruff_macros`. 

This PR moves the environemtn variable macro into `ruff_macros` and deletes `ty_macros` because having some ty macros in `ruff_macros` but not others is confusing.

## Test Plan

cargo test


---

_Label `internal` added by @MichaReiser on 2025-07-09 11:25_

---

_Label `ty` added by @MichaReiser on 2025-07-09 11:25_

---

_Marked ready for review by @MichaReiser on 2025-07-09 11:25_

---

_Review requested from @carljm by @MichaReiser on 2025-07-09 11:25_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-09 11:25_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-09 11:25_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-09 11:25_

---

_Merged by @MichaReiser on 2025-07-09 11:28_

---

_Closed by @MichaReiser on 2025-07-09 11:28_

---

_Branch deleted on 2025-07-09 11:28_

---

_Comment by @github-actions[bot] on 2025-07-09 11:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-09 11:37_

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
