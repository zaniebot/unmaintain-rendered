```yaml
number: 13918
title: "[red-knot] Type narrow in else clause"
type: pull_request
state: merged
author: TomerBin
labels:
  - ty
assignees: []
merged: true
base: main
head: tomer/type-narrow-in-else-clause
created_at: 2024-10-24T22:26:46Z
updated_at: 2024-10-26T16:57:16Z
url: https://github.com/astral-sh/ruff/pull/13918
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Type narrow in else clause

---

_Pull request opened by @TomerBin on 2024-10-24 22:26_

Added support for type narrowing in elif and else scopes as part of #13694.

## Summary

This is ready for review, though still somewhat of a draft. Please let me know if I addressed this correctly or if a completely different solution would be better :)

## Test Plan

- mdtest
- builder unit test for union negation.

---

_Comment by @github-actions[bot] on 2024-10-24 22:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@TomerBin reviewed on 2024-10-24 23:13_

---

_Review comment by @TomerBin on `crates/red_knot_python_semantic/src/types/builder.rs`:683 on 2024-10-24 23:13_

I didn't find any another builder tests that assert the `display` value of the result type but I actually find it more expressive and readable that way. LMK if I should better stick to the other tests' style. 

---

_@TomerBin reviewed on 2024-10-24 23:34_

---

_Review comment by @TomerBin on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:16 on 2024-10-24 23:34_

In this context, "constraint" term is used for both the node that applies the constraint, and for the outcome of the narrowing process. 
What do you think about renaming this one into `TypeGuard` and use `constraint` name only for the output (map from symbol ids to types)?

---

_Marked ready for review by @TomerBin on 2024-10-24 23:35_

---

_Review requested from @carljm by @TomerBin on 2024-10-24 23:35_

---

_Review requested from @MichaReiser by @TomerBin on 2024-10-24 23:35_

---

_Review requested from @AlexWaygood by @TomerBin on 2024-10-24 23:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:60 on 2024-10-24 23:40_

Shouldn't we be able to narrow this to `Literal[True]`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is_not.md`:95 on 2024-10-24 23:42_

Shouldn't we be able to infer `Literal[False]` here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:683 on 2024-10-24 23:52_

No, I think this is fine!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:177 on 2024-10-24 23:59_

I don't think we need to clone here, tests pass with just this:
```suggestion
                self = self.add_negative(*elem);
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:667 on 2024-10-24 23:59_

Nice catch!!

I think we might also behave wrongly for de Morgan's other law, adding an intersection on the negative side of an IntersectionBuilder. This should distribute out to a union, (using I think basically the same fold code you removed above). Can you add a test and fix for that as well? (Could be in a separate PR, too.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:16 on 2024-10-25 00:05_

Hmm. I'm open to a renaming here to better distinguish these, but not sure I like `TypeGuard`, I'd rather avoid that term outside of dealing with the actual `typing.TypeGuard` feature.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:68 on 2024-10-25 00:11_

Hmm, I think Salsa does something kind of inefficient when a tracked function has multiple inputs; it internally interns all the arguments. Although it's slightly weird-looking, I think we will get more efficient results here if we break into two separate Salsa tracked functions, `all_narrowing_constraints_for_expression` and `all_negative_narrowing_constraints_for_expression`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:75 on 2024-10-25 00:12_

Is it worth re-building a `Constraint` struct here vs just passing in the two elements separately to the builder? It mostly seems like it just results in more verbose usage inside the builder (`self.constraint.negative` vs `self.negative`).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:224 on 2024-10-25 00:16_

We could make the match arms a bit less repetitive/verbose here (though arguably also less explicit?) if we call a function here that returns the negated `ast::CmpOp` if `negative` and then match on the resulting `CmpOp`. What do you think?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_elif_else.md`:11 on 2024-10-25 00:23_

We've previously discussed that we can't actually safely do this narrowing, because `x` here could be an instance of some subclass of `int` that overrides `__eq__` to compare equal to `1`.

Perhaps experience will tell us that we are being too conservative here, and we should just ignore that possibility; but I'd rather not do that until/unless it's clear that we need to.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:258 on 2024-10-25 00:31_

