```yaml
number: 22117
title: "[ty] Fix iteration over intersections with TypeVars whose bounds contain non-iterable types"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/int-sec
created_at: 2025-12-20T19:14:50Z
updated_at: 2026-01-17T15:51:24Z
url: https://github.com/astral-sh/ruff/pull/22117
synced_at: 2026-01-17T16:15:18Z
```

# [ty] Fix iteration over intersections with TypeVars whose bounds contain non-iterable types

---

_@charliermarsh_

## Summary

When iterating over an intersection like `T & tuple[object, ...]` where `T` is a `TypeVar` bounded by `tuple[int, ...] | int`, the iteration was returning `object` instead of `int`. Now, when a `TypeVar` fails to iterate normally, we check if it has a union bound, then extract the iterable parts and use those for the result.

See: https://github.com/astral-sh/ruff/pull/22115#issuecomment-3677967837


---

_Label `ty` added by @charliermarsh on 2025-12-20 19:14_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 19:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2025-12-20 19:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
artigraph (https://github.com/artigraph/artigraph)
- src/arti/internal/type_hints.py:172:46: error[invalid-argument-type] Argument to function `lenient_issubclass` is incorrect: Argument type `object` does not satisfy upper bound `type | tuple[type, ...]` of type variable `T`
- Found 145 diagnostics
+ Found 144 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | ndarray[Never, Never] | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- Found 1821 diagnostics
+ Found 1823 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14497 diagnostics
+ Found 14496 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2025-12-20 19:18_

---

_Review requested from @carljm by @charliermarsh on 2025-12-20 19:18_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-20 19:18_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-20 19:18_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-20 19:18_

---

_Comment by @carljm on 2025-12-24 02:14_

The approach here feels not quite right to me? Lacking in generality. Like what if one of the iterable elements of the union upper bound is actually disjoint with another element in the intersection, and the whole thing ought to simplify to `Never`. Or even more complex variants of that.

It feels like the general approach here is that we take the typevar upper bound (no matter what it is) and intersect it with the rest of the intersection elements, then iterate that intersection. (Or even more generally: always replace any typevar with its upper bound, for iteration purposes.) Once we are iterating a type, the fact that it's a typevar no longer matters: all that matters is the upper bound.

---

_@carljm requested changes on 2026-01-14 00:38_

So I think the changes here were a step in the right direction, but didn't go far enough, and this is still buggy.

Our DNF representation of types generally ensures that we can never have a union inside an intersection -- this should always be simplified by distributing the union across the intersection, potentially leading to simplifications of some elements, and resulting in a union-of-intersections. Here that rule is broken because we can have a typevar with union upper bound inside an intersection, and so we end up handling a union inside an intersection. What _should_ happen in the tested case where we end up with effectively `(tuple[int, ...] | int) & tuple[object, ...]` is that we distribute and get `(tuple[int, ...] & tuple[object, ...]) | (int & tuple[object, ...])`, which simplifies to `tuple[int, ...] | Never`, which simplifies to just `tuple[int, ...]`. This PR sort of simulates that by excluding all non-iterable parts of the union, but that solution is overfitted to the one example case. Consider this alternative case:

```py
def f[T: tuple[int, ...] | list[str]](x: T):
    if isinstance(x, tuple):
        reveal_type(x)
        for item in x:
            reveal_type(item) # should be `int` but this PR reveals `int | str`
```

Both elements of the union here are iterable, so the special case in this PR doesn't take effect and we end up wrongly considering `str` as possible. The question we should be asking is not "which elements of the union are iterable", but "what is the result of distributing the union over the intersection". If we asked that question in this second example, we would find that `list[str]` is disjoint from `tuple[object, ...]` so that intersection simplifies to `Never`, and we again would end up with just `tuple[int, ...]` and thus just `int`.

I think the right implementation approach here will be to introduce a `flatten_typevars` function which takes a type and recursively maps over any unions or intersections, resolving any typevars found to their upper bound/constraints and rebuilding the union/intersection accordingly (but unlike our type mappings, it shouldn't otherwise descend into generic or nested types -- it should only flatten top-level typevars found directly in unions/intersections). We should call this on the type before we try to iterate it.

Let me know if that doesn't make sense.

---

_Review requested from @carljm by @charliermarsh on 2026-01-17 15:41_

---

_Comment by @charliermarsh on 2026-01-17 15:51_

Thanks for the clear write-up, took another stab at it.

---
