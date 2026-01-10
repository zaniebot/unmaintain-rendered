```yaml
number: 16469
title: "[red-knot] Support unpacking `with` target"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - ty
assignees: []
merged: true
base: main
head: unpack-with-stmt
created_at: 2025-03-03T06:27:53Z
updated_at: 2025-03-08T14:58:06Z
url: https://github.com/astral-sh/ruff/pull/16469
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Support unpacking `with` target

---

_Pull request opened by @ericmarkmartin on 2025-03-03 06:27_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves astral-sh/ruff#16365

Add support for unpacking `with` statement targets.

## Test Plan

<!-- How was it tested? -->
Added some test cases, alike the ones added by astral-sh/ruff#15058.


---

_@ericmarkmartin reviewed on 2025-03-03 06:32_

---

_Review comment by @ericmarkmartin on `crates/red_knot_python_semantic/src/types/infer.rs`:1574 on 2025-03-03 06:32_

I changed this because tests were panicking when using `infer_expression` in the else branch. Not sure I totally understand what happened here, but probably had to do with my changing the usage of `infer_context_expression` such that types are being inferred earlier?

---

_@ericmarkmartin reviewed on 2025-03-03 06:33_

---

_Review comment by @ericmarkmartin on `crates/red_knot_python_semantic/src/types/unpacker.rs`:66 on 2025-03-03 06:33_

I almost missed this because it's not a match and the compiler didn't catch me. Would folks be okay with my refactoring it?

---

_Label `red-knot` added by @AlexWaygood on 2025-03-03 12:35_

---

_@ericmarkmartin reviewed on 2025-03-05 03:58_

---

_Review comment by @ericmarkmartin on `crates/red_knot_python_semantic/src/types.rs`:3076 on 2025-03-05 03:58_

very small nit: we should probably be consistent with respect to "doesn't" vs "does not"

---

_Marked ready for review by @ericmarkmartin on 2025-03-05 03:59_

---

_Review requested from @carljm by @ericmarkmartin on 2025-03-05 03:59_

---

_Review requested from @MichaReiser by @ericmarkmartin on 2025-03-05 03:59_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-03-05 03:59_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-03-05 03:59_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1313 on 2025-03-05 07:36_

Nit: This code seems to be the same as for `for` targets with the exception of `UnpackValue::ContextManager`, `AttributeAssignment::ContextManager`. Would it be possible to reuse some more code by having a shared method that takes an `UnpackKind` argument that then constructs the `UnpackValue`, the `AttributeAssignment`

---

_@MichaReiser reviewed on 2025-03-05 07:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:361 on 2025-03-06 00:05_

We should also have a test similar to this, but with code like `with ContextManager() as (self.x, self.y):`. I think this would catch a change we are missing in the `ast::Expr::Attribute` branch of `SemanticIndexBuilder::visit_expr`, where we match on `Assign` and `For` unpacks, but also need to match on `With`.

Bonus points if we can find a way to make this mistake harder to make in future, e.g. with an `UnpackKind` enum that that code matches on.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1313 on 2025-03-06 00:06_

`Assign` has quite similar code as well, they should probably all three share a helper.

At the very least we should extract repetition around creation of the `Unpack`, things like the unsafe node-ref creation and the `countme::Count::default()` feel like details/boilerplate that don't belong at this level of abstraction and shouldn't be repeated for each node kind.

That said, I'd also be open to landing this PR with some duplication if we get all the behavior correct, and then doing the refactor as a separate PR for ease of review.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1289 on 2025-03-06 00:09_

This should really have a comment `// SAFETY: the node `optional_vars` is part of the `self.module` tree` or similar. But so should the same code in the `For` and `Assign` versions.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1300 on 2025-03-06 00:13_

Small preference for going with arbitrary `true` over arbitrary `false` here, since it is technically still the first (of one).

But this change would also apply to the same code in `For` and `Assign`, so should be made in whichever PR we refactor for better code reuse.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:204 on 2025-03-06 00:14_

Is this comment obsolete? There's only one Option here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:3076 on 2025-03-06 00:30_

I would go with "does not"

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2852 on 2025-03-06 00:33_

We should be quite careful about methods like these, because it means the path of least resistance for a caller is to ignore errors entirely. If we provide this, it should have a warning doc comment, like `Type::bool` and `Type::iterate` have.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2861 on 2025-03-06 00:36_

Doc comment for this method wouldn't hurt; similar to `try_iterate`, `try_bool`, etc.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:617 on 2025-03-06 00:48_

How about a test where evaluating the context_expr itself emits a diagnostic?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1574 on 2025-03-06 00:52_

I suspect it's related to the added support for `self.x` as a with statement target, meaning we are now inferring context managers with attribute targets as standalone expressions, and that leads to double inference of the same expression(s) if we don't always go through `infer_standalone_expression`.

I think this change is correct, given that the semantic index builder always creates a standalone expression for every `with` statement.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1271 on 2025-03-06 00:53_

