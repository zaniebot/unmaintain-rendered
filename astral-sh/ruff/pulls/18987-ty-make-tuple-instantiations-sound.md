```yaml
number: 18987
title: "[ty] Make tuple instantiations sound"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/sound-tuples
created_at: 2025-06-27T14:27:42Z
updated_at: 2025-06-27T21:06:05Z
url: https://github.com/astral-sh/ruff/pull/18987
synced_at: 2026-01-12T15:56:29Z
```

# [ty] Make tuple instantiations sound

---

_@AlexWaygood_

## Summary

Ensure that we correctly infer calls such as `tuple((1, 2))`, `tuple(range(42))`, etc. Ensure that we emit errors on invalid calls such as `tuple[int, str]()`.

## Test Plan

Mdtests


---

_Label `ty` added by @AlexWaygood on 2025-06-27 14:27_

---

_Comment by @github-actions[bot] on 2025-06-27 14:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- error[invalid-assignment] src/attr/_make.py:1559:5: Object of type `tuple[Unknown, ...]` is not assignable to `list[Unknown]`
+ error[invalid-assignment] src/attr/_make.py:1559:5: Object of type `tuple[@Todo(generator type), ...]` is not assignable to `list[Unknown]`

vision (https://github.com/pytorch/vision)
- error[invalid-assignment] references/classification/utils.py:420:5: Object of type `tuple[Unknown, ...]` is not assignable to `list[type] | None`
+ error[invalid-assignment] references/classification/utils.py:420:5: Object of type `tuple[@Todo(map_with_boundness: intersections with negative contributions), ...]` is not assignable to `list[type] | None`
- error[invalid-argument-type] torchvision/ops/poolers.py:283:34: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `tuple[int] | list[int] | int`
+ error[invalid-argument-type] torchvision/ops/poolers.py:283:34: Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `tuple[int] | list[int] | int`
- error[invalid-argument-type] torchvision/transforms/_functional_pil.py:179:42: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number) | (tuple[int, ...] & tuple[Unknown, ...]) | (list[int] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(Subscript expressions on intersections)`
+ error[invalid-argument-type] torchvision/transforms/_functional_pil.py:179:42: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number) | (tuple[int, ...] & tuple[Unknown, ...]) | (list[int] & list[Unknown] & ~list[Unknown]) | @Todo(Subscript expressions on intersections)`
- error[invalid-argument-type] torchvision/transforms/_functional_pil.py:183:37: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number) | (tuple[int, ...] & tuple[Unknown, ...]) | (list[int] & list[Unknown] & ~list[Unknown]) | tuple[Unknown, ...] | @Todo(Subscript expressions on intersections)`
+ error[invalid-argument-type] torchvision/transforms/_functional_pil.py:183:37: Argument to function `expand` is incorrect: Expected `int | tuple[int, ...]`, found `(int & Number) | (list[int] & Number & ~list[Unknown]) | (tuple[int, ...] & Number) | (tuple[int, ...] & tuple[Unknown, ...]) | (list[int] & list[Unknown] & ~list[Unknown]) | @Todo(Subscript expressions on intersections)`

pywin32 (https://github.com/mhammond/pywin32)
- error[invalid-argument-type] win32/Demos/security/setkernelobjectsecurity.py:77:12: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[Unknown, ...]`
+ error[invalid-argument-type] win32/Demos/security/setkernelobjectsecurity.py:77:12: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[@Todo(generator type), ...]`

static-frame (https://github.com/static-frame/static-frame)
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4597:56: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4600:59: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4609:56: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4612:59: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4624:53: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4627:69: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4640:53: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4643:56: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4646:71: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4659:65: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4662:65: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4665:55: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4678:65: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4681:65: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4684:71: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4700:53: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4703:55: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4711:59: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4714:59: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4717:63: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4729:59: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4732:59: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4735:59: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4747:59: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4750:59: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4753:63: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4764:67: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] static_frame/test/unit/test_series.py:4767:68: Unused blanket `type: ignore` directive
- Found 1839 diagnostics
+ Found 1811 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-argument-type] src/_pytest/fixtures.py:1387:70: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Sequence[object] & ~((...) -> object)) | (((Any, /) -> object) & ~((...) -> object))`
+ error[invalid-argument-type] src/_pytest/fixtures.py:1387:70: Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `(Sequence[object] & ~((...) -> object)) | (((Any, /) -> object) & ~((...) -> object))`

