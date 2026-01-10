```yaml
number: 12714
title: "Remove 'cli' module from `red_knot`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-remove-cli-module
created_at: 2024-08-06T12:06:20Z
updated_at: 2024-08-06T12:19:44Z
url: https://github.com/astral-sh/ruff/pull/12714
synced_at: 2026-01-10T21:47:02Z
```

# Remove 'cli' module from `red_knot`

---

_Pull request opened by @MichaReiser on 2024-08-06 12:06_

## Summary

The Red Knot crate is no longer a mixture of workspace and CLI. It's now THE CLI. That's why a `cli` sub-module is confusing. 

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-08-06 12:06_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-06 12:06_

---

_@AlexWaygood approved on 2024-08-06 12:07_

---

_Label `red-knot` added by @MichaReiser on 2024-08-06 12:08_

---

_Merged by @MichaReiser on 2024-08-06 12:10_

---

_Closed by @MichaReiser on 2024-08-06 12:10_

---

_Branch deleted on 2024-08-06 12:10_

---

_Comment by @github-actions[bot] on 2024-08-06 12:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
