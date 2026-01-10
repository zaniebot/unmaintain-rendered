```yaml
number: 20955
title: "[ty] Fix panic on recursive class definitions in a stub that use constrained type variables"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/pandas-panic-2
created_at: 2025-10-18T12:53:39Z
updated_at: 2025-10-18T13:02:56Z
url: https://github.com/astral-sh/ruff/pull/20955
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Fix panic on recursive class definitions in a stub that use constrained type variables

---

_Pull request opened by @AlexWaygood on 2025-10-18 12:53_

## Summary

Fixes https://github.com/astral-sh/ty/issues/1390

## Test Plan

Added mdtests that cause us to panic on `main`


---

_Label `bug` added by @AlexWaygood on 2025-10-18 12:53_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-18 12:53_

---

_Label `ty` added by @AlexWaygood on 2025-10-18 12:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-18 12:53_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-18 12:53_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:8327 on 2025-10-18 12:55_

Nit: Rename the cycle function to `lazy_bound_or_constraints_recover`?

---

_@MichaReiser approved on 2025-10-18 12:55_

---

_Comment by @github-actions[bot] on 2025-10-18 12:55_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@AlexWaygood reviewed on 2025-10-18 12:56_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8327 on 2025-10-18 12:56_

aw but then it's not a 1-line fix anymore 🤣

---

_Comment by @github-actions[bot] on 2025-10-18 12:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @AlexWaygood on 2025-10-18 13:02_

---

_Closed by @AlexWaygood on 2025-10-18 13:02_

---

_Branch deleted on 2025-10-18 13:02_

---
