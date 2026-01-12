```yaml
number: 20130
title: "[ty] Unpack variadic argument type in specialization"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/unpack-variadic-arg-type
created_at: 2025-08-28T10:17:15Z
updated_at: 2025-08-29T13:23:10Z
url: https://github.com/astral-sh/ruff/pull/20130
synced_at: 2026-01-12T15:56:55Z
```

# [ty] Unpack variadic argument type in specialization

---

_@dhruvmanila_

## Summary

This PR fixes various TODOs around overload call when a variadic argument is used.

The reason this bug existed is because the specialization wouldn't account for unpacking the type of the variadic argument.

This is fixed by expanding `MatchedArgument` to contain the type of that argument _only_ when it is a variadic argument. The reason is that there's a split for when the argument type is inferred -- the non-variadic arguments are inferred using `infer_argument_types` _after_ parameter matching while the variadic argument type is inferred _during_ the parameter matching. And, the `MatchedArgument` is populated _during_ parameter matching which means the unpacking would need to happen during parameter matching.

This split seems a bit inconsistent but I don't want to spend a lot of time on trying to merge them such that all argument type inference happens in a single place. I might look into it while adding support for `**kwargs`.

## Test Plan

Update existing tests by resolving the todos.

The ecosystem changes looks correct to me except for the `slice` call but it seems that it's unrelated to this PR as we infer `slice[Any, Any, Any]` for a `slice(1, 2, 3)` call on `main` as well ([playground](https://play.ty.dev/9eacce00-c7d5-4dd5-a932-4265cb2bb4f6)).


---

_Label `ty` added by @dhruvmanila on 2025-08-28 10:17_

---

_Comment by @github-actions[bot] on 2025-08-28 10:20_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-28 10:21_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Expression (https://github.com/cognitedata/Expression)
- tests/test_pipe.py:88:43: error[invalid-argument-type] Argument to function `gn` is incorrect: Expected `tuple[Literal[1], Literal[2]]`, found `Literal[1]`
- tests/test_pipe.py:88:43: error[invalid-argument-type] Argument to function `gn` is incorrect: Expected `tuple[Literal[1], Literal[2]]`, found `Literal[2]`
- tests/test_pipe.py:92:47: error[invalid-argument-type] Argument to function `yn` is incorrect: Expected `tuple[tuple[Literal[1], Literal[2]], tuple[Literal[1], Literal[2]]]`, found `tuple[Literal[1], Literal[2]]`
- tests/test_pipe.py:92:47: error[invalid-argument-type] Argument to function `yn` is incorrect: Expected `tuple[tuple[Literal[1], Literal[2]], tuple[Literal[1], Literal[2]]]`, found `tuple[Literal[1], Literal[2]]`
- tests/test_pipe.py:92:51: error[invalid-argument-type] Argument to function `gn` is incorrect: Expected `tuple[Literal[1], Literal[2]]`, found `Literal[1]`
- tests/test_pipe.py:92:51: error[invalid-argument-type] Argument to function `gn` is incorrect: Expected `tuple[Literal[1], Literal[2]]`, found `Literal[2]`
- Found 231 diagnostics
+ Found 225 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_model_construction.py:530:14: error[no-matching-overload] No overload of function `__new__` matches arguments
- Found 770 diagnostics
+ Found 769 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/index_hierarchy.py:2079:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/util.py:2444:16: error[no-matching-overload] No overload of function `__new__` matches arguments
- static_frame/test/test_case.py:165:46: error[no-matching-overload] No overload of function `__new__` matches arguments
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:42:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:66:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/test/unit/test_index_hierarchy_set_utils.py:100:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1789 diagnostics
+ Found 1791 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/matrices/common.py:3094:25: error[no-matching-overload] No overload of function `__new__` matches arguments
- sympy/matrices/common.py:3098:25: error[no-matching-overload] No overload of function `__new__` matches arguments
- Found 13410 diagnostics
+ Found 13408 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-08-28 14:38_

---

_Review requested from @carljm by @dhruvmanila on 2025-08-28 14:38_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-08-28 14:38_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-08-28 14:38_

---

_Review requested from @dcreager by @dhruvmanila on 2025-08-28 14:38_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1978 on 2025-08-28 23:40_

Do we want to leave this in?
```suggestion
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:2339 on 2025-08-28 23:46_

It looks to me like the length of this vector is always the same as the length of the `parameters` vector -- that is, these are the types assigned to each matched parameter. This isn't necessarily the same as the number of types in the argument type (which might not be a fixed-length iterable). I think this comment could be clearer about this?

---

_@carljm approved on 2025-08-28 23:56_

Looks great, thank you!

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:1978 on 2025-08-29 04:06_

Nope, thanks for catching that!

---

_@dhruvmanila reviewed on 2025-08-29 04:06_

---

_@dhruvmanila reviewed on 2025-08-29 04:18_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/call/bind.rs`:2339 on 2025-08-29 04:18_

Thanks, I've expanded the comment to include this and some additional notes around the fact that this is only populated for a variadic argument.

---

_Merged by @dhruvmanila on 2025-08-29 04:27_

---

_Closed by @dhruvmanila on 2025-08-29 04:27_

---

_Branch deleted on 2025-08-29 04:27_

---

_Comment by @dcreager on 2025-08-29 13:23_

> This split seems a bit inconsistent but I don't want to spend a lot of time on trying to merge them such that all argument type inference happens in a single place.

I like how you found a simpler solution for this — I was worried this would be a much more invasive change!

Note that doing argument inference in a single place will require being able to infer the type of a particular expression as both a value form and a type form. Right now we assume that each expression is inferred to a single type, and so we have to _choose_ whether it should be a value form type or a type form type. That's why we're currently doing the argument type inference in two places — for normal arguments, we have to match the argument against a parameter, to see if the parameter is annotated as a `TypeForm` (which we support synthetically but not via the actual `TypeForm` Python syntax yet), which tells us whether to call `infer_expression` or `infer_type_expression` for that argument. For variadic arguments, we need to know the type of the iterable before we perform parameter matching, which is why we perform that inference early. (That means that we don't currently support splatting an argument into a `TypeForm` parameter!)

So the ideal end state for this, I think, would be to update `TypeInferenceBuilder` to let us track the value-form and type-form inferred types separately. At the moment we only allow ourselves to infer a single type for an expression, and panic if we try to infer more than one. With this change, we would panic if we try to infer more than one value-form type, or more than one type-form type, but would allow ourselves to infer one of each without panicking.

---
