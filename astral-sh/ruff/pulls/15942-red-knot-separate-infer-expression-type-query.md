```yaml
number: 15942
title: "[red-knot] Separate 'infer_expression_type' query"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
base: main
head: david/make-infer_expression_type-a-separate-query
created_at: 2025-02-04T15:46:56Z
updated_at: 2025-05-07T15:22:16Z
url: https://github.com/astral-sh/ruff/pull/15942
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Separate 'infer_expression_type' query

---

_Pull request opened by @sharkdp on 2025-02-04 15:46_

## Summary

As a follow-up to [the discussion here](https://github.com/astral-sh/ruff/pull/15811#discussion_r1939617368), this changeset adds a new salsa query
```rs
fn infer_expression_type<'db>(db: &'db dyn Db, expression: Expression<'db>) -> Type<'db>
```
which is similar to `infer_expression_types` (plural), but returns the type of the expression directly instead of returning a `TypeInference` object. We can use this in a few places. Notably, it can't be used here:

https://github.com/astral-sh/ruff/blob/9d83e76a3b57bbd8788535cd968376587fb3ec38/crates/red_knot_python_semantic/src/types/narrow.rs#L500-L504

because that uses `self.scope()`, not `cls.scope()`.

## Test Plan

—

---

_Label `red-knot` added by @sharkdp on 2025-02-04 15:46_

---

_Review requested from @carljm by @sharkdp on 2025-02-04 15:46_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-04 15:46_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-04 15:46_

---

_@sharkdp reviewed on 2025-02-04 15:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/unpacker.rs`:46 on 2025-02-04 15:52_

I just noticed that this is not semantically equivalent (uses `self.scope`, not `value.expression().scope()`). Will check if that's okay.

---

_@AlexWaygood approved on 2025-02-04 15:52_

Lovely!

---

_@sharkdp reviewed on 2025-02-04 15:55_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/unpacker.rs`:46 on 2025-02-04 15:55_

It seems to me that this should be the same? But someone familiar with this code should probably double-check.

I added a
```rs
assert_eq!(value.expression().scope(self.db()), self.scope);
```
assertion locally and all tests passed.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/unpacker.rs`:46 on 2025-02-04 15:56_

@dhruvmanila probably has the most context on this module?

---

_@AlexWaygood reviewed on 2025-02-04 15:56_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:199 on 2025-02-04 17:36_

We may want to add a note that it isn't necessary to use this query in `TypeInferenceBuilder` or anywhere else where it's known that the `expression` belongs to the current file. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/unpacker.rs`:46 on 2025-02-04 17:37_

I don't think it's necessary to use `infer_expression_type` here because `value` always belongs to the same file (`Unpacker` never does any cross-module type inference).



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:394 on 2025-02-04 17:39_

I'm not 100% if we should use `infer_expression_type` here. We should only use it if `analyze_single` can be reached from another module than where `test_expr` is defined.

---

_@MichaReiser reviewed on 2025-02-04 17:41_

Whether we want to use `infer_expression_type` or not is slightly more subtle. The way you changed it in this PR isn't incorrect but it means that we pay for an extra salsa queries even in cases where the isolation isn't necessary. 

We should only use `infer_expression_type` if it isn't guaranteed that `expression` belongs to the same file as the enclosing query. This, for example, isn't the case in the `TypeInferenceBuilder` or `Unpacker` where all ast nodes are guaranteed to be from the same file. We do want the isolation in `Type::own_member` because there's no guarantee from which file (if any) the method is called.

---

_@AlexWaygood reviewed on 2025-02-04 17:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:394 on 2025-02-04 17:43_

Hmm, the new API is much more convenient and ergonomic. If we expect it to be slower in many cases, it feels like we're adding a footgun (making something that we should only do in very specific cases easy to do in all cases)

---

_@MichaReiser reviewed on 2025-02-04 17:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:394 on 2025-02-04 17:49_

We can have two different methods for it but it's important that we use the right method in the right context. I do think `infer_expression_type` as a query will be useful in other situations too, but it's not a "fits all" solution.

---

_@carljm reviewed on 2025-02-04 19:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/unpacker.rs`:46 on 2025-02-04 19:13_

In general, an expression node can only be part of one scope, and that scope is the only possible correct scope to pass to `scoped_expression_id` for that expression. So I believe that in any case where we are doing this pattern, either you could use `infer_expression_types` instead with no change in behavior, or the existing code was buggy. I think the only reason existing code wouldn't use the scope of the expression itself was just because that code happened to already have that same scope stored in a more convenient place. I think this is also true for the narrowing code you linked in the PR summary.

(Note this comment is just about semantic correctness, and independent of the whole question of whether we should introduce another layer of Salsa query in all of these places.)

---

_@carljm reviewed on 2025-02-04 19:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:394 on 2025-02-04 19:21_

It seems to me that this is sufficiently useful on ergonomic grounds, that it might even be worth having two variants of it, a Salsa-cached one and a non-Salsa-cached one. (The Salsa-cached one could just call the non-Salsa-cached one.)

I think it would also be very valuable if we could implement (via some combination of types, visibility, and code organization) a clearer distinction between "code that can be called across files" (mostly I think this code should live in `types.rs` and on `Type` today? Because `Type` is how type information travels between files) and "code that exclusively operates on only one file", and ensure that the former code can never see an AST directly. This might be a non-trivial refactor, but I think it would be worth it. Maybe worth creating an issue for, at least?

---

_@carljm reviewed on 2025-02-04 20:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:394 on 2025-02-04 20:56_

I created https://github.com/astral-sh/ruff/issues/15949 to follow up on the latter.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:46 on 2025-02-05 08:00_

I think the current implementation (as is on `main`) could be incorrect when this will be used to unpack in comprehensions. The reason being that the value expression of the first generator comes from the outer scope and not the comprehension scope where the unpacking would belong to once implemented (https://github.com/astral-sh/ruff/issues/15369).

---

_@dhruvmanila reviewed on 2025-02-05 08:00_

---

_@carljm reviewed on 2025-02-05 16:34_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/unpacker.rs`:46 on 2025-02-05 16:34_

@dhruvmanila I could be remembering wrong, but I would think we should give that "value expression of the first generator" an expression ID in the scope where it should be evaluated, maintaining the invariant that every expression belongs to exactly one scope, which I think would mean the current implementation of this helper is correct?

Sounds like this case deserves some investigation.

---

_@dhruvmanila reviewed on 2025-02-06 08:36_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/unpacker.rs`:46 on 2025-02-06 08:36_

> I could be remembering wrong, but I would think we should give that "value expression of the first generator" an expression ID in the scope where it should be evaluated, maintaining the invariant that every expression belongs to exactly one scope, which I think would mean the current implementation of this helper is correct?

Oh, I think you're correct. This means the previous implementation would've been probably incorrect for comprehensions because the unpacking would happen in the comprehension scope while the expression is in the outer scope but the previous implementation would try to get the expression from the comprehension scope where it doesn't exist.

We should be giving the expression of the first generator in the scope where it should be evaluated:

https://github.com/astral-sh/ruff/blob/17245b21a6d7142dd9df5e283f30b2fc7320f60e/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L627-L631

---

_Comment by @MichaReiser on 2025-02-20 11:40_

I tried to incorporate this change into https://github.com/astral-sh/ruff/pull/16268

---

_Closed by @sharkdp on 2025-02-21 09:49_

---
