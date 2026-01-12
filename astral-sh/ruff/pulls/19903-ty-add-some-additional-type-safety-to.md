```yaml
number: 19903
title: "[ty] Add some additional type safety to `CycleDetector`"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/pair-visitor-tag
created_at: 2025-08-13T20:33:10Z
updated_at: 2025-08-13T21:32:37Z
url: https://github.com/astral-sh/ruff/pull/19903
synced_at: 2026-01-12T15:56:50Z
```

# [ty] Add some additional type safety to `CycleDetector`

---

_@dcreager_

This PR adds a type tag to the `CycleDetector` visitor (and its aliases).

There are some places where we implement e.g. an equivalence check by making a disjointness check. Both `is_equivalent_to` and `is_disjoint_from` use a `PairVisitor` to handle cycles, but they should not use the same visitor. I was finding it tedious to remember when it was appropriate to pass on a visitor and when not to. This adds a `PhantomData` type tag to ensure that we can't pass on one method's visitor to a different method.

For `has_relation` and `apply_type_mapping`, we have an existing type that we can use as the tag. For the other methods, I've added empty structs (`Normalized`, `IsDisjointFrom`, `IsEquivalentTo`) to use as tags.

---

_Label `internal` added by @dcreager on 2025-08-13 20:33_

---

_Review requested from @carljm by @dcreager on 2025-08-13 20:33_

---

_Review requested from @AlexWaygood by @dcreager on 2025-08-13 20:33_

---

_Review requested from @sharkdp by @dcreager on 2025-08-13 20:33_

---

_Label `ty` added by @dcreager on 2025-08-13 20:33_

---

_Comment by @github-actions[bot] on 2025-08-13 20:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-13 21:23:19.455742809 +0000
+++ new-output.txt	2025-08-13 21:23:19.524743390 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(c4bf)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(413ae)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-13 20:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@carljm approved on 2025-08-13 20:40_

This is great, thank you!

As someone who has added a lot of `visitor` arguments to methods, and will be adding a lot more, I think I'd really appreciate some type aliases here, so I can say e.g. `visitor: &NormalizeVisitor` instead of `visitor: &TypeTransformer<'db, Normalized>`. But not a blocker.

---

_Comment by @dcreager on 2025-08-13 21:21_

> As someone who has added a lot of `visitor` arguments to methods, and will be adding a lot more, I think I'd really appreciate some type aliases here, so I can say e.g. `visitor: &NormalizeVisitor` instead of `visitor: &TypeTransformer<'db, Normalized>`. But not a blocker.

Good idea! Done

---

_Merged by @dcreager on 2025-08-13 21:32_

---

_Closed by @dcreager on 2025-08-13 21:32_

---

_Branch deleted on 2025-08-13 21:32_

---
