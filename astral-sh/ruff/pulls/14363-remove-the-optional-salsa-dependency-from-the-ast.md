```yaml
number: 14363
title: Remove the optional salsa dependency from the AST crate
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/parser-remove-optional-salsa-dependency
created_at: 2024-11-15T16:40:50Z
updated_at: 2024-11-15T17:00:36Z
url: https://github.com/astral-sh/ruff/pull/14363
synced_at: 2026-01-12T15:55:47Z
```

# Remove the optional salsa dependency from the AST crate

---

_@MichaReiser_


## Summary

I added the `Lookup` implementation to `Name` to allow `Class` and `FunctionType` lookups with a `&str`.
What I didn't realize is that all lookups have a `&Name` reference. We never have a case where we just have a `&str`. 

That's why I'm removing the `Lookup` implementation because I prefer if `ruff-python-ast` doesn't depend on salsa, even if optionally.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-11-15 16:40_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-15 16:40_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-15 16:40_

---

_Label `red-knot` added by @MichaReiser on 2024-11-15 16:41_

---

_Merged by @MichaReiser on 2024-11-15 16:46_

---

_Closed by @MichaReiser on 2024-11-15 16:46_

---

_Branch deleted on 2024-11-15 16:46_

---

_Comment by @github-actions[bot] on 2024-11-15 17:00_

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
