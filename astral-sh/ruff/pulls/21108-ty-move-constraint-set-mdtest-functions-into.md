```yaml
number: 21108
title: "[ty] Move constraint set mdtest functions into `ConstraintSet` class"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/constraint-set-api
created_at: 2025-10-28T14:11:45Z
updated_at: 2025-10-29T12:06:30Z
url: https://github.com/astral-sh/ruff/pull/21108
synced_at: 2026-01-12T15:57:16Z
```

# [ty] Move constraint set mdtest functions into `ConstraintSet` class

---

_@dcreager_

We have several functions in `ty_extensions` for testing our constraint set implementation. This PR refactors those functions so that they are all methods of the `ConstraintSet` class, rather than being standalone top-level functions. :tophat: to @sharkdp for pointing out that `KnownBoundMethod` gives us what we need to implement that!

---

_Review requested from @carljm by @dcreager on 2025-10-28 14:11_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-28 14:11_

---

_Review requested from @sharkdp by @dcreager on 2025-10-28 14:11_

---

_Label `internal` added by @dcreager on 2025-10-28 14:11_

---

_Review requested from @MichaReiser by @dcreager on 2025-10-28 14:11_

---

_Label `ty` added by @dcreager on 2025-10-28 14:11_

---

_Renamed from "[ty] Move constraint set extension functions into `ConstraintSet` class" to "[ty] Move constraint set mdtest functions into `ConstraintSet` class" by @dcreager on 2025-10-28 14:12_

---

_Comment by @github-actions[bot] on 2025-10-28 14:15_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-28 14:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:26 on 2025-10-28 17:46_

I find `always` so much easier to understand. Thanks for making this change.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:4122 on 2025-10-28 17:47_

I didn't review this part closely as it mostly seems about moving code around. I hope that's okay

---

_@MichaReiser approved on 2025-10-28 17:48_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4122 on 2025-10-28 17:56_

Yep, a bunch of things that were `KnownFunction`s are now `KnownBoundMethod`s.

---

_@dcreager reviewed on 2025-10-28 17:56_

---

_Merged by @dcreager on 2025-10-28 18:32_

---

_Closed by @dcreager on 2025-10-28 18:32_

---

_Branch deleted on 2025-10-28 18:32_

---

_Comment by @sharkdp on 2025-10-29 12:06_

This new API/DSL looks cool!

---
