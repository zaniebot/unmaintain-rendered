```yaml
number: 21539
title: "[ty] Distinguish \"unconstrained\" from \"constrained to any type\""
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/unconstrained-typevar
created_at: 2025-11-20T15:00:56Z
updated_at: 2025-11-24T20:57:09Z
url: https://github.com/astral-sh/ruff/pull/21539
synced_at: 2026-01-12T15:57:26Z
```

# [ty] Distinguish "unconstrained" from "constrained to any type"

---

_@dcreager_

Before, we would collapse any constraint of the form `Never ≤ T ≤ object` down to the "always true" constraint set. This is correct in terms of BDD semantics, but loses information, since "not constraining a typevar at all" is different than "constraining a typevar to take on any type". Once we get to specialization inference, we should fall back on the typevar's default for the former, but not for the latter.

This is much easier to support now that we have a sequent map, since we need to treat `¬(Never ≤ T ≤ object)` as being impossible, and prune it when we walk through BDD paths, just like we do for other impossible combinations.

---

_Review requested from @carljm by @dcreager on 2025-11-20 15:00_

---

_Label `internal` added by @dcreager on 2025-11-20 15:00_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-20 15:00_

---

_Review requested from @sharkdp by @dcreager on 2025-11-20 15:00_

---

_Label `ty` added by @dcreager on 2025-11-20 15:00_

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 15:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-20 15:04_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:173 on 2025-11-20 18:05_

This is a weird case, and the more I think about it, the weirder it gets.

I _think_ the principles of constrained type var should probably be:

1. We should always solve to one of the constrained types (or to `Unknown` or `Any`). This is the core invariant of constrained type variables.
2. If the constraints don't allow us to narrow down to a single constrained type, but could allow multiple, we should prefer the subtype, if one constrained type is a subtype of the other. For example, for `def f[T: (Base, Sub)](x: T)` we should prefer solving `f(typed_as sub)` to `Sub` rather than `Base`, even though both solutions would be possible.
3. If multiple constrained types are possible, and neither is a subtype of the other, we should... do what? Fall back to the typevar default?

But if we agree with these principles, then we can _never_ solve this constrained type var to `Base`, because any set of constraints that would allow us to solve to `Base` would also allow solving to `Any`, therefore we can't narrow it down to one of the choices.