sympy (https://github.com/sympy/sympy)
- error[invalid-argument-type] sympy/combinatorics/fp_groups.py:25:33: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(bound method FpGroup._generators() -> Unknown) | @Todo(instance attribute on class with dynamic base)`
+ error[invalid-argument-type] sympy/combinatorics/fp_groups.py:25:33: Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `(bound method FpGroup._generators() -> Unknown) | @Todo(instance attribute on class with dynamic base)`
- error[invalid-argument-type] sympy/stats/joint_rv_types.py:192:29: Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | ImmutableDenseMatrix`
+ error[invalid-argument-type] sympy/stats/joint_rv_types.py:192:29: Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `Unknown | ImmutableDenseMatrix`

```
</details>


---

_Comment by @AlexWaygood on 2025-06-27 18:19_

The new errors on `static-frame` are because `ndarray` has a `to_list()` method but `Series` does not, and `post[0][1]` [here](https://github.com/static-frame/static-frame/blob/a97f207a7efeeddb65fa812106c240a110b43489/static_frame/test/unit/test_series.py#L4597) is now inferred as `ndarray | Series` where it was previously inferred as `Unknown`. Other type checkers have the same inference as us here, which is why all these calls already have `type: ignore`s on them, and therefore why these errors present as "unused `type: ignore`" errors going away.

---

_Marked ready for review by @AlexWaygood on 2025-06-27 18:20_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-27 18:20_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-27 18:20_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-27 18:20_

---

_@carljm approved on 2025-06-27 18:33_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:2430 on 2025-06-27 18:33_

nit: `Self::Iterable` to be consistent with the other arms

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:978 on 2025-06-27 18:35_

I think this would avoid unwrapping the argument type just to then rewrap it:

```suggestion
                                    argument.filter(Type::is_tuple).unwrap_or_else(|| {
```

(though it depends on an `is_tuple` method that may not exist yet!)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4310 on 2025-06-27 18:36_

I think I follow the constructors here, but can you put a comment with the Python syntax of the signature you're trying to construct here?

---

_Merged by @AlexWaygood on 2025-06-27 18:37_

---

_Closed by @AlexWaygood on 2025-06-27 18:37_

---

_Branch deleted on 2025-06-27 18:37_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4314 on 2025-06-27 18:39_

With the `Tuple::resize` method that #18948 introduces, we can make this work for _any_ tuple, not just one with exactly the same size as the type being constructed.  I also have plans to have `try_iterator` return a tuple spec describing the return values of the iterator, which would then give us what we need for this to work for any iterable type.

---

_@dcreager reviewed on 2025-06-27 18:39_

---

_@AlexWaygood reviewed on 2025-06-27 20:44_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:978 on 2025-06-27 20:44_

`argument` is a type rather than an `Option` here so I think this would have to be

```suggestion
                                    argument.into_tuple().filter(Type::is_tuple).unwrap_or_else(|| {
```

Adding a `Type::is_tuple` method just for that feels a bit unnecessary to me? No strong opinion though

---

_@AlexWaygood reviewed on 2025-06-27 20:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4314 on 2025-06-27 20:53_

> With the `Tuple::resize` method that #18948 introduces, we can make this work for _any_ tuple, not just one with exactly the same size as the type being constructed.

Hmm, not sure I totally follow this. Can you give an example of something that should be covered in this PR but isn't? :-)

---

_@dcreager reviewed on 2025-06-27 21:06_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:4314 on 2025-06-27 21:06_

Sorry, wasn't requesting any changes here with this comment, just leaving me/others a breadcrumb for a future improvement

---
