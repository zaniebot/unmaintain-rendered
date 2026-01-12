```yaml
number: 15839
title: "[red-knot] Should A ∧ !A always be false?"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/visconand
created_at: 2025-01-30T22:49:55Z
updated_at: 2025-02-03T14:17:39Z
url: https://github.com/astral-sh/ruff/pull/15839
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Should A ∧ !A always be false?

---

_@dcreager_

This mimics a simplification we have on the OR side, where we simplify `A ∨ !A` to true.  This requires changes to how we add `while` statements to the semantic index, since we now need distinct `VisibilityConstraint`s if we need to model evaluating a `Constraint` multiple times at different points in the execution of the program.

---

_Review requested from @carljm by @dcreager on 2025-01-30 22:49_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-30 22:49_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-30 22:49_

---

_Review requested from @sharkdp by @dcreager on 2025-01-30 22:49_

---

_Label `internal` added by @dcreager on 2025-01-30 22:50_

---

_Label `red-knot` added by @dcreager on 2025-01-30 22:50_

---

_@carljm approved on 2025-01-31 00:36_

---

_@carljm requested changes on 2025-01-31 00:53_

I don't think this holds, because we are dealing with three-valued logic (there is the `Ambiguous` possibility), and a value of `Ambiguous` truthiness can change truthiness between one time it is checked and another.

Multiple checks of the truthiness of the same condition only occur in while loops, which is why all of the test failures occur in while-loop tests. This PR models the premise that a while-loop test can never change truthiness over time, which would indeed lead to the conclusion seen in the test failures, that a `while` loop with no `break` must either be never entered at all, or must be an infinite loop once entered.

I think in order for this PR to be valid, we would need to create two separate visibility constraints for the same while-loop test expression, one for "as checked on first entry" and another for "as checked on subsequent iteration", so that we wouldn't actually see those as "the same constraint", and would thus model the possibility of entering the loop and then leaving it on a later iteration. (I think this same change in `while` loop visibility constraints would be necessary if we switch to BDDs for visibility constraints.)

I think technically the existing OR simplification is wrong for the same reason? But in practice it doesn't cause a problem, either because the OR simplification case doesn't arise with while loops, or because simplifying to `ALWAYS_TRUE` doesn't lead us to eliminate paths as unreachable, or for both reasons.

---

_Comment by @carljm on 2025-01-31 00:56_

cc @sharkdp, you might find this interesting.

---

_Comment by @dcreager on 2025-01-31 01:26_

>  a value of `Ambiguous` truthiness can change truthiness between one time it is checked and another.

Yep, that makes sense!  The part that I wasn't sure about was what the scope of "one time it is checked and another" is.  My intuition would be that a literal (in the boolean formula sense, not the Python source sense) might evaluate to `Ambiguous`, meaning that we don't know if it's true or false — but that within the context of a particular evaluation of the boolean formula, it would have the same true or false value each time it appears.  Which would make `A ∧ !A == 0` hold.

> Multiple checks of the truthiness of the same condition only occur in while loops, which is why all of the test failures occur in while-loop tests.

Or put another way, while-loop tests are the only place (at least for now) where we need a visibility constraint to model the relationship between two different states in the execution trace of the program.

> I think technically the existing OR simplification is wrong for the same reason?

Definitely agree — and I think that's a better wording of my question! :smile:   Either both `A ∧ !A == 0` and `A ∨ !A == 1` should hold, or neither should.

---

_Comment by @carljm on 2025-01-31 01:34_

> within the context of a particular evaluation of the boolean formula, it would have the same true or false value each time it appears

The problem is that we construct boolean formulae (for visibility of paths through a `while` loop) which contain a single constraint that represents checks of the while-test expression's truthiness at different points in execution state (first entry vs later iteration). Because we do this, we create the scenario that "the same constraint" in the same boolean formula doesn't necessarily have a consistent truthiness value, even for a single evaluation of the formula.

If we instead created separate visibility constraints (referring to the same test expression) for the truthiness checks in these two different execution states, then the simplification would hold. (I think this change in the semantic index builder's handling of `while` loops would not be too difficult, but I haven't tried to make the change.)

---

_Comment by @carljm on 2025-01-31 01:43_

Once we model the back-edge in control flow in a while loop, we _could_ even evaluate the test expression under two different conditions, one without the back-edge (for first entry) and one with the back-edge (for later iteration). This could change our evaluation of its static truthiness, too. (Imagine an `x = True; while x: x = get_bool()`.)

---

_Comment by @dcreager on 2025-01-31 17:36_

> I think in order for this PR to be valid, we would need to create two separate visibility constraints for the same while-loop test expression, one for "as checked on first entry" and another for "as checked on subsequent iteration", so that we wouldn't actually see those as "the same constraint", and would thus model the possibility of entering the loop and then leaving it on a later iteration. (I think this same change in `while` loop visibility constraints would be necessary if we switch to BDDs for visibility constraints.)

This is done!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1012 on 2025-01-31 17:48_

Doesn't `(first ∨ (first ∧ later))` simplify to just `first`? If `first` is true, it is always true; if `first` is false, it is always false.

Which I think is also intuitive, because for the body of the while loop to be reachable, it is both necessary and sufficient that the first evaluation of the test condition be truthy.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:990 on 2025-01-31 17:58_

I wonder if this could be a bit more ergonomic, and not have to expose `VisibilityConstraintAtom` and the implementation detail of its internal `u8` to the semantic index builder? But I see there are complexities there, and with only one call site at the moment I don't think it matters much.

---

_@carljm approved on 2025-01-31 17:59_

Looks like this fixes the test failures, sweet!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1012 on 2025-01-31 18:21_

Yes, good catch!  I had thought I needed to assert that later evaluations of the condition also evaluate to true.  But this shows that that's not the right interpretation — we don't know that we _did_ evaluate the condition a second time, and so executing the body does not imply that (there was) a later evaluation (and it) evaluated to true.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:990 on 2025-01-31 18:22_

Done. I removed the `Atom` wrapper.  My thought here was to reduce the number of `, 0`s I'd have to add, but most of those were already hidden behind helper methods

---

_@dcreager reviewed on 2025-01-31 18:22_

---

_@dcreager reviewed on 2025-01-31 18:31_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:990 on 2025-01-31 18:31_

(Making the `u8` not visible at all would be more invasive, since that's how the caller up in the semantic index builder specifies that it wants distinct copies.  It's up to the caller to choose the values, such that distinct `u8`s refer to different evaluations of the constraint.  Do you have thoughts on a different API shape to express that?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:990 on 2025-01-31 18:48_

I think I was vaguely envisioning some kind of `copy` API that would take an existing constraint ID and bump the counter behind the scenes. But the awkwardness there is that a constraint ID might not represent a `VisibleIf` constraint at all, making the API error prone. So I'm not sure that would be an improvement. It's already much nicer without the extra `Atom` type, I think what you have is good.

---

_@carljm reviewed on 2025-01-31 18:48_

---

_@carljm approved on 2025-01-31 18:49_

---

_Merged by @dcreager on 2025-01-31 19:06_

---

_Closed by @dcreager on 2025-01-31 19:06_

---

_Branch deleted on 2025-01-31 19:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:185 on 2025-01-31 20:40_

It's late here already, but just reading this, I wonder why it applies to while conditions, but not to something like
```py
for _ in iterable:
    if <cond>:
        # …
