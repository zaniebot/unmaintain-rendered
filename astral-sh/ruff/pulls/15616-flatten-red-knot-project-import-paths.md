```yaml
number: 15616
title: "Flatten `red_knot_project` import paths"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/flatten-project-structure
created_at: 2025-01-20T13:05:19Z
updated_at: 2025-01-20T13:57:59Z
url: https://github.com/astral-sh/ruff/pull/15616
synced_at: 2026-01-12T15:55:51Z
```

# Flatten `red_knot_project` import paths

---

_@MichaReiser_

## Summary

This is a follow up to https://github.com/astral-sh/ruff/pull/15615

The repeated `project` in `red_knot_project::project` looks a bit silly. That's why this PR moves the project module one level up so that the import path becomes `red_knot_project::Project`. Having `Project` in the `lib.rs` seems appropriate, 
considering that the focus of this crate is the `Project`. 


This PR only moves code around. There are no functional changes.

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-01-20 13:05_

---

_Label `red-knot` added by @MichaReiser on 2025-01-20 13:05_

---

_Marked ready for review by @MichaReiser on 2025-01-20 13:12_

---

_Review requested from @carljm by @MichaReiser on 2025-01-20 13:12_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-20 13:12_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-20 13:12_

---

_@AlexWaygood approved on 2025-01-20 13:13_

---

_Comment by @github-actions[bot] on 2025-01-20 13:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @MichaReiser on 2025-01-20 13:57_

---

_Closed by @MichaReiser on 2025-01-20 13:57_

---

_Branch deleted on 2025-01-20 13:57_

---
