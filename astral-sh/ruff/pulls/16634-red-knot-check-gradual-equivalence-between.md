```yaml
number: 16634
title: "[red-knot] Check gradual equivalence between callable types"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/callable-is-gradual-equivalent-to
created_at: 2025-03-11T16:29:15Z
updated_at: 2025-03-13T02:46:53Z
url: https://github.com/astral-sh/ruff/pull/16634
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Check gradual equivalence between callable types

---

_Pull request opened by @dhruvmanila on 2025-03-11 16:29_

## Summary

Part of #15382 

Add support for checking the gradual equivalence between callable types.

A callable type is gradually equivalent to the other callable type:
* Both uses gradual form for the parameters. If not, then the number of parameters should be the same and the type at the a position in the first parameter list should be gradually equivalent the the type at the same position in the second parameter list
* Both the return types are either `None` or the some values are gradually equivalent to each other.

## Test Plan

Update `is_gradual_equivalent_to.md` with callable type test cases.

Note: I've an explicit goal of updating the property tests with the new callable types once all relations are implemented.


---

_Label `red-knot` added by @dhruvmanila on 2025-03-11 16:29_

---

_Review requested from @carljm by @dhruvmanila on 2025-03-11 16:29_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-11 16:29_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-11 16:29_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-03-11 16:29_

---

_Comment by @github-actions[bot] on 2025-03-11 16:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_gradual_equivalent_to.md`:75 on 2025-03-11 19:06_

Maybe use `Any` here on both sides to make the intent clearer?
```suggestion
static_assert(not is_gradual_equivalent_to(Callable[[int, Any], None], Callable[[Any, int], None]))
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4609 on 2025-03-11 19:13_

If I understand correctly, a `Callable` without a return type is basically treated as returning `Unknown`. So is this check here sufficient? Or should we consider an (invalid) `Callable[[int]]` to be equivalent to `Callable[[int], Unknown]`?

(and same question below for parameters)

---

_@sharkdp approved on 2025-03-11 19:37_

Apart for the one question I had, this looks good to me. Thank you.

---

_@carljm reviewed on 2025-03-12 04:33_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4609 on 2025-03-12 04:33_

A `return_type` of `None` represents e.g. a function with no return type annotation. (We could just as well represent this with `Unknown`, as we ultimately will fall back to that, but I decided to explicitly represent the lack of annotation -- as opposed to e.g. a broken annotation -- in `Signature`. Not sure this distinction is necessary/useful.) A signature with `return_type` of `None` is not possible to construct with the `Callable` special form, not even with an invalid one.

But I think your question is still a good one. I think we _should_ consider a `return_type` of `None` equivalent to a dynamic return type, because it will fall back to `Unknown` in type inference; the abstracted callable type of the function `def f(): ...` should be gradual equivalent to `Callable[[], Any]`.

This will be somewhat tricky to test currently, as we don't yet have a way to abstract a `Type::FunctionLiteral` to its `Type::Callable` supertype. I think this would be easy to add (it's just taking the signature from the function and putting it in a `Type::Callable`); we'd also need to expose it to tests through some kind of `CallableTypeFromFunction[...]` operator or similar.

Once we have a way to test this equivalence, implementing it could either be via handling in `are_optional_types_gradually_equivalent`, or it could be a reason to reverse my earlier decision and always represent missing annotations as `Type::Unknown` in signatures. I think I'd still weakly prefer the former, it should be fairly simple to do it here? But I wouldn't be opposed if you want to just make types in Signature non-optional and use `Unknown` instead of `None`.

---

_@carljm reviewed on 2025-03-12 04:38_

Looks good! I guess this (and the other similar PRs) will probably need some adjustment rebased on top of @dcreager's overload support.

---

_@dhruvmanila reviewed on 2025-03-12 16:11_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:4609 on 2025-03-12 16:11_

I've implemented the special form over at https://github.com/astral-sh/ruff/pull/16683, rebased this on top of that branch and added a couple of test cases related to this behavior.

---

_@carljm approved on 2025-03-12 20:07_

Looks great!

---

_Comment by @dhruvmanila on 2025-03-13 02:28_

I found one more case which we need to consider:
```py
def foo(*args: Any, **kwargs: Any) -> Any:
    pass

static_assert(is_gradual_equivalent_to(CallableTypeFromFunction[foo], Callable[..., Any]))
```

Because, as per the spec:

> If the input signature in a function definition includes both a `*args` and `**kwargs` parameter and both are typed as `Any` (explicitly or implicitly because it has no annotation), a type checker should treat this as the equivalent of `...`.
>
> https://typing.python.org/en/latest/spec/callables.html#meaning-of-in-callable

---

_Comment by @dhruvmanila on 2025-03-13 02:33_

Oh, that's simple to fix. I don't need to explicitly check whether both signatures has `is_gradual` flag as `true` because even in the case of `...`, the parameters is populated with `*Any, **Any`.

---

_Merged by @dhruvmanila on 2025-03-13 02:46_

---

_Closed by @dhruvmanila on 2025-03-13 02:46_

---

_Branch deleted on 2025-03-13 02:46_

---
