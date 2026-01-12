```yaml
number: 17077
title: "[red-knot] visibility_constraint analysis for match cases"
type: pull_request
state: merged
author: ericmarkmartin
labels:
  - ty
assignees: []
merged: true
base: main
head: match-case-visibility
created_at: 2025-03-31T03:42:04Z
updated_at: 2025-04-03T11:15:34Z
url: https://github.com/astral-sh/ruff/pull/17077
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] visibility_constraint analysis for match cases

---

_@ericmarkmartin_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add visibility constraint analysis for pattern predicate kinds `Singleton`, `Or`, and `Class`.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

update conditional/match.md
<!-- How was it tested? -->



---

_Review requested from @carljm by @ericmarkmartin on 2025-03-31 03:42_

---

_Review requested from @AlexWaygood by @ericmarkmartin on 2025-03-31 03:42_

---

_Review requested from @sharkdp by @ericmarkmartin on 2025-03-31 03:42_

---

_Review requested from @dcreager by @ericmarkmartin on 2025-03-31 03:42_

---

_Comment by @github-actions[bot] on 2025-03-31 03:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Renamed from "Match case visibility" to "[red-knot] visibility_constraint analysis for match cases" by @ericmarkmartin on 2025-03-31 03:45_

---

_Label `red-knot` added by @MichaReiser on 2025-03-31 07:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/conditional/match.md`:93 on 2025-03-31 11:55_

I think this test is more interesting if the `Baz()` branch comes first: red-knot should be able to detect that the branch can never be taken, since `Baz` is disjoint from `FooSub`, but that's currently not tested since the `Baz` branch comes last. Can we add some more tests here showing how type inference changes when the `Bar` branch comes after the `FooSub` branch?

```suggestion
    match target:
        case Baz():
            y = 3
        case Foo():
            y = 4
        case Baz():
            y = 5

    reveal_type(y)  # revealed: Literal[4]

    match target:
        case Baz():
            z = 3
        case Bar():
            z = 4
        case Foo():
            z = 5

    reveal_type(z)  # revealed: Literal[4, 5]
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:188 on 2025-03-31 11:59_

micro-nit: we already import a bunch of things from `crate::semantic_index::predicate` on line 181; the second import could be combined with that statement, and the first import could use a similar style to the other imports in this file

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:589 on 2025-03-31 12:14_

is this branch tested at all? It seems like if I apply this diff to your branch then all tests still pass, so perhaps not?

```suggestion
            PatternPredicateKind::Singleton(singleton) => Truthiness::Ambiguous,
```

I think we can also do much better here! Consider something like this:

```py
def f(x: int):
    match x:
        case None:
            y = 42
        case _:
            y = 56
    reveal_type(y)
```

On your branch, this currently reveals `Literal[42, 56]`. But in reality `y` will always have value `56`, because `int` is disjoint from `None`, so the first branch will never be taken. We can infer `Truthiness::AlwaysFalse` for `PatternPredicateKind::Singleton::(Singleton::None)` if `subject_ty.is_disjoint_from(Type::none(db))` evaluates to `true`. And similar for the other singletons.

---

_@AlexWaygood reviewed on 2025-03-31 12:15_

Nice! Not a full review, but I spotted something in one branch that I suspect could apply to some of the other branches, so I'll hold off on making further comments until that's addressed.

Overall this looks great, though -- thank you!

---

_@ericmarkmartin reviewed on 2025-03-31 23:13_

---

_Review comment by @ericmarkmartin on `crates/red_knot_python_semantic/resources/mdtest/conditional/match.md`:93 on 2025-03-31 23:13_

> I think this test is more interesting if the Baz() branch comes first: red-knot should be able to detect that the branch can never be taken, since Baz is disjoint from FooSub, but that's currently not tested since the Baz branch comes last

Makes sense, thanks for the pointer

> Can we add some more tests here showing how type inference changes when the Bar branch comes after the FooSub branch?

I'm a bit confused here---I don't think we have any `FooSub` branches right now, just `Foo`, and `Bar` always comes after `Foo`. Do you want me to add more tests where this is a `Bar` branch that follows a `FooSub` branch? Would that be different than `Bar` following `Foo`?

---

_@ericmarkmartin reviewed on 2025-03-31 23:16_

---

_Review comment by @ericmarkmartin on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:188 on 2025-03-31 23:16_

I clearly lean on vscode code actions too much 😆 

---

_@AlexWaygood reviewed on 2025-04-01 11:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/conditional/match.md`:93 on 2025-04-01 11:38_

> I'm a bit confused here---I don't think we have any `FooSub` branches right now, just `Foo`, and `Bar` always comes after `Foo`.

