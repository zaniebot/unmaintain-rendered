```yaml
number: 15470
title: "[red-knot] Migrate `is_equivalent_to` unit tests to Markdown tests"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: rk-pt-is-equivalent-to
created_at: 2025-01-14T09:36:33Z
updated_at: 2025-01-15T17:01:50Z
url: https://github.com/astral-sh/ruff/pull/15470
synced_at: 2026-01-12T15:55:51Z
```

# [red-knot] Migrate `is_equivalent_to` unit tests to Markdown tests

---

_@InSyncWithFoo_

## Summary

Part of astral-sh/ruff#15397, built on top of astral-sh/ruff#15469.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-14 09:36_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-14 09:36_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-14 09:36_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-14 09:36_

---

_Label `testing` added by @AlexWaygood on 2025-01-14 09:41_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-14 09:41_

---

_Comment by @github-actions[bot] on 2025-01-14 09:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:1 on 2025-01-14 14:58_

Minor:
```suggestion
# Equivalence relation
```

---

_@sharkdp reviewed on 2025-01-14 14:58_

---

_@sharkdp approved on 2025-01-14 15:00_

Thank you. This looks good to me as soon as the conflicts have been resolved.

---

_Review request for @MichaReiser removed by @MichaReiser on 2025-01-14 15:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:2 on 2025-01-14 17:42_

```suggestion

`is_equivalent_to` implements the equivalence relation for fully static types.

```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 17:48_

I don't think this change is a good idea.

In the previous tests, we asserted the relation in both directions for every pair of types, including in the negative cases. This change doesn't give us similar testing of commutativity, particularly for the negative tests, because if one direction returns true and the other direction returns false, we'll just report false. So non-commutativity in the negative equivalence tests would have been caught as an error previously, and now would silently pass.

I also think that for clarity reasons, `knot_extensions.is_equivalent_to` should simply reflect what our `is_equivalent_to` function does, and not implicitly double-check the commutation.

I think that instead of making this change, we should explicitly test both directions in the markdown tests. If that's too painful, we should consider just not moving these to mdtests.

@sharkdp, you already reviewed this and were prepared to land it as-is; how do you feel about this?

---

_@carljm reviewed on 2025-01-14 17:50_

---

_@AlexWaygood reviewed on 2025-01-14 18:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:2 on 2025-01-14 18:03_

It would also be good to link to https://typing.readthedocs.io/en/latest/spec/glossary.html#term-equivalent and/or https://typing.readthedocs.io/en/latest/spec/concepts.html#subtype-supertype-and-type-equivalence here

---

_@InSyncWithFoo reviewed on 2025-01-14 18:15_

---

_Review comment by @InSyncWithFoo on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 18:15_

The tests for `is_disjoint_from` have the same problem, but much worse in that there are many more of them compared to that of `is_equivalent_to`.

Perhaps `is_equivalent_to`/`is_disjoint_from` should return `tuple[bool, bool]` instead?

```python
# knot_extensions.pyi
BOTH_WAYS = (True, True)
NEITHER_WAYS = (False, False)

def is_equivalent_to(...) -> tuple[bool, bool]: ...

# is_equivalent_to.md
static_assert(is_equivalent_to(A, B) == BOTH_WAYS)
static_assert(is_equivalent_to(A, B) == NEITHER_WAYS)
```

---

_@AlexWaygood reviewed on 2025-01-14 18:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 18:17_

or we could add more functions such as `knot_extensions.is_commutatively_equivalent()` and `knot_extensions.is_commutatively_disjoint()`?

(Wait for Carl's and/or David's response before going ahead and implementing this!)

---

_@carljm reviewed on 2025-01-14 18:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 18:25_

Good point about the `is_disjoint_from` tests!

I feel that `knot_extensions.is_equivalent_to` and `knot_extensions.is_disjoint_from` are semantically meaningful type operations that I'm comfortable exposing publicly to users of red-knot. These operations should simply be commutative, and if they are not, it's a bug in red-knot. Testing that we in fact implement them commutatively is a concern only of red-knot's test suite, which I don't want to expose in the public API of those methods. So I don't want them to check commutativity or return two booleans.

So if we want to move these tests to mdtest, without manual testing of commutativity, then I prefer Alex's suggestion of dedicated methods for this purpose.

My main hesitance is that these would be the first methods in the `knot_extensions` namespace that are intended solely for testing red-knot, and that I would not prefer to expose otherwise. This suggests possibly putting them in a different namespace, and possibly adding some infrastructure to only expose that namespace in red-knot tests, and not otherwise. I think this is all doable, but it's certainly more work.

Would like to hear @sharkdp take on this.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 18:29_

Oh! Last time I looked at the PR, it was still showing a version that was combined with the is-subtype-of changes, and I completely missed this change, thinking it was something from the previous changeset.


> I also think that for clarity reasons, `knot_extensions.is_equivalent_to` should simply reflect what our `is_equivalent_to` function does,

Yes, absolutely.

> Perhaps `is_equivalent_to`/`is_disjoint_from` should return `tuple[bool, bool]` instead?

> or we could add more functions such as `knot_extensions.is_commutatively_equivalent()` and `knot_extensions.is_commutatively_disjoint()`?

I'm personally not a big fan of either of these options. I would propose we add a "is_equivalent_to is commutative" section to this suite, add a couple of (non exhaustive) tests, and then rely on a `is_equivalent_to_is_commutative` property test to specifically test this property. We can add a reference to the property test in this file.

Yes, this would not be equivalent to (heh) what we had before, but I don't think that a commutativity-violation is a very likely scenario. And if so, we would hopefully catch that using the new property test.

---

_@sharkdp reviewed on 2025-01-14 18:29_

---

_@AlexWaygood reviewed on 2025-01-14 18:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 18:29_

That reminds me that I had an idea for a way we could issue a warning to users about the `knot_extensions` API relatively easily: https://github.com/astral-sh/ruff/issues/15355#issuecomment-2590811421

---

_@AlexWaygood reviewed on 2025-01-14 18:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 18:30_

I'm also happy to go with @sharkdp's suggested path here!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 18:31_

I like @sharkdp 's proposal.

---

_@carljm reviewed on 2025-01-14 18:31_

---

_@sharkdp reviewed on 2025-01-14 18:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 18:32_

Additional thought: if we had "inlining" of small functions during type inference (like pyright), we could potentially even write helpers such as `is_commutatively_equivalent` in Python, which would be neat.

---

_@carljm reviewed on 2025-01-14 18:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-14 18:53_

Either inlining (so we could just write the helpers without annotations), or explicit `TypeForm` annotations, would allow this!

---

_@carljm approved on 2025-01-14 18:55_

---

_Merged by @carljm on 2025-01-14 18:57_

---

_Closed by @carljm on 2025-01-14 18:57_

---

_Branch deleted on 2025-01-14 19:02_

---

_@sharkdp reviewed on 2025-01-15 08:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-15 08:15_

https://github.com/astral-sh/ruff/pull/15488

---

_@carljm reviewed on 2025-01-15 17:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1958 on 2025-01-15 17:01_

> or explicit `TypeForm` annotations

For posterity: @sharkdp pointed out in chat that this wouldn't be enough to propagate the right literal boolean return type back; we'd need to be able to annotate with something like `def is_commutative_equivalent_to[T, S](ty_a: TypeForm[T], ty_b: TypeForm[S]) -> And[is_equivalent_to(T, S), is_equivalent_to(S, T)]: ...` -- obviously that return type annotation is depending on several things that don't exist :)

---
