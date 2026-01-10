```yaml
number: 18981
title: "[ty] Simplify `KnownClass::check_call()` and `KnownFunction::check_call()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/simplify-check-call
created_at: 2025-06-27T11:07:22Z
updated_at: 2025-06-27T11:23:30Z
url: https://github.com/astral-sh/ruff/pull/18981
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Simplify `KnownClass::check_call()` and `KnownFunction::check_call()`

---

_Pull request opened by @AlexWaygood on 2025-06-27 11:07_

Neither of these methods really need a mutable reference to a `Binding`. `KnownFunction::check_call()` doesnt need a reference to a binding at all; it only needs the parameter types

---

_Review requested from @carljm by @AlexWaygood on 2025-06-27 11:07_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-27 11:07_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-27 11:07_

---

_Label `internal` added by @AlexWaygood on 2025-06-27 11:07_

---

_Label `ty` added by @AlexWaygood on 2025-06-27 11:07_

---

_Comment by @github-actions[bot] on 2025-06-27 11:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @AlexWaygood on 2025-06-27 11:23_

---

_Closed by @AlexWaygood on 2025-06-27 11:23_

---

_Branch deleted on 2025-06-27 11:23_

---
