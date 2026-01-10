```yaml
number: 21414
title: "[ty] Create a specialization from a constraint set"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/specialize-from-constraints
created_at: 2025-11-12T21:06:18Z
updated_at: 2025-11-19T19:20:35Z
url: https://github.com/astral-sh/ruff/pull/21414
synced_at: 2026-01-10T16:48:01Z
```

# [ty] Create a specialization from a constraint set

---

_Pull request opened by @dcreager on 2025-11-12 21:06_

This patch lets us create specializations from a constraint set. The constraint encodes the restrictions on which types each typevar can specialize to. Given a generic context and a constraint set, we iterate through all of the generic context's typevars. For each typevar, we abstract the constraint set so that it only mentions the typevar in question (propagating derived facts if needed). We then find the "best representative type" for the typevar given the abstracted constraint set.

When considering the BDD structure of the abstracted constraint set, each path from the BDD root to the `true` terminal represents one way that the constraint set can be satisfied. (This is also one of the clauses in the DNF representation of the constraint set's boolean formula.) Each of those paths is the conjunction of the individual constraints of each internal node that we traverse as we walk that path, giving a single lower/upper bound for the path. We use the upper bound as the "best" (i.e. "closest to `object`") type for that path.

If there are multiple paths in the BDD, they technically represent independent possible specializations. If there's a single specialization that satisfies all of them, we will return that as the specialization. If not, then the constraint set is ambiguous. (This happens most often with constrained typevars.) We could in the future turn _each_ of the paths into separate specializations, but it's not clear what we would do with that, so instead we just report the ambiguity as a specialization failure.


---

_Label `internal` added by @dcreager on 2025-11-12 21:06_

---

_Label `ty` added by @dcreager on 2025-11-12 21:06_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 21:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-12 21:09_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @dcreager on 2025-11-18 01:40_

---

_Review requested from @carljm by @dcreager on 2025-11-18 01:40_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-18 01:40_

---

_Review requested from @sharkdp by @dcreager on 2025-11-18 01:40_

---

_Review requested from @MichaReiser by @dcreager on 2025-11-18 01:40_

---

_Review requested from @ibraheemdev by @MichaReiser on 2025-11-18 17:33_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:132 on 2025-11-18 20:54_

Doesn't this result violate "must specialize to one of those specific types"?

I feel like we discussed this case earlier and looked at what other type checkers do. I think they rather infer `Unknown` if they have no basis for picking one of the constrained types? At least that's what pyright seems to do: https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.12&reportUnreachable=true&code=CYUwZgBGDaAqBcEAUBLAdgFwDQQM4YCcBKAXSQA9FYIAfCAOQHs0QiIBaAPggQCgIBEFJHJDcENIwwNmIeP0GKCIDAFcCaCOQUCCAQxS4QPAJ4AHEAFECBRgSQAiAIIBjNXoA2HkzJZiIAO62aADmDkS8vMoAbiCeAPoY5iBIYEhMLEQRvEA

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:160 on 2025-11-18 20:57_

Yes, but shouldn't we still solve to the gradual type that is one of the constraints? E.g. see https://pyright-play.net/?pyrightVersion=1.1.405&pythonVersion=3.12&reportUnreachable=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCCKcAUCQCYCmwUwA2gCoBcUAFKjADSHECUAuqwAeLBlF5QAtAD4ozElEVQQlGAFcQKKELIqAbpQCGAGwD68BJVbBWAImBgwt3rzJA

E.g. in every solution in the first section below that is not `Base`, I would expect to solve to `Any`, rather than any of the materializations of it. I suspect this will cause false positives and violate the gradual guarantee otherwise?

It seems like the current approach takes as a requirement that we always solve to a fully static type, but I'm not sure that is what we should do, in the case of constrained type variables.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:304 on 2025-11-18 21:02_

🎉 

---

_@carljm approved on 2025-11-18 21:12_

This is lovely!

My only questions fall into the category of "constrained typevars are weird" and can be punted to later.

---

_@dcreager reviewed on 2025-11-18 22:33_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:132 on 2025-11-18 22:33_

Ah yes, come to think of it, this is where we talked way back about how we might get more than one possible specialization. That seems like the better way to model this — multiple satisfiable paths in the BDD leads to an ambiguous result (and we can give you all of them if you want them), not to the union of those results.

---

_@carljm reviewed on 2025-11-18 22:37_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:132 on 2025-11-18 22:37_

Well I would distinguish between constrained typevars and others here. For bounded or unbounded typevars, I like the union type resulting from multiple ORed constraints! I don't think that should go to Unknown. But I do think that if we can't pick one of the constrained types for a constrained typevar, that should go to Unknown.

---

_@dcreager reviewed on 2025-11-19 13:55_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:132 on 2025-11-19 13:55_

I think the unbounded example was incorrect, though. The constraint set is `T ≤ int ∨ T ≤ str`, but `int | str` is not a subtype of `int` nor of `str`, so `T = int | str` is not a valid specialization. With the change that I'm introducing to handle constrained typevars, I now get `T = Never` as the specialization, which I think is the only correct one.

---

_@dcreager reviewed on 2025-11-19 13:57_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:132 on 2025-11-19 13:57_

(This is the unbounded example:)

https://github.com/astral-sh/ruff/blob/4327616df4480d11b2a2373cbd3968a4c304dc5c/crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md?plain=1#L44-L45

---

_@dcreager reviewed on 2025-11-19 15:32_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:160 on 2025-11-19 15:32_

> It seems like the current approach takes as a requirement that we always solve to a fully static type, but I'm not sure that is what we should do, in the case of constrained type variables.

Hmm, that is a good point. I would like to put in TODOs and handle that as follow-on work.

---

_@carljm reviewed on 2025-11-19 16:38_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:132 on 2025-11-19 16:38_

> I think the unbounded example was incorrect, though. The constraint set is `T ≤ int ∨ T ≤ str`, but `int | str` is not a subtype of `int` nor of `str`, so `T = int | str` is not a valid specialization.

Oh yes, of course this is right.

> With the change that I'm introducing to handle constrained typevars, I now get `T = Never` as the specialization, which I think is the only correct one.

Hmm, well it's clearly not the only correct solution, and definitely not the "closest to `object` one"? Aren't `T = int` and `T = str` both valid solutions for the constraint set `T ≤ int ∨ T ≤ str`? (It's an OR, not an AND.) `Never` is a valid solution but doesn't seem like a useful one.

But now I see why it might make sense to specialize to `Unknown` in this case, too. We have two equally-good specialization options and no reason to pick one over the other, and either one we pick could lead to false positives.


---

_@dcreager reviewed on 2025-11-19 17:00_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:132 on 2025-11-19 17:00_

> But now I see why it might make sense to specialize to `Unknown` in this case, too. We have two equally-good specialization options and no reason to pick one over the other, and either one we pick could lead to false positives.

This maybe brings up a different question, which is the difference between a _useful_ solution and a _valid_ one. As I've written it, we do return a specialization for the unbounded example ­— `T = Never`. (It's the only _single_ specialization that satisfies all branches of the constraint set union.) That's different than the constrained typevar case, where we return `None`. So we wouldn't fall back on `T = Unknown` for the unbounded case.

This has come up in some of the other PRs, as well. Is `T = Never` ever a specialization we'd want to return? I've been arguing that if you don't want that as a specialization, you should be adding `T ≠ Never` as an additional constraint in your constraint set, and that the constraint set algorithms shouldn't pre-suppose that.

---

_@carljm reviewed on 2025-11-19 19:00_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/specialize_constrained.md`:132 on 2025-11-19 19:00_

I'm honestly not sure on these questions without tying it back to real Python examples. It's not clear to me in what real-world scenarios we'd end up with a constraint set like `T ≤ int ∨ T ≤ str`. So maybe it's better to leave this for a future PR with ecosystem results to look at.

---

_Merged by @dcreager on 2025-11-19 19:20_

---

_Closed by @dcreager on 2025-11-19 19:20_

---

_Branch deleted on 2025-11-19 19:20_

---
