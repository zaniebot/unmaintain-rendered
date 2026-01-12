```yaml
number: 18116
title: "[ty] Enable 'ansi' feature to fix compile error"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/ansi-feature
created_at: 2025-05-15T09:36:20Z
updated_at: 2025-05-15T09:49:24Z
url: https://github.com/astral-sh/ruff/pull/18116
synced_at: 2026-01-12T15:56:12Z
```

# [ty] Enable 'ansi' feature to fix compile error

---

_@MichaReiser_

## Summary

`cargo install --path crates/ty -f` failed because of the missing `ansi` feature (required for the `pretty` tracing formatter).

This PR enables the `ansi` feature.

## Test Plan

`cargo install --path crates/ty -f` now succeeds


---

_Review requested from @carljm by @MichaReiser on 2025-05-15 09:36_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-15 09:36_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-15 09:36_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-15 09:36_

---

_Label `internal` added by @MichaReiser on 2025-05-15 09:36_

---

_Label `isort` added by @MichaReiser on 2025-05-15 09:36_

---

_Label `ty` added by @MichaReiser on 2025-05-15 09:36_

---

_Label `isort` removed by @MichaReiser on 2025-05-15 09:36_

---

_Comment by @github-actions[bot] on 2025-05-15 09:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @MichaReiser on 2025-05-15 09:43_

---

_Closed by @MichaReiser on 2025-05-15 09:43_

---

_Branch deleted on 2025-05-15 09:43_

---

_Comment by @github-actions[bot] on 2025-05-15 09:49_

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
