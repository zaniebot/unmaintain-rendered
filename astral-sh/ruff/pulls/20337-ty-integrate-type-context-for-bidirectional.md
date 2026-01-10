```yaml
number: 20337
title: "[ty] Integrate type context for bidirectional inference"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/bidirectional-inference
created_at: 2025-09-10T18:37:22Z
updated_at: 2025-09-15T10:04:48Z
url: https://github.com/astral-sh/ruff/pull/20337
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Integrate type context for bidirectional inference

---

_Pull request opened by @ibraheemdev on 2025-09-10 18:37_

## Summary

Adds the infrastructure necessary to perform bidirectional type inference (https://github.com/astral-sh/ty/issues/168) without any typing changes.

---

_Label `typing` added by @ibraheemdev on 2025-09-10 18:37_

---

_Label `ty` added by @ibraheemdev on 2025-09-10 18:37_

---

_Label `typing` removed by @ibraheemdev on 2025-09-10 18:37_

---

_Label `internal` added by @ibraheemdev on 2025-09-10 18:37_

---

_Comment by @github-actions[bot] on 2025-09-10 18:39_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-11 19:05:55.986136869 +0000
+++ new-output.txt	2025-09-11 19:05:58.979164413 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(b816)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(12a7a)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/3713cd7/src/function/execute.rs:213:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(12e7a)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-10 18:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@ibraheemdev reviewed on 2025-09-10 19:56_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer.rs`:260 on 2025-09-10 19:56_

We never actually call `infer_expression_type` with a non-empty `TypeContext` currently, I'm not sure if that's something that will change in the future?

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer.rs`:2181 on 2025-09-10 21:59_

The functions (and salsa queries) that now take a `TypeContext` are most often called with an empty one, but I'm not sure if we want to have separate functions or make the lack of a `TypeContext` explicit on every call.

---

_@ibraheemdev reviewed on 2025-09-10 21:59_

---

_Marked ready for review by @ibraheemdev on 2025-09-10 21:59_

---

_Review requested from @carljm by @ibraheemdev on 2025-09-10 21:59_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-09-10 21:59_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-09-10 21:59_

---

_Review requested from @dcreager by @ibraheemdev on 2025-09-10 21:59_

---

_@ibraheemdev reviewed on 2025-09-10 22:03_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer.rs`:6423 on 2025-09-10 22:03_

I left this comment in a few notable places where we now have a `TypeContext` that we could use for more precise inference.

---

_@ibraheemdev reviewed on 2025-09-10 22:04_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer.rs`:266 on 2025-09-10 22:04_

I moved the queries into private functions because the `InferExpression` supertype is mostly an internal implementation detail.

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer.rs`:296 on 2025-09-10 22:08_

We also discussed splitting the queries into variants with and without a `TypeContext` as a potential workaround, but this supertype is effectively a more convenient way of doing that (internally, Salsa essentially creates a query for each variant of the input enum). The performance impact of this PR seems to be very minor with this optimization (and without it, there was a noticeable 2-3% regression).

---

_@ibraheemdev reviewed on 2025-09-10 22:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:260 on 2025-09-11 00:27_

Hmm. This PR is great, and it's awesome that it has negligible performance impact. And now this question is making me wonder if we don't even need this and I mis-guided you last week? I was thinking the RHS of assignment statements was a standalone expression, but it only is for un-annotated assignment statements, not for annotated assignments. And it's only annotated assignments and call arguments where we currently intend to apply a type context, but neither of those are standalone expressions. So it seems like at least for the initial scope here, we only need to pass type contexts internally in a `TypeInferenceBuilder`?

Unless I'm missing something here (@dcreager?), maybe the thing to take from this PR is that if we ever need additional context on `infer_type_expression`, now we know it can be done without a major performance regression -- but for now we should put it on ice and focus on just the internal type-contexts we need to actually implement bidirectional inference within an annotated assignment and for call arguments. (This will still involve the need to accommodate potentially multiple inferred types for a single expression, in the case of call arguments to unions of callables or overloaded callables.)

---

_@carljm approved on 2025-09-11 00:28_

