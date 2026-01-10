```yaml
number: 21463
title: "[ty] Better handling of \"derived information\" in constraint sets"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/ordering-bug
created_at: 2025-11-14T22:43:41Z
updated_at: 2025-11-18T17:02:27Z
url: https://github.com/astral-sh/ruff/pull/21463
synced_at: 2026-01-10T16:53:56Z
```

# [ty] Better handling of "derived information" in constraint sets

---

_Pull request opened by @dcreager on 2025-11-14 22:43_

This saga began with a regression in how we handle constraint sets where a typevar is constrained by another typevar, which #21068 first added support for:

```py
def mutually_constrained[T, U]():
    # If [T = U ∧ U ≤ int], then [T ≤ int] must be true as well.
    given_int = ConstraintSet.range(U, T, U) & ConstraintSet.range(Never, U, int)
    static_assert(given_int.implies_subtype_of(T, int))
```

While working on #21414, I saw a regression in this test, which was strange, since that PR has nothing to do with this logic! The issue is that something in that PR made us instantiate the typevars `T` and `U` in a different order, giving them differently ordered salsa IDs. And importantly, we use these salsa IDs to define the variable ordering that is used in our constraint set BDDs. This showed that our "mutually constrained" logic only worked for one of the two possible orderings. (We can — and now do — test this in a brute-force way by copy/pasting the test with both typevar orderings.)

The underlying bug was in our `ConstraintSet::simplify_and_domain` method. It would correctly detect `(U ≤ T ≤ U) ∧ (U ≤ int)`, because those two constraints affect different typevars, and from that, infer `T ≤ int`. But it wouldn't detect the equivalent pattern in `(T ≤ U ≤ T) ∧ (U ≤ int)`, since those constraints affect the same typevar. At first I tried adding that as yet more pattern-match logic in the ever-growing `simplify_and_domain` method. But doing so caused other tests to start failing.

At that point, I realized that `simplify_and_domain` had gotten to the point where it was trying to do too much, and for conflicting consumers. It was first written as part of our display logic, where the goal is to remove redundant information from a BDD to make its string rendering simpler. But we also started using it to add "derived facts" to a BDD. A derived fact is a constraint that doesn't appear in the BDD directly, but which we can still infer to be true. Our failing test relies on derived facts — being able to infer that `T ≤ int` even though that particular constraint doesn't appear in the original BDD. Before, `simplify_and_domain` would trace through all of the constraints in a BDD, figure out the full set of derived facts, and _add those derived facts_ to the BDD structure. This is brittle, because those derived facts are not universally true! In our example, `T ≤ int` only holds along the BDD paths where both `T = U` and `U ≤ int`. Other paths will test the negations of those constraints, and on those, we _shouldn't_ infer `T ≤ int`. In theory it's possible (and we were trying) to use BDD operators to express that dependency...but that runs afoul of how we were simultaneously trying to _remove_ information to make our displays simpler.

So, I ripped off the band-aid. `simplify_and_domain` is now _only_ used for display purposes. I have not touched it at all, except to remove some logic that is definitely not used by our `Display` impl. Otherwise, I did not want to touch that house of cards for now, since the display logic is not load-bearing for any type inference logic.

For all non-display callers, we have a new **_sequent map_** data type, which tracks exactly the same derived information. But it does so (a) without trying to remove anything from the BDD, and (b) lazily, without updating the BDD structure.

So the end result is that all of the tests (including the new regressions) pass, via a more efficient (and hopefully better structured/documented) implementation, at the cost of hanging onto a pile of display-related tech debt that we'll want to clean up at some point.

---

_Label `internal` added by @dcreager on 2025-11-14 22:43_

---

_Label `ty` added by @dcreager on 2025-11-14 22:43_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 23:01_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 23:02_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Marked ready for review by @dcreager on 2025-11-14 23:26_

---

_Review requested from @carljm by @dcreager on 2025-11-14 23:26_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-14 23:26_

---

_Review requested from @sharkdp by @dcreager on 2025-11-14 23:26_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/constraints.rs`:2119 on 2025-11-17 13:10_

Does this case (and the one below) need a `!upper.is_typevar()` guard? Otherwise, the `S ≤ T ≤ S` case that you mentioned above would be handled here, and you would get a post constraint of `Never ≤ S ≤ S`, which is also vacuously true.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/constraints.rs`:1997 on 2025-11-17 13:12_

Just curious about the terminology here. The special thing about the [equivalent term in logic](https://en.wikipedia.org/wiki/Sequent) seems to be that there can be multiple "consequents" on the right hand side. But none of the cases listed below seem to have that form?

---

_@sharkdp reviewed on 2025-11-17 13:13_

Not a full review yet, but I might not have enough time to review the rest today, so sending two initial questions.

---

_@dcreager reviewed on 2025-11-17 16:48_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1997 on 2025-11-17 16:48_

The representation does allow for multiple consequents (there is a `Vec` in the `single_implications` and `pair_implications` maps). But we never construct one directly, since each sequent that we construct corresponds to an implication where we infer one fact from something else we already know. But when that single-consequent sequent is folded in to the rest of the `SequentMap`, it might get combined with other sequents that have the same lhs, producing a combined sequent with multiple consequents. (And the consequents of a sequent are ORed, so if there's multiple derived facts that we can infer from the same set of given facts, we can choose which of them we need to use.)

---

_@dcreager reviewed on 2025-11-17 19:18_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:2119 on 2025-11-17 19:18_

Yes that's a great catch! I think I can solve this instead by moving the conditional inside the match arm body above

---

_Comment by @codspeed-hq[bot] on 2025-11-17 19:33_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fordering-bug?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21463 will **not alter performance**

<sub>Comparing <code>dcreager/ordering-bug</code> (a87c3d9) with <code>main</code> (e4a32ba)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fordering-bug?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@dcreager reviewed on 2025-11-17 19:58_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:2119 on 2025-11-17 19:58_

(done)

---

_@sharkdp approved on 2025-11-18 15:24_

Fantastic in-code documentation and writeup. I have the feeling that I understood roughly what is going on and it seems to make a lot of sense to me, but I'll also admit that my understanding is most certainly not deep enough to provide any meaningful input on the overall design. But all of the individual pieces seem consistent to me.

---

_Merged by @dcreager on 2025-11-18 17:02_

---

_Closed by @dcreager on 2025-11-18 17:02_

---

_Branch deleted on 2025-11-18 17:02_

---
