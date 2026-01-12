```yaml
number: 12529
title: Remove unnecessary clone in workspace API
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: workspace-remove-unnecessary-clone
created_at: 2024-07-26T13:26:20Z
updated_at: 2024-07-26T15:19:06Z
url: https://github.com/astral-sh/ruff/pull/12529
synced_at: 2026-01-12T15:55:41Z
```

# Remove unnecessary clone in workspace API

---

_@MichaReiser_

## Summary
I didn't know that the salsa setter removes the old value. 

We can use that to remove an Arc bump

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-07-26 13:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-26 13:26_

---

_Comment by @github-actions[bot] on 2024-07-26 13:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-07-26 15:19_

---

_Merged by @MichaReiser on 2024-07-26 15:19_

---

_Closed by @MichaReiser on 2024-07-26 15:19_

---

_Branch deleted on 2024-07-26 15:19_

---