```
Is it because we merge control flow twice here (after the if, after the for) instead of just once in the while loop? Would a `break` point inside the `if` complicate the situation?

---

_@sharkdp reviewed on 2025-01-31 20:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:185 on 2025-01-31 20:49_

~I think it's because currently a `for` loop generates no visibility constraints. (We don't attempt to detect statically-known-empty iterables.) If we did add that feature, we might need to consider this, but I'm still not sure it would apply unless we also tried to detect statically-known-infinite iterables! Which I doubt we would ever do.~

EDIT: ignore the above, I totally missed the point of your comment, which was the nested `if` test. Thinking about it...

---

_@carljm reviewed on 2025-01-31 20:49_

---

_@carljm reviewed on 2025-01-31 21:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:185 on 2025-01-31 21:19_

Not sure I've totally thought through all the possible concrete visibility constraint trees we might generate here, but my intuition says it's not an issue in this scenario, because there is no difference between any one iteration of the loop and any other one, in terms of the effect on which code is visible or not. Which is not true in a `while` loop, where the key fact is that a static `True` means we'll never exit the loop, a static `False` means we'll never enter it, and a `True` that later changes to `False` means we can enter and then exit.

---

_@sharkdp reviewed on 2025-02-03 08:08_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:185 on 2025-02-03 08:08_

It seems to me like the following two are similar control flow constructs (ignoring `else` branches in loops…):
```py
while cond:
    # <code block>
```
and
```py
for _ in itertools.repeat(None):
    if not cond:
        break
    # <code block>
```


It looks like we can handle the former, but not the latter:
```py
import itertools

cond = False

w = 1
f = 1

while cond:
    w = 2

for _ in itertools.repeat(None):
    if not cond:
        break

    f = 2

print(reveal_type(w))  # Literal[1]
print(reveal_type(f))  # Literal[1, 2] ?
```

Or am I missing something? This might not be related to this PR at all(?).

(Edit: I guess I could only expect the latter to be working with https://github.com/astral-sh/ruff/pull/15817)

---

_@carljm reviewed on 2025-02-03 14:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:185 on 2025-02-03 14:17_

I think it's more than #15817. The latter example is only equivalent to the former if we are able to detect that an iterator is infinite. This is not reflected in the type system, and would require special casing for certain iterables (like ‘itertools.repeat`); it's not possible in general.

If the iterator is not infinite (or not known to be infinite), then exit from the loop is always possible at every iteration, regardless of the value of `cond`, and the result we currently give is correct. 

Detecting known-empty iterators and their effect on `for` loop control flow is similarly difficult to do in general. 

---
