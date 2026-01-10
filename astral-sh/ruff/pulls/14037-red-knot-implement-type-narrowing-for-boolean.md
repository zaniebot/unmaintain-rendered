```yaml
number: 14037
title: "[red-knot] Implement type narrowing for boolean conditionals"
type: pull_request
state: merged
author: TomerBin
labels:
  - ty
assignees: []
merged: true
base: main
head: tomer/type-narrow-boolean-conditionals
created_at: 2024-11-01T11:14:26Z
updated_at: 2024-11-04T23:03:50Z
url: https://github.com/astral-sh/ruff/pull/14037
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Implement type narrowing for boolean conditionals

---

_Pull request opened by @TomerBin on 2024-11-01 11:14_

## Summary

This PR enables red-knot to support type narrowing based on `and` and `or` conditionals, including nested combinations and their negation (for `elif` / `else` blocks and for `not` operator). Part of #13694.

In order to address this properly (hopefully 😅), I had to run `NarrowingConstraintsBuilder` functions recursively. In the first commit I introduced a minor refactor - instead of mutating `self.constraints`, the new constraints are now returned as function return values. I also modified the constraints map to be optional, preventing unnecessary hashmap allocations.
Thanks @carljm for your support on this :)

The second commit contains the logic and tests for handling boolean ops, with improvements to intersections handling in `is_subtype_of` .

As I'm still new to Rust and the internals of type checkers, I’d be more than happy to hear any insights or suggestions.
Thank you!

---

_Marked ready for review by @TomerBin on 2024-11-01 11:29_

---

_Review requested from @carljm by @TomerBin on 2024-11-01 11:29_

---

_Review requested from @MichaReiser by @TomerBin on 2024-11-01 11:29_

---

_Review requested from @AlexWaygood by @TomerBin on 2024-11-01 11:29_

---

_Review requested from @sharkdp by @TomerBin on 2024-11-01 11:29_

---

_Comment by @github-actions[bot] on 2024-11-01 11:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-11-01 15:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_boolean.md`:42 on 2024-11-01 18:04_

With detection of statically-known `True`, this could be `B` or `B & ~A`, right?

Not saying we have to do it in this PR, but it could be a) worth a TODO comment, and/or b) worth having a test that uses `and bool_instance()` for the "adds no narrowing constraint" condition rather than `and True`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_boolean.md`:72 on 2024-11-01 18:04_

same as above

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_boolean.md`:83 on 2024-11-01 18:07_

This is arguable. The type `C & ~A & ~B` is technically correct here, and it is technically correct not to simplify it, because of multiple inheritance.

Whether it is _practically useful_ not to simplify it is another question, but we can let that be driven by future real-world experience, I don't think we need a TODO for it.
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_boolean.md`:130 on 2024-11-01 18:10_

Should this test also demonstrate a case where all arms _do_ add the constraint? Or, even more subtly, where all arms add overlapping constraints, and the OR of those constraints still does apply a constraint? E.g.

```
x: int | None

if x or x is not None:
    reveal_type(x)  # revealed: int
```

or 

```
if isinstance(x, A) or isinstance(x, B):
    reveal_type(x)  # revealed: A | B
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals_boolean.md`:121 on 2024-11-01 18:11_

I think this test is stronger if all arms make the same assertion, but about different symbols. Since otherwise we could pass the test just due to "different constraints" logic already tested above.

```suggestion
if isinstance(x, A) or isinstance(y, A):
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:557 on 2024-11-01 18:26_

I think this should rather be "all negative elements are disjoint with `ty`".

Example case is that `bool`, `BooleanLiteral[False]`, and `BooleanLiteral[True]` should all be subtypes of `int & ~Literal[0]`.

Testing this for a `BooleanLiteral` will require adding a case for `BooleanLiteral[_] <: int`, which we are currently missing -- but that's easy to add in this diff.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:545 on 2024-11-01 18:43_

I think here, if this check fails, the other thing we need to check is "is at least one positive element of `self_intersection` disjoint from this negative element of `target_intersection`" -- if so, that should also satisfy this negative element of `target_intersection.

Example case would be that `A & int` is a subtype of `~Literal[False]`, because `A` is disjoint from `Literal[False]` (even though `int` is not -- or should not be).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:202 on 2024-11-01 18:53_

Nit: now that these methods do not have side effects, but rather return their constraints, I think we should rename from `add_*` to `evaluate_*` (e.g. `self.evaluate_expr_call` etc)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:374 on 2024-11-01 18:56_

It looks like both branches have this iterator in common, it could be extracted into a variable above?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:394 on 2024-11-01 19:07_

I think this is too strict, and will mean we can't understand a more complex constraint like `if (isinstance(x, A) and isinstance(y, B)) or (isinstance(x, B) and isinstance(y, A)):`

In this case of course we have no way to represent the precise relationship between the types of `x` and `y`, but it is correct to narrow both `x` and `y` to `A | B`. Your current code will refuse to do that.

I think the code might be clearer here if you structure it more similarly to the above branch, rename the current `merge_constraints` function to `merge_constraints_and`, and write a new `merge_constraints_or` to use in this branch.

The `merge_constraints_or` function should union the constraints (instead of intersecting them like `merge_constraints_and` does), and should eliminate any variables not constrained by both sides, or where the union of constraints results in `object` (these two cases are really the same: no constraint for a variable is implicitly "constrained to `object`").

---

_@carljm requested changes on 2024-11-01 19:08_

This is fantastic work!

I do have a few comments: feel free to push back if any of my comments don't seem right, it's quite possible I've missed something important!

---

_Comment by @TomerBin on 2024-11-01 21:27_

> This is fantastic work!
> 
> I do have a few comments: feel free to push back if any of my comments don't seem right, it's quite possible I've missed something important!

Thanks for the review! I'll address all comments soon



---

_@TomerBin reviewed on 2024-11-03 16:00_

---

_Review comment by @TomerBin on `crates/red_knot_python_semantic/src/types.rs`:557 on 2024-11-03 16:00_

Nice catch :) 
Can we test it with just int variants? As `Literal[1]` should be a subtype of `int  & ~Literal[0]`?

---

_Comment by @TomerBin on 2024-11-03 16:01_

I think i've addressed all comments, it is ready for another review now :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2154 on 2024-11-04 21:20_

I'm not sure which case this was intended to test, but I don't think it is testing intersection subtype-of logic; it is only testing that `int & Literal[2]` immediately simplifies to `Literal[2]`, because `Literal[2] <: int`. So then of course `Literal[2] <: Literal[2]`.

---

_@carljm approved on 2024-11-04 21:24_

This is awesome!! One nit on one test, then it's good to go.

---

_@TomerBin reviewed on 2024-11-04 22:53_

---

_Review comment by @TomerBin on `crates/red_knot_python_semantic/src/types.rs`:2154 on 2024-11-04 22:53_

Thanks! 
I wanted to test that not all positive intersection members must be subtypes of the target, only one is required.
Fixed in this commit - https://github.com/astral-sh/ruff/pull/14037/commits/1fd24024801cec1899640e7b611864ea6061ad2f


---

_Merged by @carljm on 2024-11-04 22:54_

---

_Closed by @carljm on 2024-11-04 22:54_

---
