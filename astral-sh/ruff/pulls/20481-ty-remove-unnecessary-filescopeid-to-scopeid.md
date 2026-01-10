```yaml
number: 20481
title: "[ty] Remove unnecessary `FileScopeId` to `ScopeId` conversion"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/remove-unnecessary-scope-id-conversion
created_at: 2025-09-19T13:12:45Z
updated_at: 2025-09-20T11:28:45Z
url: https://github.com/astral-sh/ruff/pull/20481
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Remove unnecessary `FileScopeId` to `ScopeId` conversion

---

_Pull request opened by @MichaReiser on 2025-09-19 13:12_

I noticed this when debugging https://github.com/astral-sh/ty/issues/1111 because I saw many repeated `semantic_index` calls.

Going to `ScopeId` here is unnecessary (and only introduces more work). We can retrieve the `node` directly from `index.scope(id).node`


---

_Label `internal` added by @MichaReiser on 2025-09-19 13:12_

---

_Review requested from @carljm by @MichaReiser on 2025-09-19 13:12_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-09-19 13:12_

---

_Review requested from @sharkdp by @MichaReiser on 2025-09-19 13:12_

---

_Review requested from @dcreager by @MichaReiser on 2025-09-19 13:12_

---

_Label `internal` added by @MichaReiser on 2025-09-19 13:12_

---

_Comment by @github-actions[bot] on 2025-09-19 13:14_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-19 13:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/scope.rs`:32 on 2025-09-19 15:18_

I'd just delete this if it's now unused -- it seems pretty trivial? But no strong opinion

---

_@AlexWaygood approved on 2025-09-19 15:18_

---

_Label `ty` added by @AlexWaygood on 2025-09-19 15:49_

---

_Merged by @MichaReiser on 2025-09-20 11:20_

---

_Closed by @MichaReiser on 2025-09-20 11:20_

---

_Branch deleted on 2025-09-20 11:20_

---
