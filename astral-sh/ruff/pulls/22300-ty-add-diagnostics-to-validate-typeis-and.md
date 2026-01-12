```yaml
number: 22300
title: "[ty] Add diagnostics to validate `TypeIs` and `TypeGuard` definitions"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/type-is
created_at: 2025-12-30T16:21:43Z
updated_at: 2025-12-30T17:50:29Z
url: https://github.com/astral-sh/ruff/pull/22300
synced_at: 2026-01-12T15:57:46Z
```

# [ty] Add diagnostics to validate `TypeIs` and `TypeGuard` definitions

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/ty/issues/2267.


---

_Comment by @astral-sh-bot[bot] on 2025-12-30 16:23_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-30 17:48:51.808699256 +0000
+++ new-output.txt	2025-12-30 17:48:54.030699212 +0000
@@ -756,16 +756,22 @@
 namedtuples_usage.py:43:5: error[not-subscriptable] Cannot delete subscript on object of type `Point` with no `__delitem__` method
 namedtuples_usage.py:52:1: error[invalid-assignment] Too many values to unpack: Expected 2
 namedtuples_usage.py:53:1: error[invalid-assignment] Not enough values to unpack: Expected 4
+narrowing_typeguard.py:102:23: error[invalid-type-guard-definition] `TypeGuard` function must have a parameter to narrow
+narrowing_typeguard.py:107:22: error[invalid-type-guard-definition] `TypeGuard` function must have a parameter to narrow
 narrowing_typeguard.py:128:20: error[invalid-argument-type] Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeGuard[int]`
 narrowing_typeguard.py:148:26: error[invalid-argument-type] Argument to function `takes_callable_str_proto` is incorrect: Expected `CallableStrProto`, found `def simple_typeguard(val: object) -> TypeGuard[int]`
 narrowing_typeis.py:21:9: error[type-assertion-failure] Type `tuple[str, ...]` does not match asserted type `tuple[str, ...] & ~tuple[str, str]`
 narrowing_typeis.py:35:18: error[invalid-assignment] Object of type `object` is not assignable to `int`
 narrowing_typeis.py:38:9: error[type-assertion-failure] Type `int` does not match asserted type `int & ~Awaitable[object]`
+narrowing_typeis.py:105:23: error[invalid-type-guard-definition] `TypeIs` function must have a parameter to narrow
+narrowing_typeis.py:110:22: error[invalid-type-guard-definition] `TypeIs` function must have a parameter to narrow
 narrowing_typeis.py:132:20: error[invalid-argument-type] Argument to function `takes_callable_str` is incorrect: Expected `(object, /) -> str`, found `def simple_typeguard(val: object) -> TypeIs[int]`
 narrowing_typeis.py:152:26: error[invalid-argument-type] Argument to function `takes_callable_str_proto` is incorrect: Expected `CallableStrProto`, found `def simple_typeguard(val: object) -> TypeIs[int]`
 narrowing_typeis.py:169:17: error[invalid-argument-type] Argument to function `takes_typeguard` is incorrect: Expected `(object, /) -> TypeGuard[int]`, found `def is_int_typeis(val: object) -> TypeIs[int]`
 narrowing_typeis.py:170:14: error[invalid-argument-type] Argument to function `takes_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def is_int_typeguard(val: object) -> TypeGuard[int]`
 narrowing_typeis.py:191:18: error[invalid-argument-type] Argument to function `takes_int_typeis` is incorrect: Expected `(object, /) -> TypeIs[int]`, found `def bool_typeis(val: object) -> TypeIs[bool]`
+narrowing_typeis.py:195:27: error[invalid-type-guard-definition] Narrowed type `str` is not assignable to the declared parameter type `int`
+narrowing_typeis.py:199:45: error[invalid-type-guard-definition] Narrowed type `list[int]` is not assignable to the declared parameter type `list[object]`
 overloads_basic.py:39:1: error[invalid-argument-type] Method `__getitem__` of type `Overload[(__i: int, /) -> int, (__s: slice[Any, Any, Any], /) -> bytes]` cannot be called with key of type `Literal[""]` on object of type `Bytes`
 overloads_definitions.py:20:5: error[invalid-overload] Overloaded function `func1` requires at least two overloads
 overloads_definitions.py:33:5: error[invalid-overload] Overloads for function `func2` must be followed by a non-`@overload`-decorated implementation function
@@ -1020,4 +1026,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1022 diagnostics
+Found 1028 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-30 16:24_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

mypy (https://github.com/python/mypy)
+ mypy/typeshed/stdlib/builtins.pyi:1292:33: error[invalid-type-guard-definition] Narrowed type `Top[(...) -> object]` is not assignable to the declared parameter type `object`
- mypy/typeshed/stdlib/dataclasses.pyi:380:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ mypy/typeshed/stdlib/dataclasses.pyi:382:32: error[invalid-type-guard-definition] Narrowed type `DataclassInstance | type` is not assignable to the declared parameter type `Never`
+ mypy/typeshed/stdlib/dataclasses.pyi:384:34: error[invalid-type-guard-definition] Narrowed type `DataclassInstance | type` is not assignable to the declared parameter type `Never`
- Found 1753 diagnostics
+ Found 1755 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5757:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8873:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

jax (https://github.com/google/jax)
- jax/_src/tree_util.py:302:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:305:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- jax/_src/tree_util.py:308:31: error[invalid-argument-type] Argument to bound method `register_node` is incorrect: Expected `(Hashable, Iterable[object], /) -> T@register_pytree_node`, found `(_AuxData@register_pytree_node, _Children@register_pytree_node, /) -> T@register_pytree_node`
- Found 2804 diagnostics
+ Found 2801 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Self@iloc | Series[Any, Any], generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/core/dtypes/missing.pyi:58:55: error[invalid-type-guard-definition] Narrowed type `ScalarT0@notna` is not assignable to the declared parameter type `ScalarT0@notna | NaTType | NAType | None`
- Found 5150 diagnostics
+ Found 5151 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-12-30 16:27_

---

_Marked ready for review by @charliermarsh on 2025-12-30 16:45_

---

_Review requested from @carljm by @charliermarsh on 2025-12-30 16:45_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-30 16:45_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-30 16:45_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-30 16:45_

---

_Renamed from "[ty] Add diagnostics to validate TypeIs and TypeGuard definitions" to "[ty] Add diagnostics to validate `TypeIs` and `TypeGuard` definitions" by @charliermarsh on 2025-12-30 16:50_

---
