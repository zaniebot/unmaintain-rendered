```yaml
number: 20319
title: "[ty] Add mdtests that exercise constraint sets"
type: pull_request
state: merged
author: dcreager
labels: []
assignees: []
merged: true
base: main
head: dcreager/constraint-mdtest
created_at: 2025-09-09T18:09:33Z
updated_at: 2025-09-10T17:22:21Z
url: https://github.com/astral-sh/ruff/pull/20319
synced_at: 2026-01-10T17:46:22Z
```

# [ty] Add mdtests that exercise constraint sets

---

_Pull request opened by @dcreager on 2025-09-09 18:09_

This PR adds a new `ty_extensions.ConstraintSet` class, which is used to expose constraint sets to our mdtest framework. This lets us write a large collection of unit tests that exercise the invariants and rewrite rules of our constraint set implementation.

As part of this, `is_assignable_to` and friends are updated to return a `ConstraintSet` instead of a `bool`, and we implement `ConstraintSet.__bool__` to return when a constraint set is always satisfied. That lets us still use `static_assert(is_assignable_to(...))`, since the assertion will coerce the constraint set to a bool, and also lets us `reveal_type(is_assignable_to(...))` to see more detail about whether/when the two types are assignable. That lets us get rid of `reveal_when_assignable_to` and friends, since they are now redundant with the expanded capabilities of `is_assignable_to`.

---

_Comment by @github-actions[bot] on 2025-09-09 18:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-09 18:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @dcreager on 2025-09-10 01:03_

---

_Review requested from @carljm by @dcreager on 2025-09-10 01:03_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-10 01:03_

---

_Review requested from @sharkdp by @dcreager on 2025-09-10 01:03_

---

_Review requested from @MichaReiser by @dcreager on 2025-09-10 01:03_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variables.md`:140 on 2025-09-10 01:17_

These get quite a bit more verbose. Almost tempted to say we should keep `reveal_when_assignable_to` as a wrapper since it's so much more concise? But I also like reducing the API surface area of `ty_extensions`.

Since all of these tests reveal `always` or `never`, could they just use `static_assert(is_assignable_to(...))` and `static_assert(not is_assignable_to(...))` instead? Or is the idea that you want to verify we are returning `always` or `never`, rather than some set of conditions which we would convert to `false` along with `never`?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:143 on 2025-09-10 01:45_

This seems arbitrary (the choice of top materialization), which I think is OK because I think it only ever matters when using `ty_extensions.not_equivalent_constraint`? Because otherwise a not-equivalent constraint would only ever be generated via negation of a range constraint, right? And the range constraint will already have fully-static upper and lower bounds, resulting in a fully-static hole from the start.

If this is all correct, would it make sense to clarify some of this more here?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:182 on 2025-09-10 01:46_

Same comment as above.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:503 on 2025-09-10 01:57_

Why doesn't this union simplify to `SubSub <= T <= Super`?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:510 on 2025-09-10 01:58_

Similarly it seems to me this should simplify to `Sub <= T <= Super`

---

_@carljm approved on 2025-09-10 02:01_

Nice!

---

_@dcreager reviewed on 2025-09-10 12:46_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/pep695/variables.md`:140 on 2025-09-10 12:46_

I originally had left out the `ty_extensions.ConstraintSet` part, which would have left the reveal renderings exactly as they were before. But it seemed like we are trying to have the `reveal_type` output include the type name.

> Or is the idea that you want to verify we are returning `always` or `never`, rather than some set of conditions which we would convert to `false` along with `never`?

This, and more importantly, in #20093 some of these start returning constraint sets that are only sometimes satisfiable.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:503 on 2025-09-10 13:13_

There can be types that satisfy `SubSub ≤ T ≤ Super` but which aren't comparable to `Sub` or `Base`, and which therefore shouldn't be included in the union.

So with just set theory, if the possible values are 0, 1, and 2, you get a type lattice of

```
   012
01  02  12
 0   1   2
     ∅
```

Then we might have

```
SubSub = ∅
Sub    = 1
Base   = 12
Super  = 012

SubSub ≤ T ≤ Base  = {∅, 1, 2, 12}
Sub ≤ T ≤ Super    = {1, 01, 12, 012}
```

The union should be

```
(SubSub ≤ T ≤ Base) ∪ (Sub ≤ T ≤ Super) = {∅, 1, 2, 01, 12, 012}
```

but the simplification you suggest would be

```
SubSub ≤ T ≤ Super = {∅, 0, 1, 2, 01, 02, 12, 012}
```

`0` and `02` are included in the simplification but not in the actual union.

Translating that into Python, an example problem type would be `Super \ Sub*`, where `Sub*` indicates instances of just `Sub`, but not any of its subclasses. (So in English, the problem type is "instances of `Super`, or any subclass of `Super` other than `Sub`". That type is not in `SubSub ≤ T ≤ Base`, since it includes `Super`, which is outside the range. It's also not in `Sub ≤ T ≤ Super`, because it does not include `Sub`. But it is in `SubSub ≤ T ≤ Super`.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:143 on 2025-09-10 13:18_

> This seems arbitrary (the choice of top materialization),

Yeah, I wasn't sure which materialization to use here, or if the correct thing to do was to make a range from the bottom materialization to the top, and then negate that. (Even if that's more correct, I'm not at all sure what the analogue would be for incomparable below.)

But as you point out, we never create these directly, so it should be moot. I'll update the `ty_extensions` constructor to _require_ fully static inputs, and add commentary here explaining why.

---

_@dcreager reviewed on 2025-09-10 13:18_

---

_@carljm reviewed on 2025-09-10 16:11_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:503 on 2025-09-10 16:11_

Makes sense, thank you! Might be worth recording a short version of this as a comment?

---

_@carljm approved on 2025-09-10 16:12_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:503 on 2025-09-10 17:05_

Done

---

_@dcreager reviewed on 2025-09-10 17:05_

---

_Merged by @dcreager on 2025-09-10 17:22_

---

_Closed by @dcreager on 2025-09-10 17:22_

---

_Branch deleted on 2025-09-10 17:22_

---
