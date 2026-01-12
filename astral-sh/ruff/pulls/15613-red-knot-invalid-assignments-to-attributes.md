```yaml
number: 15613
title: "[red-knot] Invalid assignments to attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/attribute-stores
created_at: 2025-01-20T10:46:26Z
updated_at: 2025-01-23T09:20:27Z
url: https://github.com/astral-sh/ruff/pull/15613
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Invalid assignments to attributes

---

_@sharkdp_

## Summary

Raise "invalid-assignment" diagnostics for incorrect assignments to attributes, for example:

```py
class C:
    var: str = "a"

C.var = 1  # error: "Object of type `Literal[1]` is not assignable to `str`"
```

closes #15456 

## Test Plan

- Updated test assertions
- New test for assignments to module-attributes

---

_Label `red-knot` added by @sharkdp on 2025-01-20 10:46_

---

_Comment by @github-actions[bot] on 2025-01-20 10:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @sharkdp on 2025-01-20 12:15_

---

_Review requested from @carljm by @sharkdp on 2025-01-20 12:15_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-20 12:15_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-20 12:15_

---

_Converted to draft by @sharkdp on 2025-01-20 12:18_

---

_@sharkdp reviewed on 2025-01-20 12:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1991 on 2025-01-20 12:48_

I went through three or four different iterations of this API here. I'm still not satisfied. If we want to pass in the `assigned_ty` directly, we need to call `infer_standalone_expression` at the call site. But we are only allowed to do that if the `target` is not a `ast::Expr::Name(_)`, so we need to basically copy the whole `match` statement below to the call site. The duplication maybe not so bad, but I could some expression types are not yet handled here (see pre-existing TODO), and then we would need to remember to update it everywhere.

Maybe my struggle to incorporate this here is also a sign that I'm doing something wrong?

---

_@sharkdp reviewed on 2025-01-20 12:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2025-01-20 12:50_

Is it important that we return `Never` here? I wasn't able to construct a Python example that would get access to this type...?

---

_Marked ready for review by @sharkdp on 2025-01-20 12:50_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1991 on 2025-01-20 13:21_

I think it's fine. An altenrative, to avoid monomorphization is to have a `Target` enum with a `Name`, and another variant but naming kind of gets awkward.

What's the reason that we only infer the value's type for name? Is it because the inference for non-names happens elsewhere and it, therefore, avoids doubl-inference?

---

_@MichaReiser reviewed on 2025-01-20 13:21_

---

_@sharkdp reviewed on 2025-01-20 14:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:1991 on 2025-01-20 14:34_

> What's the reason that we only infer the value's type for name? Is it because the inference for non-names happens elsewhere and it, therefore, avoids doubl-inference?

We only call `infer_standalone_expression(value)` for *non*-`Name`s. For `Name`s, we call `infer_definition`, which (somewhere down the line) calls `infer_standalone_expression(value)` itself.

So yes, if we would call `infer_standalone_expression(value)` for all targets, we would get double-inference (results e.g. in duplicated diagnostics).

---

_@AlexWaygood reviewed on 2025-01-20 15:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2025-01-20 15:00_

It was changed to use `Never` in https://github.com/astral-sh/ruff/commit/b1ce8a39495918d1b7421317b8b8e63d0653d3df, which was prompted by https://github.com/astral-sh/ruff/pull/13981#issuecomment-2445472433

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:105 on 2025-01-21 12:06_

I wonder if this might be a slightly friendlier error message for users?

```suggestion
# error: [invalid-assignment] "Object of type `Literal[1]` is not assignable to attribute with public type `str`"
```

looks like a pre-existing issue, though, so feel free to ignore this for this PR

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:470 on 2025-01-21 12:07_

very nice!

---

_@AlexWaygood approved on 2025-01-21 12:09_

---

_@carljm reviewed on 2025-01-22 00:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2025-01-22 00:20_

You can't get access to this type in Python, the only reason we need a type for it is the "type pulling" test, which is a proxy for "what should we show if you hover over this expression in an IDE". `Never` is my best answer for that (I think it's a better answer than `None`), but I don't think it matters much.

---

_@sharkdp reviewed on 2025-01-22 08:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2025-01-22 08:11_

> You can't get access to this type in Python, the only reason we need a type for it is the "type pulling" test, which is a proxy for "what should we show if you hover over this expression in an IDE". `Never` is my best answer for that (I think it's a better answer than `None`), but I don't think it matters much.

Sorry, I should have phrased my question better: *given* that we "don't think it matters much", is it okay if we change the type of the `obj.attr` expression in an assignment like `obj.attr = value` from `Never` to the type that we would get for `obj.attr` in a `Load` context? I'm inclined to say that this is much more helpful for a user in the IDE, which is why I thought there must have been a reason why `Never` was chosen instead.

---

_@sharkdp reviewed on 2025-01-22 09:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:105 on 2025-01-22 09:41_

I'll open a follow up PR to discuss this.

---

_@sharkdp reviewed on 2025-01-22 09:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2025-01-22 09:42_

To get this merged, I'm going to assume that it's okay to do this (let me know if not!).

---

_Merged by @sharkdp on 2025-01-22 09:42_

---

_Closed by @sharkdp on 2025-01-22 09:42_

---

_Branch deleted on 2025-01-22 09:42_

---

_@carljm reviewed on 2025-01-22 15:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2025-01-22 15:44_

In a case like `obj.attr = value`, I think it's fine if hover on `obj.attr` shows the type of `value`. If it shows the type of `obj.attr` prior to the assignment, I think that's not good. (I haven't had a chance to fully review this PR yet so I don't know yet which it is.)

---

_@sharkdp reviewed on 2025-01-22 16:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2025-01-22 16:13_

> In a case like `obj.attr = value`, I think it's fine if hover on `obj.attr` shows the type of `value`. If it shows the type of `obj.attr` prior to the assignment, I think that's not good.

Okay. In that case, this will need another iteration (it's the latter).

---

_@carljm reviewed on 2025-01-22 16:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2025-01-22 16:15_

I just realized that until we add narrowing on assignments to attributes, there's no difference in the `obj.attr = value` case. But in the `obj = value`, or particularly the `obj: annotation = value` case, it could be a totally different type, and I don't think it's sensible for hover on that assignment target to show its type prior to the assignment.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3464 on 2025-01-22 16:23_

note for future: I suspect that we should allow member access to signal error conditions, rather than doing this special-casing based on the specific type out here in type inference

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2026 on 2025-01-22 16:35_

In future, if we add narrowing of attribute types, there could be a distinction between "infer the local type of this attribute expression" and "get the declared type of this attribute", and we would want the latter here, not the former. Which might also obsolete the change in this PR to the inferred type of `Store` attribute expressions. But for now, there is no difference, so it's convenient to just use `infer_attribute_expression` for both.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3433 on 2025-01-22 16:37_

But this change only applies to attribute expressions, so for now at least I think it's fine as-is.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:439 on 2025-01-22 16:39_

Should we also have a test for multi-level attribute assignment, either `mod.Class.attr = ...` or `Class.NestedClass.attr = ...`?

---

_@carljm reviewed on 2025-01-22 16:39_

Very nice! A few thoughts, but nothing that I think requires immediate follow-up.

---

_@sharkdp reviewed on 2025-01-23 09:20_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:439 on 2025-01-23 09:20_

https://github.com/astral-sh/ruff/pull/15684

---
