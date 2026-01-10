```yaml
number: 14106
title: "[red-knot] Store starred-expression annotation types"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/store-type-for-starred-expression-annotations
created_at: 2024-11-05T12:30:47Z
updated_at: 2024-11-05T19:25:48Z
url: https://github.com/astral-sh/ruff/pull/14106
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Store starred-expression annotation types

---

_Pull request opened by @sharkdp on 2024-11-05 12:30_

## Summary

- Store the expression type for annotations that are starred expressions (see [discussion here](https://github.com/astral-sh/ruff/pull/14091#discussion_r1828332857))
- Use `self.store_expression_type(…)` consistently throughout, as it makes sure that no double-insertion errors occur.

closes #14115

## Test Plan

Added an invalid-syntax example to the corpus which leads to a panic on `main`. Also added a Markdown test with a valid-syntax example that would lead to a panic once we implement function parameter inference.

---

_Label `red-knot` added by @sharkdp on 2024-11-05 12:30_

---

_Review requested from @carljm by @sharkdp on 2024-11-05 12:30_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-05 12:30_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-05 12:30_

---

_@sharkdp reviewed on 2024-11-05 12:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/starred_expressions.md`:10 on 2024-11-05 12:33_

As far as I understand, vararg annotations are the only place where starred expressions can appear in annotations at the "outermost" level (we can have things like `tuple[*Ts]`, but not a "naked" `*Ts`).

Please note that this is not a proper regression test so far, but hopefully once we have function parameter type inference. This test "succeeds" (does not panic) on `main` as well.

---

_@AlexWaygood reviewed on 2024-11-05 12:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/starred_expressions.md`:10 on 2024-11-05 12:34_

Yes, I _think_ that's correct!

---

_@sharkdp reviewed on 2024-11-05 12:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3780 on 2024-11-05 12:41_

This is probably the easiest solution to the *"we don't want to risk forgetting to store the annotation type in any of the non-`infer_type_expression` branches above"* problem that I could think of. The way this works is by having a new function *infer_**and_store**_type_expression*, while the previous *infer_type_expression* only returns the type. If you feel that this introduces other risks (calling the wrong function elsewhere), let me know.

Another solution (also not very elegant) would be to explicitly state whether or not the expression type should be stored, as a kind of `mode` parameter to the `infer_type_expression` function (which would either be `InferOnly` or `InferAndStore`, or similar).

A third option would be to simply ignore double-insertions.

---

_Comment by @github-actions[bot] on 2024-11-05 12:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/annotations/starred_expressions.md`:6 on 2024-11-05 13:52_

nit: this should future-proof the test a little bit. Strictly speaking we should complain about an import of `typing.TypeVarTuple` with our default Python version, since `typing.TypeVarTuple` was added in Python 3.11. `typing_extensions.TypeVarTuple`, on the other hand, is available on all Python versions.

```suggestion
from typing_extensions import TypeVarTuple
```

---

_@AlexWaygood approved on 2024-11-05 13:56_

This LGTM, but I'd feel more confident if Micha or Carl could also take a look here

---

_@MichaReiser reviewed on 2024-11-05 17:01_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3780 on 2024-11-05 17:01_

> A third option would be to simply ignore double-insertions.

We could disable this in release build but the assertion is intentional to catch double-inference early. 

---

_@MichaReiser approved on 2024-11-05 17:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/starred_expressions.md`:10 on 2024-11-05 17:31_

I also think it's correct, but (as I mentioned in Discord) we can't restrict our test cases to valid syntax, because we also need to support error recovery. So when we are adding tests for something that currently causes us to panic, it's fine (even good!) to write a test that is semantically nonsense, or even emits syntax errors, but demonstrates that we don't panic.

It would be helpful if mdtest gained the feature to assert on syntax errors, instead of just immediately failing if there are any.

(To be clear, I think this test is a good addition regardless, and I don't even feel strongly about adding the invalid-syntax test in this PR. Just observing that it is possible, and useful, to do so.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3780 on 2024-11-05 17:35_

I like the option you've chosen here, but I think we should flip the naming, so that `infer_type_expression` and `infer_expression` (the "default" options someone is likely to reach for, and whose naming strongly suggests parallel behavior) both _do_ store the expression type.

I realize the current naming you have here is sensible because storing is doing an extra thing, so it makes sense for that to be reflected in the name. But I think the need that `infer_annotation_expression` has to infer a type expression without storing it is the unusual case, and it's important that the method we should almost always prefer be the one with the shorter/simpler name.

So I would suggest that `infer_expression_type` does store, and we have `infer_expression_type_impl` or `infer_expression_type_no_store` (or perhaps you have a better idea for the awkward naming here) for the unusual case.

---

_@carljm approved on 2024-11-05 17:36_

Thank you!!

---

_@sharkdp reviewed on 2024-11-05 19:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3780 on 2024-11-05 19:03_

> So I would suggest that `infer_expression_type` does store, and we have `infer_expression_type_impl` or `infer_expression_type_no_store`

Done

---

_@sharkdp reviewed on 2024-11-05 19:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/annotations/starred_expressions.md`:10 on 2024-11-05 19:04_

> I also think it's correct, but (as I mentioned in Discord) we can't restrict our test cases to valid syntax, because we also need to support error recovery. So when we are adding tests for something that currently causes us to panic, it's fine (even good!) to write a test that is semantically nonsense, or even emits syntax errors, but demonstrates that we don't panic.

That makes sense. I added an invalid-syntax example to the corpus that currently leads to a panic on main.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3786 on 2024-11-05 19:08_

Ugh I'm sorry, this was my carelessness in my review comment, but this should really be `infer_type_expression_no_store`, not `infer_expression_type_no_store`

---

_@carljm approved on 2024-11-05 19:08_

---

_@sharkdp reviewed on 2024-11-05 19:09_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3786 on 2024-11-05 19:09_

I was careless — sorry. Didn't even look at the diff again :see_no_evil: 

---

_Merged by @sharkdp on 2024-11-05 19:25_

---

_Closed by @sharkdp on 2024-11-05 19:25_

---

_Branch deleted on 2024-11-05 19:25_

---