[mypy](https://mypy-play.net/?mypy=latest&python=3.13&gist=25de03cec23e19bffce364d7b02e7845) seems to just always solve such a typevar to `Any`. (Mypy doesn't support typevar defaults, nor distinguish `Any` vs `Unknown`, so I can't really experiment with how it handles fallback-to-default.)

[pyright](https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.13&strict=true&reportUnreachable=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCCKcAUCQMYA2AhgM61QDKArggKYgBcJUvUCdWmSqCoAITpsAFC3YgAlNz79BwmvSbMARlIm02innwH01ogKooQbGjDYATJcdUl7bYFGABtACqcoupIANITE8lAAvOKSALpSAB7%2BPlAAPlAAcmAobJEZWQZQALQAfFB%2BRrxIHvGYDChg%2BJnZTsq81jDMIChQ8WTKINRI%2BlAASswoMFhsAKIg4CBSAET17IvyZG4eAPpSYFoAVv57%2B2zkMCG0rP6yHCFakv56bBfa19oh41xQlta2Dob9NgANzY1EoW3g7CkwF2B3k60BILBEMQ0hhlwQ8IqUGsSPBkLRUnu%2BixiNB%2BNR0Kkly0pL4uPJKKhMKxQA) also always solves to `Any`, unless the typevar is totally unconstrained, in which case it falls back to the typevar default (which I've explicitly specified to be `Base` in this example, but could also just implicitly be `Unknown`.) I'm not sure how pyright decides that `f(base)` or `f(sub)` should be `Any` rather than "the typevar default", given that those calls are compatible with either constrained type.

[pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN0BBdKQA0dSjABuMVFAD6TYjAA66VQGMoqOHDoBlAK5LKiVXXN1i2uKo1addAELaYACkPGAlKfQXL123RNa30DbFdnOBhvMwsrHUDghwBVdAktBhhMHz94mzUsGDA6MABtABVEOgiXMSFSTzoAXicXAF1XfCryugAfOgA5XHQYZsHh6LoAWgA%2BOkrY8whi-E5ddFx%2BIZGcvwsJBgNKX3xAv0pUCCi6ACUDdAYuGABRSmpKV2UQDaUvz0DMEU6HJXLhsAArKpg8EwdQMMRwIxVDwwShibAuKqRGAIsLIsJie4mOipdKoTKYGK%2BfZSGTyRRuMCgiGef7U8wSaSyBTMRmuRHEVmLcS07kM1xMjFRIXskVc%2Bm8iX8sIy86ihVKJUykAiEAGR5QOAkciIEAAYhJBogTBK9zhEGG%2BVUgOKYF4NHJcnQBho2FRnSqrAYjVmdDgDBMwoOR18YC%2BAx9fuJwHwAF8vqodSAyBIwFBSIQGLQoBQLQAFUi5-NhjA4Ah0dTDSBsI7kh3oQiqC16GCjAAWDAYxDgiAA9KOc0V84ReGxRzB0KPMLh1HBR430M3W49hqOSrw6KhJJctNhYA2mxAWxcd75cMRb0bVGQGH3hlNpJQ4O2xl8AMyEAAjH%2BGboCAqa6qg9rSAAYtAMAUGgWB4EQZDgUAA) does the same thing you originally did here (always solve to a fully static type), which I'm pretty sure is wrong. It fails both the promises of a constrained type var and of the gradual guarantee, and I suspect will cause false positive errors in practice (the fact that pyright and mypy both prefer solving to `Any` strengthens this intuition.) If the `Any` constrained type means "there is really some static type here, but we don't know what it is" (imagine its an `Unknown` from a failed import of a type), then it's wrong to solve to some arbitrary fully static type, because we don't know that's what the constrained type was "supposed" to be - and it could cause a lot of cascading errors.

(I think it's fine to punt all of this, but I will suggest some different TODOs below based on this analysis.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:176 on 2025-11-20 18:34_

I would expect this to solve to the typevar default, in this case `Unknown`. What information allows us to pick `Base` over `Any` here? (If we are picking between `Base` and `Any`, I think `Any` is probably the better choice in practice, and is what mypy/pyright choose, but I'm not sure what heuristic they are using.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:179 on 2025-11-20 18:37_

Here I think either `Any` or `Unknown` (typevar default) would be reasonable choices?

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:185 on 2025-11-20 18:52_

Pyright and mypy would also solve this to `Any`. Similar to above, I'm not sure why we'd pick `Base` over `Any` here as a solution.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:192 on 2025-11-20 18:52_

Same as above cases, I think this should also solve to `Any` or `Unknown`

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:199 on 2025-11-20 18:52_

And again here.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/constraints.rs`:2265 on 2025-11-20 19:52_

Mostly just curiosity: is there an important reason why we frame this as `¬C₁ → false` rather than `C₁ → true`? Are these different in some way? The prose here suggests not.

---

_@carljm approved on 2025-11-20 19:53_

Awesome!

---

_@dcreager reviewed on 2025-11-20 21:25_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:2265 on 2025-11-20 21:25_

"`C₁` is always true" is `true → C₁`, not `C₁ → true`. But given that change, you're right that `true → C₁` and `¬C₁ → false` are the same thing. One focuses on what is always true, and the other focuses on what's impossible. The focus of this data structure is to track situations that are impossible, so I thought it would be best to frame it with `false` on the rhs.

Though I guess that means I could go further and rewrite some the examples below ­— for instance `C₁ ∧ C₂ → D` and `C₁ ∧ C₂ ∧ ¬D → false` are equivalent in exactly the same way!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-11-20 21:26_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:173 on 2025-11-24 19:12_

> But if we agree with these principles, then we can _never_ solve this constrained type var to `Base`, because any set of constraints that would allow us to solve to `Base` would also allow solving to `Any`, therefore we can't narrow it down to one of the choices.

Is it worth trying to consider which materializations of `Any` are valid specializations? If `Base` is a valid specialization but no subtypes are (if that were to violate some other requirement of the specialization), then the `Any` could not materialize to anything that would take precedence according to your rule (2).

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:176 on 2025-11-24 19:27_

> What information allows us to pick `Base` over `Any` here?

This is a side effect of the change I added from this discussion https://github.com/astral-sh/ruff/pull/21414#discussion_r2539830595. If there are multiple solutions, my original plan was to produce the union of them. But I don't think that's correct, so instead we either:

- find a single specialization that satisfies all of the solutions, or
- return `None` to indicate an ambiguous result.

Here, the constraint of `Any` can materialize to any type `T = *`, and still satisfy `ConstraintSet.always()`. The constraint of `Base` only satisfies the constraint set when `T = Base`. The intersection of those two solutions is `T = Base`, and so that's what we return.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:179 on 2025-11-24 19:59_

Yep, this one already has `Any` as the TODO

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:185 on 2025-11-24 20:00_

Added TODO

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:192 on 2025-11-24 20:01_

Added TODO

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:199 on 2025-11-24 20:01_

Added TODO

---

_@dcreager reviewed on 2025-11-24 20:03_

---

_@dcreager reviewed on 2025-11-24 20:20_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:179 on 2025-11-24 20:20_

(This one I think the right TODO is `Any` not `Unknown` because here we are now explicitly saying that `T` can be any type, as opposed to not having said anything about what `T` can be. The latter clearly should fall back on the default. The former I don't think should.)

---

_Merged by @dcreager on 2025-11-24 20:23_

---

_Closed by @dcreager on 2025-11-24 20:23_

---

_Branch deleted on 2025-11-24 20:23_

---

_@carljm reviewed on 2025-11-24 20:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:173 on 2025-11-24 20:57_

Yes, that makes sense! So if we have a lower bound of `Base` (e.g. because we got a call that provided an instance of `Base` as argument for this typevar), then we can solve to `Base` because it must be the most precise option.

---