As discussed in another comment in the tests, I don't think we can generally narrow on equality checks (or negative inequality checks). But I think we can (and probably will need to) do a more limited form of this, where we check the type of the LHS and if it is a union of single-valued types, we eliminate (by constraining with a negative intersection of) any single-valued types that we know are disjoint with the RHS.

In other words, while we can't infer that `x: int; if x == 1:` results in `Literal[1]`, we can infer that `x: Literal[1, 2, 3]; if x == 1:` results in `Literal[1]`, and I think we'll need to be able to make the latter inference.

(There may be a more general way to frame this than what I wrote above, need to think about it.)

---

_@carljm reviewed on 2024-10-25 00:32_

This is really excellent, thank you for doing this!!

Some comments below; maybe don't put too much work into addressing comments until @AlexWaygood and/or @sharkdp have a chance to take a look.

---

_Review requested from @sharkdp by @carljm on 2024-10-25 00:33_

---

_Label `red-knot` added by @carljm on 2024-10-25 01:18_

---

_@sharkdp reviewed on 2024-10-25 06:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:60 on 2024-10-25 06:51_

No, because there are two conditions in the `if` clause. And only one of them could be `False`: if `y = True` and `x = False`, we also end up in this branch.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is_not.md`:95 on 2024-10-25 06:58_

No. Similar to above: if `x = y = False`, we also end up in this branch.

---

_@sharkdp reviewed on 2024-10-25 06:58_

---

_@sharkdp reviewed on 2024-10-25 07:03_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is_not.md`:1 on 2024-10-25 07:03_

In this file, can we maybe also add something equivalent to the test "`is` for other types" test, but for `is not`:
```py
class A: ...

x = A()
y = x if flag else None

if y is not x:
    reveal_type(y)  # revealed: A | None
else:
    reveal_type(y)  # revealed: A
```

This wouldn't have been very interesting/useful before. But now with the `else` branch, it is.

---

_@MichaReiser reviewed on 2024-10-25 07:27_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:16 on 2024-10-25 07:27_

We often called these types *Kind* in other places.

---

_@sharkdp reviewed on 2024-10-25 08:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md`:110 on 2024-10-25 08:11_

Not sure if always desirable, but we could maybe add a `TODO` here to always remove `object` from the positive side of an intersection.

---

_@TomerBin reviewed on 2024-10-25 15:10_

---

_Review comment by @TomerBin on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:16 on 2024-10-25 15:10_

What do you think about "Predicate"? I like that it's more easy to understand what "negative" means on a predicate, than on Type or Kind as of the negation of a predicate doesn't always imply the negation of its outcome constraint.


---

_@carljm reviewed on 2024-10-25 15:36_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is.md`:60 on 2024-10-25 15:36_

Unresolving this conversation because it might be useful to add a comment here explaining this?

---

_@carljm reviewed on 2024-10-25 15:36_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_is_not.md`:95 on 2024-10-25 15:36_

And here?

---

_@carljm reviewed on 2024-10-25 15:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:16 on 2024-10-25 15:41_

I'm happy with "Predicate"!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:258 on 2024-10-25 17:49_

This represents a more general category of narrowing cases that we will also run into with, for example, simple `if not x:` narrowing, where the information gained from the narrowing in that case is "`x` is falsy" and in this case is "`x` is equal to 1", neither of which is a constraint that can correctly be expressed simply a type to intersect `x` with, given our currently available types. I spent some more time thinking about this, and wrote it up at https://github.com/astral-sh/ruff/issues/13694#issuecomment-2438438759

I'm afraid that's a bit long, but I think the relevant conclusion for this PR is, let's not handle it in this PR :) For this PR, let's just remove the narrowing on `==` (or negated `!=`) entirely for now.


---

_@carljm reviewed on 2024-10-25 17:49_

---

_Comment by @carljm on 2024-10-25 17:50_

@TomerBin are you ready for another review of this? There are still some previous comments unresolved.

---

_Comment by @TomerBin on 2024-10-25 18:33_

> @TomerBin are you ready for another review of this? There are still some previous comments unresolved.

@carljm Not yet.. I wanted to ask something regarding __eq__. The logic prior to this PR does add single-value types in the rhs as a negative constraint on the symbol, without considering whether the lhs is single-valued.
```py
ast::CmpOp::NotEq => {
                        if rhs_ty.is_single_valued(self.db) {
                            let ty = IntersectionBuilder::new(self.db)
                                .add_negative(rhs_ty)
                                .build();
                            self.constraints.insert(symbol, ty);
                        }
                    }