Argh, sorry for the garbled review comment! What I _meant_ to say was that it would be nice if we could have a test where `Foo` comes after `Bar` as well as one where `Bar` comes after `Foo`.

The `Foo` branch has visibility `Truthiness::AlwaysTrue`, so if it comes before `Bar` then we know the value of the variable is definitely the value that is assigned in the `Foo` branch. But the `Bar` branch has visibility `Truthiness::Ambiguous`, so if it comes before the `Foo` branch then we have to account for the fact that the `Bar` branch _might_ be taken (if an instance of a class `FooBar` that is a subclass of both `Foo` and `Bar` is passed in), and it also _might not_ be taken. It follows from this that if `Foo` comes after `Bar`, we should union the types in the `Foo` and `Bar` branches together when inferring the type of the assigned variable.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:571 on 2025-04-01 11:42_

I think we can do disjointness checking for `Value` predicates too, right?

```suggestion
                } else if subject_ty.is_disjoint_from(db, value_ty) {
                    Truthiness::AlwaysFalse
                } else {
                    Truthiness::Ambiguous
                }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:583 on 2025-04-01 11:47_

it should be always true here that the type of `singleton_ty` is recognised as a singleton type by red-knot. That means that we can skip checking whether `subject_ty` is single-valued and go straight to the equivalence check: if `subject_ty` is equivalent to `singleton_ty`, that means that `subject_ty` is a singleton type, which means it is also a single-valued type. (The set of singleton types is a subset of the set of single-valued types; all singleton types are single-valued.)

```suggestion
                debug_assert!(singleton_ty.is_singleton(db));

                if subject_ty.is_equivalent_to(db, singleton_ty) {
                    Truthiness::AlwaysTrue
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:313 on 2025-04-01 11:48_

I don't think this change is necessary with the latest version of this PR

```suggestion
    fn is_none(&self, db: &'db dyn Db) -> bool {
```

---

_@AlexWaygood reviewed on 2025-04-01 11:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/conditional/match.md`:158 on 2025-04-01 19:40_

This comment seems out of place here?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/conditional/match.md`:95 on 2025-04-01 19:42_

Not sure that all these tests need the initial `y = 1` -- its existence demonstrates a fairly trivial point (that an unconditional binding shadows a prior binding) that probably doesn't need repeated testing. But it also doesn't really matter, there's little cost to including it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:571 on 2025-04-01 19:55_

Testing this requires a subject that is not single-valued, and a value predicate that is disjoint from it. Something like this:

```py
from typing import final

@final
class C: pass

def _(subject: C):
    y = 1
    match subject:
        case 1:
            y = 2
    reveal_type(y)  # with the current PR this is Literal[1, 2], it should just be Literal[1]
```

It would of course also be fine to do this as a separate PR, though perhaps we'd at least want to capture a TODO here for it if we defer it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:610 on 2025-04-01 20:05_

This is awesome to have, and a really nice implementation of it too!

I don't see any tests exercising it, though. If I replace this entire arm with just `Truthiness::Ambiguous`, all tests pass. Can we add some tests for this?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:638 on 2025-04-01 20:09_

We could have a TODO here for actually analyzing truthiness of the guard?

---

_@carljm reviewed on 2025-04-01 20:09_

This is excellent work, thank you!!

---

_Review comment by @ericmarkmartin on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:571 on 2025-04-03 00:26_

Is this sound? With the suggested changes we have

```python
from typing import final

@final
class C:
    def __eq__(self, other: object) -> bool:
        return other == 1

def _(subject: C):
    y = 1
    match subject:
        case 1:
            y = 2
    reveal_type(y)  # revealed_type: Literal[1]
```

but I think (this is the same code but with `print` instead of `reveal_type`)

```python
from typing import final

@final
class C:
    def __eq__(self, other: object) -> bool:
        return other == 1

def _(subject: C):
    y = 1
    match subject:
        case 1:
            y = 2
    print(y)
```

```console
> uvx python /tmp/foo.py
2
```



---

_@ericmarkmartin reviewed on 2025-04-03 00:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:571 on 2025-04-03 00:38_

Ah, of course, great call. Yeah, we can't do this. It only works for single-valued subject (where we can be sure it doesn't compare equal to the predicate value.) And that's already supported.

Thanks for catching that!

---

_@carljm reviewed on 2025-04-03 00:38_

---

_@AlexWaygood approved on 2025-04-03 11:10_

Thank you, this is great!

---

_@AlexWaygood reviewed on 2025-04-03 11:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs`:571 on 2025-04-03 11:11_

Seconded, thanks -- that's a great catch! I forgot how value patterns worked. Great that you added a test for this!

---

_Merged by @AlexWaygood on 2025-04-03 11:15_

---

_Closed by @AlexWaygood on 2025-04-03 11:15_

---