Nice! (But see inline comment -- maybe we shouldn't actually merge this at this point?)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:296 on 2025-09-11 01:51_

> this supertype is effectively a more convenient way of doing that (internally, Salsa essentially creates a query for each variant of the input enum).

That's a nice trick, I didn't realize that about how salsa handles the enum input

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:260 on 2025-09-11 01:54_

> Unless I'm missing something here

I don't see anything you're missing

---

_@dcreager reviewed on 2025-09-11 01:54_

---

_@sharkdp reviewed on 2025-09-11 07:13_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:260 on 2025-09-11 07:13_

> I was thinking the RHS of assignment statements was a standalone expression, but it only is for un-annotated assignment statements, not for annotated assignments.

Isn't the right hand side of something like `self.t: MyTypedDict = {…}` a standalone expression (see [here](https://github.com/astral-sh/ruff/blob/c6b92b918e4d350e4f8aed95a41985f80ca7dfc9/crates/ty_python_semantic/src/semantic_index/builder.rs#L1647) and [here](https://github.com/astral-sh/ruff/blob/8a0edf0da8342ae3b9fcf8d183fc289b52d64f86/crates/ty_python_semantic/src/types/infer.rs#L4629) and [here](https://github.com/astral-sh/ruff/blob/8a0edf0da8342ae3b9fcf8d183fc289b52d64f86/crates/ty_python_semantic/src/types/infer.rs#L4721))?

Maybe that's only needed for something like `self.MY_CONSTANT: Final = 17` where the annotation contains no type (only a qualifier) though? Which could mean that it's not important for purposes of bidirectional inference.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-09-11 10:55_

---

_@carljm reviewed on 2025-09-11 14:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:260 on 2025-09-11 14:19_

Aha, I _was_ missing something, should have looked more carefully last night, thanks @sharkdp. I found the `Final` case yesterday but we indeed don't need to apply any type context in that scenario (at least I don't think we currently have plans for a bare `Final` or `ClassVar` annotation to affect inference of the RHS). But that doesn't matter -- we now make annotated assignment RHS a standalone expression whenever it's inside a method, so as you point out, even `self.x: MyTypedDict = {...}` inside a method (or for that matter, `x: MyTypedDict = {...}` inside a method) will have a standalone expression on the RHS. So I think we do need this PR. And we should (at some point) pass a non-empty `TypeContext` through in those places in `infer_annotated_assignment_statement` and `infer_annotated_assignment_definition` that @sharkdp linked.

---

_@carljm reviewed on 2025-09-11 14:21_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:2181 on 2025-09-11 14:21_

I don't have strong feelings here -- there's maybe some advantage to making it explicit, and avoiding another axis on which to proliferate separate inference functions?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:260 on 2025-09-11 14:30_

It looks like already in this PR, we are (correctly) passing a non-empty `TypeContext` into `infer_maybe_standalone_expression` from `infer_annotated_assignment_statement` and `infer_annotated_assignment_definition`, which passes it through to `infer_standalone_expression_impl`, which passes it to `infer_expression_types`.

I think I mis-interpreted this question from @ibraheemdev entirely last night -- it's actually a narrow question specifically about this `infer_expression_type` wrapper function, not about the `infer_expression_types` Salsa query generally. I think the answer to that question is that I don't immediately see where we'll use `infer_expression_type` with a non-empty `TypeContext`, but it still seems reasonable that it should accept one for consistency.

Sorry for mis-reading things here and not reviewing more carefully here last night; shouldn't do reviews when short on time. But I think we are back to concluding that we should go ahead with this PR as-is, and hopefully some of the discussion above was useful context anyway 😆 

---

_@carljm reviewed on 2025-09-11 14:30_

---

_Merged by @ibraheemdev on 2025-09-11 19:19_

---

_Closed by @ibraheemdev on 2025-09-11 19:19_

---

_Branch deleted on 2025-09-11 19:19_

---

_@MichaReiser reviewed on 2025-09-15 10:04_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:296 on 2025-09-15 10:04_

This is a nice solution!

---
