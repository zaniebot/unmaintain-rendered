```yaml
number: 12985
title: "Add the `testing` feature of `ruff_db` as a dev-dependency for `ruff_workspace`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: workspace-deps
created_at: 2024-08-19T10:16:36Z
updated_at: 2024-08-19T10:31:14Z
url: https://github.com/astral-sh/ruff/pull/12985
synced_at: 2026-01-12T15:55:42Z
```

# Add the `testing` feature of `ruff_db` as a dev-dependency for `ruff_workspace`

---

_@AlexWaygood_

Without this, `cargo test -p ruff_workspace` fails, as it relies on the `testing` feature without this being listed as an explicit requirement

---

_Review requested from @carljm by @AlexWaygood on 2024-08-19 10:16_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-19 10:16_

---

_Label `red-knot` added by @AlexWaygood on 2024-08-19 10:16_

---

_Merged by @AlexWaygood on 2024-08-19 10:22_

---

_Closed by @AlexWaygood on 2024-08-19 10:22_

---

_Branch deleted on 2024-08-19 10:22_

---

_Comment by @github-actions[bot] on 2024-08-19 10:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
