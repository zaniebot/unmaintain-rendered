```yaml
number: 20348
title: "[ty] Require that implementors of `Constraints` also implement `Debug`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/contraints-dbg
created_at: 2025-09-11T11:45:30Z
updated_at: 2025-09-11T13:34:42Z
url: https://github.com/astral-sh/ruff/pull/20348
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Require that implementors of `Constraints` also implement `Debug`

---

_Pull request opened by @AlexWaygood on 2025-09-11 11:45_

The debug representation isn't as useful as calling `.display(db)`, but it's still kind-of annoying when `dbg!()` calls don't compile locally due to the compiler not being able to guarantee that an object of type `impl Constraints` implements `Debug`

---

_Review requested from @carljm by @AlexWaygood on 2025-09-11 11:45_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-11 11:45_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-11 11:45_

---

_Label `internal` added by @AlexWaygood on 2025-09-11 11:45_

---

_Label `ty` added by @AlexWaygood on 2025-09-11 11:45_

---

_Comment by @github-actions[bot] on 2025-09-11 11:48_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-11 11:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@dcreager approved on 2025-09-11 12:51_

---

_Merged by @AlexWaygood on 2025-09-11 13:34_

---

_Closed by @AlexWaygood on 2025-09-11 13:34_

---

_Branch deleted on 2025-09-11 13:34_

---
