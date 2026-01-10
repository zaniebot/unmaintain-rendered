```yaml
number: 20533
title: "[ty] Change to BDD representation for constraint sets"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/consraint-bdd
created_at: 2025-09-23T13:40:44Z
updated_at: 2025-09-26T01:55:37Z
url: https://github.com/astral-sh/ruff/pull/20533
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Change to BDD representation for constraint sets

---

_Pull request opened by @dcreager on 2025-09-23 13:40_

While working on #20093, I kept running into test failures due to constraint sets not simplifying as much as they could, and therefore not being easily testable against "always true" and "always false".

This PR updates our constraint set representation to use BDDs. Because BDDs are reduced and ordered, they are canonical — equivalent boolean formulas are represented by the same interned BDD node.

That said, there is a wrinkle, in that the "variables" that we use in these BDDs — the individual constraints like `Lower ≤ T ≤ Upper` are not always independent of each other.

As an example, given types `A ≤ B ≤ C ≤ D` and a typevar `T`, the constraints `A ≤ T ≤ C` and `B ≤ T ≤ D` "overlap" — their intersection is non-empty. So we should be able to simplify

```
(A ≤ T ≤ C) ∧ (B ≤ T ≤ D) == (B ≤ T ≤ C)
```

That's not a simplification that the BDD structure can perform itself, since those three constraints are modeled as separate BDD variables, and are therefore "opaque" to the BDD algorithms.

That means we need to perform this kind of simplification ourselves. We look at pairs of constraints that appear in a BDD and see if they can be simplified relative to each other, and if so, replace the pair with the simplification. A large part of the toil of getting this PR to work was identifying all of those patterns and getting that substitution logic correct.

With this new representation, all existing tests pass, as well as some new ones that represent test failures that were occuring on #20093.

---

_Label `internal` added by @dcreager on 2025-09-23 13:40_

---

_Label `ty` added by @dcreager on 2025-09-23 13:40_

---

_Comment by @github-actions[bot] on 2025-09-23 13:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @codspeed-hq[bot] on 2025-09-23 13:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fconsraint-bdd?runnerMode=WallTime)

### Merging #20533 will **improve performances by 8.05%**

<sub>Comparing <code>dcreager/consraint-bdd</code> (bca166c) with <code>main</code> (e66a872)</sub>



### Summary

`⚡ 2` improvements  
`✅ 6` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` medium[colour-science] `` | 10.4 s | 9.6 s | +8.05% |
| ⚡ | `` small[freqtrade] `` | 8.3 s | 7.8 s | +6.22% |


---

_Comment by @codspeed-hq[bot] on 2025-09-23 13:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fconsraint-bdd?runnerMode=Instrumentation)

### Merging #20533 will **improve performances by 4.25%**

<sub>Comparing <code>dcreager/consraint-bdd</code> (bca166c) with <code>main</code> (e66a872)</sub>



### Summary

`⚡ 1` improvement  
`✅ 12` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` hydra-zen `` | 825.3 ms | 791.7 ms | +4.25% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fconsraint-bdd?runnerMode=Instrumentation&sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Comment by @github-actions[bot] on 2025-09-23 13:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 53 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/constraints.md`:602 on 2025-09-25 15:33_

These tests all failed to simplify with the old representation

---

_@dcreager reviewed on 2025-09-25 16:24_

> No memory usage changes detected

This is because #20093 is what introduces the logic to actually create non-trivial constraint sets. That is where we will see the memory increases from using salsa to intern BDD nodes and memoize the BDD operations.

---

_Marked ready for review by @dcreager on 2025-09-25 16:24_

---

_Review requested from @carljm by @dcreager on 2025-09-25 16:24_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-25 16:24_

---

_Review requested from @sharkdp by @dcreager on 2025-09-25 16:24_

---

_Renamed from "[ty] WIP: Change to BDD representation for constraint sets" to "[ty] Change to BDD representation for constraint sets" by @dcreager on 2025-09-25 16:24_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:264 on 2025-09-25 19:14_

I'm not sure I understand why `RangeConstraint` and `ConstrainedTypeVar` should be separate structs, given that the only kind of constraint a `ConstrainedTypeVar` can contain is a `RangeConstraint`. It seems to result in a lot of awkward entanglement between the types:

* We have `RangeConstraint::new_node`, which returns a `Node` containing a `ConstrainedTypeVar` containing a `RangeConstraint` -- why is this method located on `RangeConstraint`?
*  `RangeConstraint::display` has to accept an external typevar.
* Various methods (e.g. `RangeConstraint::intersect`) are not sensible unless the constraints are applied to the same typevar, but `RangeConstraint` can't validate this itself.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:562 on 2025-09-25 20:16_

