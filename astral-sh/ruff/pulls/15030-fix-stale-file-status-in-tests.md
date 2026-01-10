```yaml
number: 15030
title: Fix stale File status in tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fix-test-isolation
created_at: 2024-12-17T08:27:50Z
updated_at: 2024-12-17T11:45:39Z
url: https://github.com/astral-sh/ruff/pull/15030
synced_at: 2026-01-10T20:42:27Z
```

# Fix stale File status in tests

---

_Pull request opened by @MichaReiser on 2024-12-17 08:27_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15027

The `MemoryFileSystem::write_file` API automatically creates non-existing ancestor directoryes 
but we failed to update the status of the now created ancestor directories in the `Files` data structure. 


## Test Plan

Tested that the case in https://github.com/astral-sh/ruff/issues/15027 now passes regardless of whether the *Simple* case is commented out or not


---

_Review requested from @carljm by @MichaReiser on 2024-12-17 08:27_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-17 08:27_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-17 08:27_

---

_Label `red-knot` added by @MichaReiser on 2024-12-17 08:27_

---

_@MichaReiser reviewed on 2024-12-17 08:28_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:127 on 2024-12-17 08:28_

I don't think the definition id gives us much here and it resulted in very long tracing lines but I'll revert if this ends up being controverisal

---

_Comment by @github-actions[bot] on 2024-12-17 08:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp approved on 2024-12-17 08:57_

Thank you!

---

_Merged by @MichaReiser on 2024-12-17 11:45_

---

_Closed by @MichaReiser on 2024-12-17 11:45_

---

_Branch deleted on 2024-12-17 11:45_

---
