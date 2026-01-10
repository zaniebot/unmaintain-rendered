```yaml
number: 20355
title: "[ty] Remove the `Constraints` trait"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/remove-constraint-trait
created_at: 2025-09-11T18:46:48Z
updated_at: 2025-09-12T00:55:30Z
url: https://github.com/astral-sh/ruff/pull/20355
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Remove the `Constraints` trait

---

_Pull request opened by @dcreager on 2025-09-11 18:46_

This PR removes the `Constraints` trait. We removed the `bool` implementation several weeks back, and are using `ConstraintSet` everywhere. There have been discussions about trying to include the reason for an assignability failure as part of the result, but that there are no concrete plans to do so soon, and it's not clear that we'll need the `Constraints` trait to do that. (We can ideally just update the `ConstraintSet` type directly.)

In the meantime, this just complicates the code for no good reason.

This PR is a pure refactoring, and contains no behavioral changes.

---

_Label `internal` added by @dcreager on 2025-09-11 18:46_

---

_Label `ty` added by @dcreager on 2025-09-11 18:46_

---

_Comment by @github-actions[bot] on 2025-09-11 18:48_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-11 18:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:119 on 2025-09-11 19:09_

These methods (and by extension, `from_bool`) didn't actually use their `db` parameter, so I've simplified the API a bit.  `from_bool` is now replaced with a `From<bool>` impl, and these two constructors are replaced with `ConstraintSet::from(true)` and `from(false)`.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:249 on 2025-09-11 19:11_

There's now an annoying difference here, where `union` takes in an owned `other`, while `intersect` takes in a reference. This is purely due to internal details about when we can/cannot reuse the existing storage. This didn't exist before, because clippy couldn't see through the trait to trip the "you can pass this by reference" lint. I decided to go ahead and accept clippy's recommendation. If folks prefer, I would be equally happy putting an `#[expect]` here so that both methods take in an owned `other`.

---

_@dcreager reviewed on 2025-09-11 19:13_

---

_Marked ready for review by @dcreager on 2025-09-11 19:13_

---

_Review requested from @carljm by @dcreager on 2025-09-11 19:13_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-11 19:13_

---

_Review requested from @sharkdp by @dcreager on 2025-09-11 19:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:46 on 2025-09-11 19:34_

```suggestion
//! NOTE: This module is currently in a transitional state. We've added the DNF [`ConstraintSet`]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:119 on 2025-09-11 19:35_

In terms of readability, I think I weakly prefer the old spelling, actually! `ConstraintSet::unsatisfiable()` seems clearer to me about what it means than `ConstraintSet::from(false)`, even if the constructor is strictly-speaking redundant with the new `From<bool>` implementation. Same for `ConstraintSet::always_satisfiable`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:249 on 2025-09-11 19:36_

I'm happy to genuflect to Clippy here!

---

_@AlexWaygood approved on 2025-09-11 19:38_

Nice!

---

_Review request for @carljm removed by @carljm on 2025-09-11 21:30_

---

_@carljm reviewed on 2025-09-11 21:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:119 on 2025-09-11 21:32_

If it's a weak preference, I'll put in a vote in favor of the new spelling (or at least, in favor of not taking time to reintroduce the old methods and updating the PR to use them everywhere.)

---

_@dcreager reviewed on 2025-09-11 22:58_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:119 on 2025-09-11 22:58_

> or at least, in favor of not taking time to reintroduce the old methods and updating the PR to use them everywhere

I did that change in a separate commit, so the cost of reverting it would be ~zero if that sways your vote

---

_@carljm reviewed on 2025-09-11 23:27_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:119 on 2025-09-11 23:27_

I don't feel strongly either, but I think the new spelling is pretty clear, and I like that it generalizes well to cases where we already have a boolean value, not literal `true` or `false` (e.g. `ConstraintSet::from_bool(relation.is_assignability)`). So it seems like we will need to have (and be able to read and understand the concept of) `ConstraintSet::from_bool` anyway, which makes me dis-inclined to pad the API with special-case methods for literal `true` and `false`.

---

_Merged by @dcreager on 2025-09-12 00:55_

---

_Closed by @dcreager on 2025-09-12 00:55_

---

_Branch deleted on 2025-09-12 00:55_

---