```
In the scope of this PR - do you think this logic should be adopted as well? Or should I just ignore the negation of this predicate at all?

Thanks :)

---

_Comment by @carljm on 2024-10-25 18:46_

Although it's not safe to apply an intersection with `Literal[1]` as a result of `if x == 1`, it is safe to apply an intersection with `~Literal[1]` as a result of `if x != 1`. In the former case, we can't say that `x` must be `Literal[1]`, it could be some other type that just compares equal to `1`. But in the latter case, we can definitively say that `x` cannot be `Literal[1]`, because we know that every inhabitant of `Literal[1]` always compares equal to `1`.

So the current code is correct.

And in this PR, I think it is always the case that if we can infer something from `x != y`, then we can infer the same thing from the `else` clause of `if x == y`.

So yes, I think we should, in this PR, infer that in the `else` clause of `if x == 1`, we apply a constraint of `~Literal[1]` to `x`.


---

_@TomerBin reviewed on 2024-10-25 18:47_

---

_Review comment by @TomerBin on `crates/red_knot_python_semantic/src/types/builder.rs`:667 on 2024-10-25 18:47_

Im not sure how to test that in a unit test. I need to generate an intersection between two types that won't be simplified (so they can't be disjoint). If it was an mdtest I'd do it with class literals, but I'm not sure how to that that in here. Do you have an idea?
Maybe Ill prefer to do it in separate PR if you're happy with that.


---

_@carljm reviewed on 2024-10-25 19:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/builder.rs`:667 on 2024-10-25 19:00_

Yes, separate PR is totally fine! I've created an issue just to make sure we don't lose track of it, in case you don't have time to do it: https://github.com/astral-sh/ruff/issues/13931

I think you can use known types from typeshed to create the intersection? E.g. `KnownClass::Str.to_instance()` and `KnownClass::Int.to_instance()` can be intersected without simplifying.

(Technically we probably should consider these disjoint because they are both builtin types with different memory layouts, so you can't subclass both of them at runtime. But we don't represent this yet.)

I think you can also just `db.write_file(...)` a file with a couple class definitions in it, much like the tests in `infer.rs`, and then pull out those class types. It's a bit more boilerplate, and there aren't existing tests in here that do it, but it's fine to do it.


---

_Comment by @TomerBin on 2024-10-26 09:02_

I think that It's ready for another CR :) 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_elif_else.md`:6 on 2024-10-26 10:07_

We should fill in the bodies of helper functions in tests so that they don't start failing in the future when we start checking that functions actually return what they say they return:

```suggestion
def int_instance() -> int:
    return 42
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not_eq.md`:81 on 2024-10-26 10:08_

```suggestion
def int_instance() -> int:
    return 42
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/constraint.rs`:12 on 2024-10-26 10:12_

It feels slightly odd to me that a negative boolean value for this field indicates a positive predicate. Could we have the meaning flipped, so that a positive value for the field indicates a positive predicate (and vice versa)?

```suggestion
    pub(crate) is_positive: bool,
```

---

_@AlexWaygood reviewed on 2024-10-26 10:21_

