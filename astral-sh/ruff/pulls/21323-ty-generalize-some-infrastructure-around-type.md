```yaml
number: 21323
title: "[ty] Generalize some infrastructure around type visitors"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/generalize-visitor
created_at: 2025-11-07T18:03:55Z
updated_at: 2025-11-07T21:26:32Z
url: https://github.com/astral-sh/ruff/pull/21323
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Generalize some infrastructure around type visitors

---

_Pull request opened by @AlexWaygood on 2025-11-07 18:03_

We have lots of `TypeVisitor`s that end up having very similar `visit_type` implementations. This PR consolidates some of the code for these so that there's less repetition and duplication.

---

_Label `internal` added by @AlexWaygood on 2025-11-07 18:03_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-07 18:03_

---

_Label `ty` added by @AlexWaygood on 2025-11-07 18:03_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-07 18:03_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-07 18:03_

---

_Label `internal` added by @AlexWaygood on 2025-11-07 18:03_

---

_Label `ty` added by @AlexWaygood on 2025-11-07 18:03_

---

_Comment by @github-actions[bot] on 2025-11-07 18:06_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-07 18:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-11-07 21:19_

---

_Merged by @AlexWaygood on 2025-11-07 21:26_

---

_Closed by @AlexWaygood on 2025-11-07 21:26_

---

_Branch deleted on 2025-11-07 21:26_

---
