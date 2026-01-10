```yaml
number: 18450
title: "update to salsa that doesn't panic silently on cycles"
type: pull_request
state: merged
author: carljm
labels: []
assignees: []
merged: true
base: main
head: cjm/salsa-cycle-panic
created_at: 2025-06-04T00:59:48Z
updated_at: 2025-06-04T05:40:18Z
url: https://github.com/astral-sh/ruff/pull/18450
synced_at: 2026-01-10T18:45:04Z
```

# update to salsa that doesn't panic silently on cycles

---

_Pull request opened by @carljm on 2025-06-04 00:59_

## Summary

Update to Salsa with https://github.com/salsa-rs/salsa/pull/898, to restore debug information on query cycle panics.

## Test Plan

Checked a file known to fail with a query cycle panic and observed debug information (Rust backtrace, Salsa query trace) again.


---

_Review requested from @MichaReiser by @carljm on 2025-06-04 00:59_

---

_Comment by @github-actions[bot] on 2025-06-04 01:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-04 01:08_

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

_Merged by @MichaReiser on 2025-06-04 05:40_

---

_Closed by @MichaReiser on 2025-06-04 05:40_

---

_Branch deleted on 2025-06-04 05:40_

---