```suggestion
    /// Returns the `if-then-else` of three BDDs: when `self` evaluates to `true`, it returns what
    /// `then_node` evaluates to; otherwise it returns what `else_node` evaluates to.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:587 on 2025-09-25 20:19_

```suggestion
    ) -> Self {
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:598 on 2025-09-25 20:20_

```suggestion
    fn restrict_one(self, db: &'db dyn Db, assignment: ConstraintAssignment<'db>) -> Self {
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1164 on 2025-09-25 20:22_

Why is this named `flipped()` instead of `negated()`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:795 on 2025-09-25 20:53_

Couldn't this just swap `if_true` and `if_false` instead of recursively negating them? Or would that violate some invariant of the BDD?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:631 on 2025-09-25 21:05_

When would we attempt a simplification that this validation would reject? Are some of the simplifications attempted in `InteriorNode::simplify` speculative, or possibly invalid? If so, maybe we should mark those more clearly than we do now?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:689 on 2025-09-25 21:05_

Same question as above.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:999 on 2025-09-25 21:09_

I might be misunderstanding, but it seems like the attempted simplification here doesn't match the comment? The simplification I'd expect to match the comment would be this one:

```suggestion
                // larger ∧ smaller = smaller
                simplified = simplified.substitute_intersection(
                    db,
                    larger_constraint.when_true(),
                    smaller_constraint.when_true(),
                    Node::new_satisfied_constraint(db, smaller_constraint.when_true()),
                );
```

It seems like the currently implemented simplification is `¬smaller ∧ ¬larger = ¬larger`, which is also correct, but not the same thing.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1266 on 2025-09-25 21:15_

Is it not possible to sufficiently simplify the underlying BDD so we don't need additional simplification of the `SatisfiedConstraints`?

---

_@carljm approved on 2025-09-25 21:15_

Nice! It looks like a few tests are failing with the latest commit.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1266 on 2025-09-25 21:55_

No, this is unrelated to the simplifications that we make to the BDD. Those are needed since the BDD variables are not independent of each other.

This issue is true of BDDs in general, even those with nicely independent variables. The problem is that the naive DNF of a BDD includes extra information, because it's just walking the BDD node graph. The simplest example is if you create the BDD for `x ∨ y`:

```
    x
  ₀/ \₁
  y   1
₀/ \₁
0   1
```

and then render that by turning it into DNF, you get `x ∨ (¬x ∧ y)`. The DNF formula is just a straight rendering of each path that leads to a 1 terminal. But when evaluating that formula, if `x=1`, we short circuit and don't need to evaluate the second clause. Since we can _assume_ that `x=0` if we get to the point where we need to look at the second clause, we can remove `¬x` from it. That's what this simplification is doing.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:631 on 2025-09-25 22:07_

In a way all of the simplifications are speculative — not because they sometimes don't hold, but because we don't know in advance whether a particular BDD contains the clause that we're trying to simplify. For instance, we might try to substitute `x || y` with `y` (because we determined that `y` fully contains `x`). If we do that to the function `x`, without this check, the result we get from the substitution is `y`. But the result should be `x`, since `x || y` doesn't appear in the function. In general there isn't a way to see if a BDD contains a clause like `x || y`, since those clauses aren't stored directly.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:795 on 2025-09-25 22:10_

No, if you have `a ∧ b` that would produce `a ∧ ¬b`:

```
a ∧ b =  a
       ₀/ \₁
       0   b
         ₀/ \₁
         0   1
```

Flipping the true/false edges of the root node gives:

```
         a
       ₀/ \₁
       b   0
     ₀/ \₁
     0   1
```

which is `¬a ∧ b`. Instead we want to walk the entire graph and change each terminal 0 or 1 to its opposite.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1164 on 2025-09-25 22:11_

Oh yeah good catch! I'll rename to be consistent with the other types

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:264 on 2025-09-25 22:13_

This is a holdover from the previous representation, where there were different kinds of constraint that we built up into a `ConstrainedTypeVar`. I was mildly leaning towards keeping the diff smaller where possible, but I take your point. I'll combine them.

---

_@dcreager reviewed on 2025-09-25 22:15_

> It looks like a few tests are failing with the latest commit.

That patch was me trying to fix things on #20093. I don't know that it's the right approach so I'm going to revert it here.

---

_@carljm reviewed on 2025-09-25 22:48_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:1266 on 2025-09-25 22:48_

I see, makes sense. It still somehow feels like we ought to be able to do this "on the fly" while building the DNF representation, by tracking the information we can assume and not adding clauses that can be assumed? But maybe not, or maybe doing it after-the-fact is just as good in practice.

---

_@dcreager reviewed on 2025-09-26 00:51_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1266 on 2025-09-26 00:51_

We might be able to, but this code path is only executed as part of displaying constraint sets, which only happens in our test cases, so I didn't think it was worth spending too much time optimizing.

---

_@dcreager reviewed on 2025-09-26 01:20_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:999 on 2025-09-26 01:20_

It's the comment that's wrong (which I just fixed). The `larger ∧ smaller = smaller` case holds for any two non-disjoint types, not just for strict containment, so it's handled in the stanza below.

---

_Merged by @dcreager on 2025-09-26 01:55_

---

_Closed by @dcreager on 2025-09-26 01:55_

---

_Branch deleted on 2025-09-26 01:55_

---