This line looks redundant with line 1274? We shouldn't add `context_expr` as a standalone expression _twice_, and we shouldn't need to do it at all if there are no `optional_vars` (meaning we aren't going to create any Definition or register any attribute assignment).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/unpacker.rs`:66 on 2025-03-06 00:54_

Totally.

---

_@carljm reviewed on 2025-03-06 00:55_

Looks great, very impressive first PR!

I didn't get a chance to explore the (lack of) duplicated diagnostics that we discussed on Discord, will try to look at that later.

---

_Comment by @carljm on 2025-03-06 06:14_

Ok, I looked into the diagnostics situation, and it is going to need some attention here. A bit of background will be needed to understand the issue.

We have type inference queries at several granularities: expression (but only some "standalone" expressions, so we don't create too many Salsa ingredients), definition (that is, a value assigned to some name), and scope. We use the definition granularity queries to do lazy type inference of names from scopes we aren't actually checking (e.g. imported libraries). When we are actually checking a file, we run the scope-level inference queries for every scope in the file, and it is those queries where we aggregate and emit diagnostics.

So the intent when we run scope-level inference is that as we walk the AST, anywhere we hit a node that is a definition of a name, we use `self.infer_definition` to find that Definition by key, query its types, and then merge those types and diagnostics into the scope-level `TypeInference`. What this achieves is that we never double-infer the same types (even if we first lazily query the type of a name from a scope, and then later do full scope type inference on that scope), because we always use the (Salsa-cached) `infer_definition_types` query whenever we hit a definition.

What is currently happening in this PR is that when we find an unpacked with statement in scope-level inference, we don't eagerly run `infer_definition_types` for the definitions of the unpack-target names at all. This is not good, because it means we are implicitly depending on a later load of one of those names to actually run `infer_definition_types`, go through the Unpacker, and catch any unpack errors.
 
We should add a test like this to demonstrate this:

```py
class ContextManager:
    def __enter__(self) -> tuple[int, str]:
        return (1, "a")

    def __exit__(self, exc_type, exc_value, traceback) -> None:
        pass

with ContextManager() as (a, b, c):
    pass
```

This should emit an error about trying to unpack two elements into three, but currently in this PR it does not.

The fix is that this PR needs to update `TypeInferenceBuilder::infer_with_statement`, and currently doesn't; there's even a relevant TODO statement there. In the case where the target is not a simple Name, we are currently just inferring the type of the context expression (just once! which is why you currently can't make duplicate diagnostics happen, regardless of `first`) and then we visit the optional-vars, and nothing in that visit will ever look up any of the definitions or run `self.infer_definition()` on them.

The _right_ fix for this involves fixing https://github.com/astral-sh/ty/issues/185, since this duplicate-definition thing is already broken for `for` statements, as we discovered yesterday. But that fix is not going to be simple, and I don't think we should try to fix it in this PR.

Instead, I think the goal for this PR should be to emulate the current handling of `for` statements, and be just as broken as `for` in this regard, and then the fix for https://github.com/astral-sh/ty/issues/185 will fix them both in the same way. That will mean, in this PR, updating `TypeInferenceBuilder::infer_with_statement` to look a lot more like `TypeInferenceBuilder::infer_for_statement`, and use `TypeInferenceBuilder::infer_target`.

Hopefully that all made sense! If not, feel free to ask questions, here or in Discord. Or if it doesn't make sense and you don't have time to wrap your head around it and update the PR, that's totally fine too! Just let me know and one of us can pick it up.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/attribute_assignment.rs`:27 on 2025-03-07 08:24_

nit: "right-hand side" is a bit confusing but I understand that it's how assignment statements are usually viewed. Maybe we can reword it as "... where the (value|expression) to be assigned is a context-manager ..."

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1988 on 2025-03-07 08:26_

Just for visibility as stated in Discord and it doesn't need to be done in this PR but we could possibly combine `first` and `unpack` into a `Option<(UnpackPosition, Unpack<'a>)>` where the position could be either `First` or `Other`. I'm not sure what the fallout of this change would be but that would at least solve the arbitrary boolean problem when the target is a name node.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:3034 on 2025-03-07 08:47_

Can we use verbose name like `enter_return_type`? It's easier to read and can help future readers understand easily about this field.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:3083 on 2025-03-07 08:49_

nit: I think I'd prefer to make this a closure inside `report_diagnostic` as I think that's the only place where it's being utilized.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1595 on 2025-03-07 08:53_

I think this TODO is resolved, no? The `infer_target` call will take care of inferring the targets involved in an unpacking.

---

_@dhruvmanila reviewed on 2025-03-07 09:06_

---

_@ericmarkmartin reviewed on 2025-03-07 23:03_

---

_Review comment by @ericmarkmartin on `crates/red_knot_python_semantic/src/types.rs`:3083 on 2025-03-07 23:03_

I think that's true right now, but we could probably use it elsewhere, right?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/unpacking.md`:708 on 2025-03-08 02:32_

This is failing now because a PR that recently landed in main branch changed our policy on how we infer types on a failed unpacking: https://github.com/astral-sh/ruff/issues/15199
```suggestion
    reveal_type(a)  # revealed: Unknown
    reveal_type(b)  # revealed: Unknown
```

---

_@carljm approved on 2025-03-08 02:32_

Looks great, thank you! Just one failing test with a trivial fix, so I'll push that and then merge.

---

_Merged by @carljm on 2025-03-08 02:36_

---

_Closed by @carljm on 2025-03-08 02:36_

---

_Comment by @dhruvmanila on 2025-03-08 04:00_

@ericmarkmartin Awesome work! This is a really great first contribution. Thank you for taking this on and also surfacing an important issue related to the unpacking diagnostics.

---

_Branch deleted on 2025-03-08 14:58_

---
