```yaml
number: 13223
title: "Add a few missing `#[return_ref]` attributes"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: micha/add-missing-return-ref
created_at: 2024-09-03T07:09:31Z
updated_at: 2024-09-03T07:23:30Z
url: https://github.com/astral-sh/ruff/pull/13223
synced_at: 2026-01-12T15:55:43Z
```

# Add a few missing `#[return_ref]` attributes

---

_@MichaReiser_

## Summary

Add a few missing `#[return_ref]` attributes to avoid cloning large structures.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-09-03 07:09_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-09-03 07:09_

---

_Label `performance` added by @MichaReiser on 2024-09-03 07:09_

---

_Label `red-knot` added by @MichaReiser on 2024-09-03 07:09_

---

_Merged by @MichaReiser on 2024-09-03 07:15_

---

_Closed by @MichaReiser on 2024-09-03 07:15_

---

_Branch deleted on 2024-09-03 07:15_

---

_Comment by @github-actions[bot] on 2024-09-03 07:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
