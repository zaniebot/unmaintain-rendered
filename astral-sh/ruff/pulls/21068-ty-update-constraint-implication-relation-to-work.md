```yaml
number: 21068
title: "[ty] Update \"constraint implication\" relation to work on constraints between two typevars"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/subtype-given-typevars
created_at: 2025-10-24T17:49:36Z
updated_at: 2025-10-30T20:11:07Z
url: https://github.com/astral-sh/ruff/pull/21068
synced_at: 2026-01-12T15:57:15Z
```

# [ty] Update "constraint implication" relation to work on constraints between two typevars

---

_@dcreager_

It's possible for a constraint to mention two typevars. For instance, in the body of

```py
def f[S: int, T: S](): ...
```

the baseline constraint set would be `(T ≤ S) ∧ (S ≤ int)`. That is, `S` must specialize to some subtype of `int`, and `T` must specialize to a subtype of the type that `S` specializes to.

This PR updates the new "constraint implication" relationship from #21010 to work on these kinds of constraint sets. For instance, in the example above, we should be able to see that `T ≤ int` must always hold:

```py
def f[S, T]():
    constraints = ConstraintSet.range(Never, S, int) & ConstraintSet.range(Never, T, S)
    static_assert(constraints.implies_subtype_of(T, int))  # now succeeds!
```

This did not require major changes to the implementation of `implies_subtype_of`. That method already relies on how our `simplify` and `domain` methods expand a constraint set to include the transitive closure of the constraints that it mentions, and to mark certain combinations of constraints as impossible. Previously, that transitive closure logic only looked at pairs of constraints that constrain the same typevar. (For instance, to notice that `(T ≤ bool) ∧ ¬(T ≤ int)` is impossible.)

Now we also look at pairs of constraints that constraint different typevars, if one of the constraints is bound by the other — that is, pairs of the form `T ≤ S` and `S ≤ something`, or `S ≤ T` and `something ≤ S`. In those cases, transitivity lets us add a new derived constraint that `T ≤ something` or `something ≤ T`, respectively. Having done that, our existing `implies_subtype_of` logic finds and takes into account that derived constraint.

---

_Label `internal` added by @dcreager on 2025-10-24 17:49_

---

_Label `ty` added by @dcreager on 2025-10-24 17:49_

---

_Comment by @github-actions[bot] on 2025-10-24 20:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-24 20:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @dcreager on 2025-10-28 16:55_

---

_Review requested from @carljm by @dcreager on 2025-10-28 16:55_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-28 16:55_

---

_Review requested from @sharkdp by @dcreager on 2025-10-28 16:55_

---

_Review requested from @MichaReiser by @dcreager on 2025-10-28 18:32_

---

_Comment by @codspeed-hq[bot] on 2025-10-29 14:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fsubtype-given-typevars?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21068 will **not alter performance**

<sub>Comparing <code>dcreager/subtype-given-typevars</code> (c054447) with <code>main</code> (d0aebaa)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fsubtype-given-typevars?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:188 on 2025-10-29 21:10_

More an understand question for me. What's the difference between `Never` and passing `U`? 

---

_@MichaReiser reviewed on 2025-10-29 21:10_

---

_@MichaReiser approved on 2025-10-29 21:13_

Makes sense to me

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/implies_subtype_of.md`:188 on 2025-10-30 18:15_

`range(Never, T, U)` is `Never ≤ T ≤ U`, which simplifies to `T ≤ U` since every type is a supertype of `Never`.

`range(U, T, U)` is `U ≤ T ≤ U`, which simplifies to `T = U`.

So the first one is "`T` must be a subtype of `U`", and the second one is "`T` must be exactly the same type as `U`"

---

_@dcreager reviewed on 2025-10-30 18:15_

---

_@carljm approved on 2025-10-30 19:13_

---

_Merged by @dcreager on 2025-10-30 20:11_

---

_Closed by @dcreager on 2025-10-30 20:11_

---

_Branch deleted on 2025-10-30 20:11_

---
