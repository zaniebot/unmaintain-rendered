```yaml
number: 19256
title: "[ty] Consolidate submodule resolving code between `types.rs` and `ide_support.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/resolve-submodule-method
created_at: 2025-07-10T11:05:38Z
updated_at: 2025-07-10T13:10:13Z
url: https://github.com/astral-sh/ruff/pull/19256
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Consolidate submodule resolving code between `types.rs` and `ide_support.rs`

---

_Pull request opened by @AlexWaygood on 2025-07-10 11:05_

## Summary

These two regions of code are doing the same thing; this PR factors them out into common methods that can be used in both places.

## Test Plan

Existing tests all pass. This is a pure refactor that shouldn't change functionality at all.


---

_Review requested from @carljm by @AlexWaygood on 2025-07-10 11:05_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-10 11:05_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-10 11:05_

---

_Label `internal` added by @AlexWaygood on 2025-07-10 11:05_

---

_Label `ty` added by @AlexWaygood on 2025-07-10 11:05_

---

_Comment by @github-actions[bot] on 2025-07-10 11:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser approved on 2025-07-10 11:49_

---

_Merged by @AlexWaygood on 2025-07-10 13:10_

---

_Closed by @AlexWaygood on 2025-07-10 13:10_

---

_Branch deleted on 2025-07-10 13:10_

---
