```yaml
number: 20423
title: "[ty] More constraint set simplifications via simpler constraint representation"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/more-simpler
created_at: 2025-09-15T19:04:41Z
updated_at: 2025-09-16T14:05:03Z
url: https://github.com/astral-sh/ruff/pull/20423
synced_at: 2026-01-10T17:40:28Z
```

# [ty] More constraint set simplifications via simpler constraint representation

---

_Pull request opened by @dcreager on 2025-09-15 19:04_

Previously, we used a very fine-grained representation for individual constraints: each constraint was _either_ a range constraint, a not-equivalent constraint, or an incomparable constraint. These three pieces are enough to represent all of the "real" constraints we need to create — range constraints and their negation.

However, it meant that we weren't picking up as many chances to simplify constraint sets as we could. Our simplification logic depends on being able to look at _pairs_ of constraints or clauses to see if they simplify relative to each other. With our fine-grained representation, we could easily encounter situations that we should have been able to simplify, but that would require looking at three or more individual constraints.

For instance, negating a range constraint would produce:

```
¬(Base ≤ T ≤ Super) = ((T ≤ Base) ∧ (T ≠ Base)) ∨ (T ≁ Base) ∨
                      ((Super ≤ T) ∧ (T ≠ Super)) ∨ (T ≁ Super)
```

That is, `T` must be (strictly) less than `Base`, (strictly) greater than `Super`, or incomparable to either.

If we tried to union those back together, we should get `always`, since `x ∨ ¬x` should always be true, no matter what `x` is. But instead we would get:

```
(Base ≤ T ≤ Super) ∨ ((T ≤ Base) ∧ (T ≠ Base)) ∨ (T ≁ Base) ∨ ((Super ≤ T) ∧ (T ≠
 Super)) ∨ (T ≁ Super)
```

Nothing would simplify relative to each other, because we'd have to look at all five union elements to see that together they do in fact combine to `always`.

The fine-grained representation was nice, because it made it easier to [work out the math](https://dcreager.net/theory/constraints/) for intersections and unions of each kind of constraint. But being able to simplify is more important, since the example above comes up immediately in #20093 when trying to handle constrained typevars.

The fix in this PR is to go back to a more coarse-grained representation, where each individual constraint consists of a positive range (which might be `always` / `Never ≤ T ≤ object`), and zero or more negative ranges. The intuition is to think of a constraint as a region of the type space (representable as a range) with zero or more "holes" removed from it.

With this representation, negating a range constraint produces:

```
¬(Base ≤ T ≤ Super) = (always ∧ ¬(Base ≤ T ≤ Super))
```

(That looks trivial, because it is! We just move the positive range to the negative side.)

The math is not that much harder than before, because there are only three combinations to consider (each for intersection and union) — though the fact that there can be multiple holes in a constraint does require some nested loops. But the mdtest suite gives me confidence that this is not introducing any new issues, and it definitely removes a troublesome TODO.

(As an aside, this change also means that we are back to having each clause contain no more than one individual constraint for any typevar. This turned out to be important, because part of our simplification logic was also depending on that!)

---

_Label `internal` added by @dcreager on 2025-09-15 19:04_

---

_Label `ty` added by @dcreager on 2025-09-15 19:04_

---

_Comment by @github-actions[bot] on 2025-09-15 19:06_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-15 19:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @dcreager on 2025-09-15 20:09_

---

_Review requested from @carljm by @dcreager on 2025-09-15 20:09_

---

_Review requested from @AlexWaygood by @dcreager on 2025-09-15 20:09_

---

_Review requested from @sharkdp by @dcreager on 2025-09-15 20:09_

---

_Review requested from @MichaReiser by @dcreager on 2025-09-15 20:09_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:828 on 2025-09-15 20:29_

Does this comment reflect something that still needs to be looked into in this PR, or should it be removed?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:837 on 2025-09-15 20:29_

```suggestion
    /// Returns the union of this individual constraint and another, if it can be simplified to a
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:875 on 2025-09-15 20:31_

Is it worth (or have you already done) some experimentation on what performs best here? This optimizes for a single negated range -- is that the most common case? Is it better to use a regular `Vec` and optimize for zero negated ranges?

---

_@carljm approved on 2025-09-15 20:36_

Looks good. Net reduction in LOC and better simplification is a good sign.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:828 on 2025-09-16 13:03_

Yes thank you! Updated

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:875 on 2025-09-16 13:04_

I think that analysis won't be meaningful until #20093 lands (or as part of reviewing it). At the moment in real workloads we're still only creating constraint sets that are equivalent to `true` or `false`.

That said, my intuition is that this will be a common case, since there several negations in our various `has_relation_to` methods, and negating any range constraint would produce a constraint with one negated range in this field.

---

_@dcreager reviewed on 2025-09-16 13:05_

---

_Merged by @dcreager on 2025-09-16 14:05_

---

_Closed by @dcreager on 2025-09-16 14:05_

---

_Branch deleted on 2025-09-16 14:05_

---
