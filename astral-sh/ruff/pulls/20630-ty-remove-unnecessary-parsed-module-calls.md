```yaml
number: 20630
title: "[ty] Remove unnecessary `parsed_module()` calls"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/unnecessary-node-calls
created_at: 2025-09-29T14:25:53Z
updated_at: 2025-09-29T15:05:14Z
url: https://github.com/astral-sh/ruff/pull/20630
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Remove unnecessary `parsed_module()` calls

---

_Pull request opened by @AlexWaygood on 2025-09-29 14:25_

## Summary

I noticed this while reviewing #20517. It's unnecessary for `bind_typevar` and `enclosing_generic_context` to depend directly on a module's AST.

## Test Plan

Existing tests


---

_Review requested from @carljm by @AlexWaygood on 2025-09-29 14:25_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-29 14:25_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-29 14:25_

---

_Label `internal` added by @AlexWaygood on 2025-09-29 14:25_

---

_Label `ty` added by @AlexWaygood on 2025-09-29 14:25_

---

_Comment by @github-actions[bot] on 2025-09-29 14:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-29 14:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser approved on 2025-09-29 15:00_

---

_Merged by @AlexWaygood on 2025-09-29 15:05_

---

_Closed by @AlexWaygood on 2025-09-29 15:05_

---

_Branch deleted on 2025-09-29 15:05_

---
