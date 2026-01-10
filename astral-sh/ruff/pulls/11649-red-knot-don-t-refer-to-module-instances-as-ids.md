```yaml
number: 11649
title: "red-knot: Don't refer to `Module` instances as IDs"
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: module-rs-comments
created_at: 2024-05-31T19:31:39Z
updated_at: 2024-05-31T20:15:28Z
url: https://github.com/astral-sh/ruff/pull/11649
synced_at: 2026-01-10T21:56:00Z
```

# red-knot: Don't refer to `Module` instances as IDs

---

_Pull request opened by @AlexWaygood on 2024-05-31 19:31_

This PR is a followup to #11638.

`Module` instances are not IDs that can be used to lookup modules; in fact, they are a direct representation of the module. (The representation of a module internally wraps a unique ID, but that's just an implementation detail.)

This PR accordingly updates various comments and variable names in `module.rs` to remove various places where Module instances are referred to as "IDs".

---

_Review requested from @carljm by @AlexWaygood on 2024-05-31 19:31_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-05-31 19:31_

---

_Renamed from "red-knot: Don't refer to  instances as IDs" to "red-knot: Don't refer to `Module` instances as IDs" by @AlexWaygood on 2024-05-31 19:31_

---

_@MichaReiser approved on 2024-05-31 19:43_

---

_Comment by @github-actions[bot] on 2024-05-31 19:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @AlexWaygood on 2024-05-31 20:04_

---

_Closed by @AlexWaygood on 2024-05-31 20:04_

---

_Branch deleted on 2024-05-31 20:04_

---