(Just a few comments from skimming, haven't looked properly yet!)

Looks like some of your tests are failing following https://github.com/astral-sh/ruff/commit/6aaf1d944657a7f37bd99c781c2b1ebdebfdf0e3 being pushed to `main` — you'll need to define the `flag` variables in your tests in the same way we did in that commit. (You may need to merge in `main` or rebase to see the same test failures locally)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_elif_else.md`:30 on 2024-10-26 15:40_

We'll not do it in this PR, but let's note the TODOs

```suggestion
    # TODO should be Literal[1]
    reveal_type(x)  # revealed: Literal[1, 2, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_elif_else.md`:33 on 2024-10-26 15:41_

```suggestion
    # TODO should be Literal[2]
    reveal_type(x)  # revealed: Literal[2, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_elif_else.md`:47 on 2024-10-26 15:44_

I think this comment meant to discuss `Literal[3]`, not `Literal[1]`? `Literal[1]` should be the _only_ possibility here, since we are dealing with literals. We're not doing it in this PR, but we should do narrowing on `==` enough to eliminate `Literal[3]` here, since we know it can never be `== 1`.

```suggestion
    # TODO should be `Literal[1]`
    reveal_type(x)  # revealed: Literal[1, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_elif_else.md`:13 on 2024-10-26 15:46_

let's clarify this for future readers, that this is _not_ a todo

```suggestion
    # cannot narrow; could be a subclass of `int`
    reveal_type(x)  # revealed: int
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_elif_else.md`:50 on 2024-10-26 15:49_

But we should, because we're dealing with literal types! Just not in this PR.
```suggestion
    # TODO should be Never
    reveal_type(x)  # revealed: Literal[1, 2, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_elif_else.md`:56 on 2024-10-26 15:49_

```suggestion
    # TODO should be Never
    reveal_type(x)  # revealed: Literal[1, 2]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_nested.md`:44 on 2024-10-26 15:52_

```suggestion
        # TODO should be `Literal[2]`
        reveal_type(x)  # revealed: Literal[2, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_nested.md`:52 on 2024-10-26 15:52_

```suggestion
    # TODO should be Literal[1]
    reveal_type(x)  # revealed: Literal[1, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_nested.md`:55 on 2024-10-26 15:52_

```suggestion
    # TODO should be Never
    reveal_type(x)  # revealed: Literal[1, 2, 3]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not_eq.md`:16 on 2024-10-26 15:54_

Again here, we should be able to do this narrowing, in a future PR, because the types `None` and `Literal[1]` are both literal types, there can't be unknown subclasses with unknown equality behavior.
```suggestion
    # TODO should be None
    reveal_type(x)  # revealed: None | Literal[1]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not_eq.md`:32 on 2024-10-26 15:54_

```suggestion
    # TODO should be Literal[False]
    reveal_type(x)  # revealed: bool
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not_eq.md`:65 on 2024-10-26 15:55_

Again here because these are literal class types that don't admit subclasses, in future we should be able to do this narrowing
```suggestion
    # TODO should be Literal[A]
    reveal_type(C)  # revealed: Literal[A, B]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_not_eq.md`:81 on 2024-10-26 15:56_

```suggestion
    # TODO should be Literal[2]
    reveal_type(x)  # revealed: Literal[1, 2]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:168 on 2024-10-26 15:58_

I think if we rename `Constraint` to `Predicate` we should also rename `ScopedConstraintId` to `ScopedPredicateId`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:7 on 2024-10-26 15:59_

seems like there are some missed places where we should rename `constraint` to `predicate`
```suggestion
//! We need to track arbitrary associations between bindings and predicates, not just a single set
```

---

_@carljm approved on 2024-10-26 16:03_

Looks good! 90% of my comments are just that we should have TODOs in tests reflecting the equality narrowing for literal types that we should do in future, as discussed. I'll just apply these suggested changes.

It also looks to me like the constraint -> predicate rename isn't quite fully carried through. If it's OK I might just also push some changes to complete that, and then go ahead and land this.

---

_Comment by @carljm on 2024-10-26 16:08_

After looking at it more, I think for now I'm just going to revert the Predicate name change in this PR. There's still a ton of things named "constraint" inside the use-def map and semantic index, so the current change just leaves us in an even less consistent naming situation. Fully carrying out the rename in this PR would drown out the substantive changes.

Still open to the renaming if you're interested in pursuing it, but let's consider it as a separate PR. If we do it, it should be thorough -- like a grep for `onstraint` inside `src/semantic_index/` should give zero hits, and `semantic_index/constraint.rs` should be renamed, etc. The name "constraint" would be reserved only for `narrow.rs`, on the type inference side.

---

_Comment by @TomerBin on 2024-10-26 16:17_

LGTM 👍👍

---

_Merged by @carljm on 2024-10-26 16:22_

---

_Closed by @carljm on 2024-10-26 16:22_

---

_Branch deleted on 2024-10-26 16:57_

---
