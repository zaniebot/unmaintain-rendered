```yaml
number: 20121
title: "[ty] Simplify materialization of specialized generics"
type: pull_request
state: merged
author: JelleZijlstra
labels:
  - ty
assignees: []
merged: true
base: main
head: matrel2
created_at: 2025-08-27T22:15:45Z
updated_at: 2025-08-29T13:09:26Z
url: https://github.com/astral-sh/ruff/pull/20121
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Simplify materialization of specialized generics

---

_Pull request opened by @JelleZijlstra on 2025-08-27 22:15_

This is a variant of #20076 that moves some complexity out of `apply_type_mapping_impl` in `generics.rs`. The tradeoff is that now every place that applies `TypeMapping::Specialization` must take care to call `.materialize()` afterwards. (A previous version of this didn't work because I had missed a spot where I had to call `.materialize()`.)

@carljm as asked in https://github.com/astral-sh/ruff/pull/20076#discussion_r2305385298 .


---

_Comment by @github-actions[bot] on 2025-08-27 22:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `ty` added by @AlexWaygood on 2025-08-27 22:18_

---

_Comment by @github-actions[bot] on 2025-08-27 22:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "[ty] Variant of #20076 (simpler materialization of specialized generics)" to "[ty] Simplify materialization of specialized generics" by @JelleZijlstra on 2025-08-28 14:09_

---

_Marked ready for review by @JelleZijlstra on 2025-08-28 14:40_

---

_Review requested from @carljm by @JelleZijlstra on 2025-08-28 14:41_

---

_Review requested from @AlexWaygood by @JelleZijlstra on 2025-08-28 14:41_

---

_Review requested from @sharkdp by @JelleZijlstra on 2025-08-28 14:41_

---

_Review requested from @dcreager by @JelleZijlstra on 2025-08-28 14:41_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-28 14:44_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class_base.rs`:290 on 2025-08-28 17:49_

Nice find!

---

_@carljm approved on 2025-08-28 18:01_

This seems like a win to me. The materialization kind is part of the specialization; it seems reasonable that `apply_specialization` methods would be responsible for handling it.

I think if we wanted to invest in better error-resilience here (if we find ourselves forgetting to apply the materialization in more cases), the approach would be to extract a Rust type that holds only the types part of a `Specialization`, and have `TypeMappling::Specialization` hold only that Rust type, so you'd have to extract that and the materialization from a `Specialization` in order to wrap it in a `TypeMapping`. This would make it impossible to ignore the materialization part accidentally.

---

_Merged by @carljm on 2025-08-28 18:35_

---

_Closed by @carljm on 2025-08-28 18:35_

---

_Branch deleted on 2025-08-29 13:09_

---
