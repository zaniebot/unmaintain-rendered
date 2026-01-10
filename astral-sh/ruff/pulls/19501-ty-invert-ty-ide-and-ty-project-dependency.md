```yaml
number: 19501
title: "[ty] Invert `ty_ide` and `ty_project` dependency"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/invert-ty-ide-and-project-dependency
created_at: 2025-07-23T07:15:28Z
updated_at: 2025-07-23T07:44:47Z
url: https://github.com/astral-sh/ruff/pull/19501
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Invert `ty_ide` and `ty_project` dependency

---

_Pull request opened by @MichaReiser on 2025-07-23 07:15_

## Summary

This PR inverts the `ty_project` -> `ty_ide` dependency to `ty_ide` -> `ty_project`.  

The reason why `ty_project` dependet on `ty_ide` was so that `ty_ide` can have its own `Db` trait. 
I currently don't forsee any IDE specific methods that `Db` would need to implement that can't be on the `ty_project::Db`. 

Inverting the dependency has the advantage that `ty_ide` can query `db.project()` which seems fairly important (e.g. to get all files)

This helps with https://github.com/astral-sh/ruff/pull/19475#discussion_r2224648880

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-07-23 07:15_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-23 07:15_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-23 07:15_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-23 07:15_

---

_Label `internal` added by @MichaReiser on 2025-07-23 07:15_

---

_Label `ty` added by @MichaReiser on 2025-07-23 07:15_

---

_Comment by @github-actions[bot] on 2025-07-23 07:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-07-23 07:21_

Nice!

---

_Comment by @github-actions[bot] on 2025-07-23 07:26_

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

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-23 07:28_

---

_Merged by @MichaReiser on 2025-07-23 07:37_

---

_Closed by @MichaReiser on 2025-07-23 07:37_

---

_Branch deleted on 2025-07-23 07:37_

---
