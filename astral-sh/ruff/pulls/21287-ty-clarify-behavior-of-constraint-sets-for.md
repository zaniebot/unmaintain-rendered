```yaml
number: 21287
title: "[ty] Clarify behavior of constraint sets for gradual upper bounds and constraints"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/dynamic-bounds
created_at: 2025-11-05T19:51:17Z
updated_at: 2025-11-07T19:01:41Z
url: https://github.com/astral-sh/ruff/pull/21287
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Clarify behavior of constraint sets for gradual upper bounds and constraints

---

_Pull request opened by @dcreager on 2025-11-05 19:51_

When checking whether a constraint set is satisfied, if a typevar has a non-fully-static upper bound or constraint, we are free to choose any materialization that makes the check succeed.

In non-inferable positions, we have to show that the constraint set is satisfied for all valid specializations, so it's best to choose the most restrictive materialization, since that minimizes the set of valid specializations that have to pass.

In inferable positions, we only have to show that the constraint set is satisfied for _some_ valid specializations, so it's best to choose the most permissive materialization, since that maximizes our chances of finding a specialization that passes.



---

_Review requested from @carljm by @dcreager on 2025-11-05 19:51_

---

_Review requested from @AlexWaygood by @dcreager on 2025-11-05 19:51_

---

_Review requested from @sharkdp by @dcreager on 2025-11-05 19:51_

---

_Label `internal` added by @dcreager on 2025-11-05 19:51_

---

_Label `ty` added by @dcreager on 2025-11-05 19:51_

---

_Comment by @github-actions[bot] on 2025-11-05 19:53_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:148 on 2025-11-05 19:54_

I think that only holds true if the upper bound of a typevar is a gradual specialization of an _invariant_ type? The bottom materialization of `list[Any]` is `Never`, but the bottom materialization of `Sequence[Any]` is `Sequence[Never]`, since `Sequence[Never]` is a subtype of `Sequence[int]` and `Sequence[str]`, and is still an inhabited type (`[]`, `()` can both inhabit `Sequence[Never]`, for example). Similarly for contravariance, the bottom materialization of `Callable[[Any], int]` is `Callable[[object], int]`.

---

_Comment by @github-actions[bot] on 2025-11-05 19:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood reviewed on 2025-11-05 19:56_

---

_@dcreager reviewed on 2025-11-05 20:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:148 on 2025-11-05 20:08_

Which part doesn't hold true? This might be poor wording on my part — I am not trying to claim that the bottom materialization is always `Never`. I am (attempting to) say that for `T: Any`, we pick `T: Never`, but it's more precise to say that we pick the bottom specialization, because for `T: list[Any]`, we pick `T: Bottom[list[Any]]`.

---

_@sharkdp reviewed on 2025-11-05 20:13_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:148 on 2025-11-05 20:13_

> might be poor wording on my part

I think I understood it the way you explained in your comment now, but I also stumbled over it.

---

_@AlexWaygood reviewed on 2025-11-05 20:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:148 on 2025-11-05 20:13_

> This might be poor wording on my part — I am not trying to claim that the bottom materialization is always `Never`.

Ah, okay -- yes, I think the wording can be improved there a little!

---

_@dcreager reviewed on 2025-11-05 20:55_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:148 on 2025-11-05 20:55_

Reworded this, lmkwyt

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:163 on 2025-11-06 13:07_

Maybe just for the first of these: "If we choose Super as the materialization *for the upper bound on T* …"?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:166 on 2025-11-06 13:14_

I was slightly confused by the description here and above, because it sounded like there would be any proof here for the statement that `T = Never` is the only valid specialization. I think I would have expected something like:

A non-inferable `T: Any` satisfies the constraint set `Never ≤ T ≤ Super`, because we are free to assume any materialization for the upper bound and can therefore chose `T: Never`.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:188 on 2025-11-06 13:18_

Would it be worth demonstrating that this is only true for `UnrelatedFinal`, but not for a general `Unrelated` class? I would find that more useful compared to checking `Super`, `Sub` and `Base` above, which do not change anything?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:216 on 2025-11-06 13:20_

Minor: I think it should either say "if we choose `Super` as the materialization of the `Any` in `list[Any]`", or just "if we choose `list[Super]` as the materialization of the upper bound"?

---

_@sharkdp approved on 2025-11-06 13:22_

Very nice, thank you!

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:163 on 2025-11-07 18:13_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:216 on 2025-11-07 18:17_

I was trying to be careful to use "materialization" to only refer to what the `Any` turns into, even when it's part of a larger type. But `list[Any]` is a type that can be materialized, so maybe that is more confusing than helpful. Updated to line up with the changes suggested above

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:188 on 2025-11-07 18:22_

I think this will hold for `UnrelatedButNotFinal` as well — and for any type at all, actually. It's the `∧ T ≠ Never` part in the constraint set that causes this to fail — `T = Never` is a valid specialization no matter what materialization we choose, and the extra clause in the constraint set prohibits that specialization no matter what else appears in the constraint set.

(That said, I did follow your suggestion to remove the redundant checks above for `Super` and `Base`)

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_properties/satisfied_by_all_typevars.md`:166 on 2025-11-07 18:25_

Reworded

---

_@dcreager reviewed on 2025-11-07 18:35_

---

_Merged by @dcreager on 2025-11-07 19:01_

---

_Closed by @dcreager on 2025-11-07 19:01_

---

_Branch deleted on 2025-11-07 19:01_

---
