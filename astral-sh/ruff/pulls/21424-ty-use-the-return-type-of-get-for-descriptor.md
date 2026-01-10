```yaml
number: 21424
title: "[ty] Use the return type of `__get__` for descriptor lookups even when `__get__` is called with incorrect arguments"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/bad-dunder-gets
created_at: 2025-11-13T11:38:06Z
updated_at: 2025-11-13T12:05:12Z
url: https://github.com/astral-sh/ruff/pull/21424
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Use the return type of `__get__` for descriptor lookups even when `__get__` is called with incorrect arguments

---

_Pull request opened by @AlexWaygood on 2025-11-13 11:38_

## Summary

Fixes https://github.com/astral-sh/ty/issues/633.

Without this, we infer the type of `numpy.iinfo("int").max` and `numpy.iinfo("int").min` as `property` rather than `int` on https://github.com/astral-sh/ruff/pull/21401, which causes a _significant_ number of diagnostics across the ecosystem.

## Test Plan

Mdtests updated


---

_Label `ty` added by @AlexWaygood on 2025-11-13 11:38_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 11:40_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Label `bug` added by @AlexWaygood on 2025-11-13 11:41_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 11:41_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
werkzeug (https://github.com/pallets/werkzeug)
- tests/test_local.py:191:12: warning[possibly-missing-attribute] Attribute `startswith` may be missing on object of type `Unknown | _ProxyLookup`
- Found 409 diagnostics
+ Found 408 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/computation/rolling.py:115:57: error[unsupported-operator] Operator `not in` is not supported for types `Hashable` and `property`
- xarray/computation/rolling.py:120:37: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `property`
- xarray/computation/rolling.py:1082:63: error[unsupported-operator] Operator `not in` is not supported for types `Any` and `property`
- xarray/computation/rolling.py:1086:37: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `property`
- xarray/computation/rolling.py:1092:30: error[no-matching-overload] No overload of bound method `fromkeys` matches arguments
- xarray/computation/rolling.py:1093:18: error[not-iterable] Object of type `property` is not iterable
- xarray/computation/rolling.py:1094:16: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `(...) -> Unknown`, in comparing `Unknown` with `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | Unknown`
+ xarray/computation/rolling.py:1094:16: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `(...) -> Unknown`, in comparing `Unknown` with `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | dict[Unknown, str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]]`
- xarray/computation/rolling.py:1096:9: error[invalid-assignment] Object of type `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | Unknown` is not assignable to attribute `coord_func` of type `Mapping[Hashable, str | ((...) -> Unknown)]`
+ xarray/computation/rolling.py:1096:9: error[invalid-assignment] Object of type `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | dict[Unknown, str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]]` is not assignable to attribute `coord_func` of type `Mapping[Hashable, str | ((...) -> Unknown)]`
- xarray/computation/rolling.py:1198:25: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `Frozen[Hashable, Variable] | Any | property`
- xarray/computation/rolling.py:1211:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
- xarray/computation/rolling.py:1212:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
- xarray/computation/weighted.py:203:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
- xarray/core/coordinates.py:1194:14: error[not-iterable] Object of type `property` is not iterable
- xarray/core/coordinates.py:1196:28: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `property`
- xarray/core/dataset.py:4082:32: error[unresolved-attribute] Object of type `property` has no attribute `get_all_coords`
- xarray/core/groupby.py:772:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `property` with no `__setitem__` method
- xarray/core/groupby.py:944:40: error[unresolved-attribute] Object of type `property` has no attribute `items`
- xarray/core/groupby.py:960:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
- xarray/core/groupby.py:960:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
- xarray/core/groupby.py:1078:25: error[unsupported-operator] Operator `not in` is not supported for types `Any` and `property`
- xarray/core/groupby.py:1104:32: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `property`
- xarray/core/groupby.py:1112:49: error[unsupported-operator] Operator `not in` is not supported for types `Hashable` and `property`
- xarray/core/indexing.py:145:24: error[unresolved-attribute] Object of type `property` has no attribute `get`
- xarray/core/indexing.py:151:14: error[unsupported-operator] Operator `in` is not supported for types `Any` and `property`
- xarray/core/indexing.py:153:14: error[unsupported-operator] Operator `not in` is not supported for types `Any` and `property`
- xarray/core/resample.py:127:21: error[unresolved-attribute] Object of type `property` has no attribute `items`
- Found 1804 diagnostics
+ Found 1780 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/heap/ptmalloc.py:517:71: error[not-iterable] Object of type `property` is not iterable
- pwndbg/aglib/heap/ptmalloc.py:523:56: error[invalid-argument-type] Argument to function `round_up` is incorrect: Expected `int`, found `property`
- pwndbg/aglib/heap/ptmalloc.py:1706:40: error[unsupported-operator] Operator `*` is unsupported between objects of type `property` and `Literal[2]`
- pwndbg/aglib/heap/ptmalloc.py:2258:48: error[unsupported-operator] Operator `*` is unsupported between objects of type `property` and `Literal[2]`
- pwndbg/commands/ptmalloc2.py:321:18: error[not-iterable] Object of type `property` is not iterable
- pwndbg/commands/ptmalloc2.py:760:54: error[invalid-argument-type] Argument to function `format_bin` is incorrect: Expected `int | None`, found `property`
- pwndbg/commands/ptmalloc2.py:854:10: error[unsupported-operator] Operator `<` is not supported for types `int` and `property`
- pwndbg/commands/ptmalloc2.py:861:9: error[invalid-assignment] Object of type `property` is not assignable to `int | None`
- pwndbg/commands/ptmalloc2.py:863:27: error[unsupported-operator] Unary operator `~` is unsupported for type `property`
- pwndbg/commands/ptmalloc2.py:866:75: error[unsupported-operator] Operator `-` is unsupported between objects of type `property` and `Literal[1]`
- pwndbg/commands/ptmalloc2.py:875:29: error[unsupported-operator] Operator `-` is unsupported between objects of type `int` and `int | property`
- pwndbg/commands/ptmalloc2.py:889:28: error[unsupported-operator] Operator `-` is unsupported between objects of type `int` and `int | property`
- pwndbg/commands/ptmalloc2.py:907:43: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `property | Literal[1]`
- pwndbg/commands/ptmalloc2.py:908:39: error[unsupported-operator] Operator `+` is unsupported between objects of type `int` and `int | property`
- pwndbg/commands/ptmalloc2.py:911:67: error[invalid-argument-type] Argument to bound method `unpack_size` is incorrect: Expected `int`, found `int | property`
- pwndbg/commands/ptmalloc2.py:912:27: error[unsupported-operator] Unary operator `~` is unsupported for type `property`
- pwndbg/commands/ptmalloc2.py:923:57: error[unsupported-operator] Operator `+` is unsupported between objects of type `int` and `property`
- pwndbg/commands/ptmalloc2.py:1045:38: error[unsupported-operator] Operator `+` is unsupported between objects of type `int` and `property`
- pwndbg/commands/ptmalloc2.py:1049:33: error[unsupported-operator] Operator `+` is unsupported between objects of type `int` and `property`
- pwndbg/commands/ptmalloc2.py:1054:42: error[unsupported-operator] Operator `*` is unsupported between objects of type `property` and `Literal[2]`
- pwndbg/commands/ptmalloc2.py:1057:40: error[unsupported-operator] Operator `*` is unsupported between objects of type `property` and `Literal[2]`
- pwndbg/commands/ptmalloc2.py:1058:42: error[unsupported-operator] Operator `*` is unsupported between objects of type `property` and `Literal[2]`
- pwndbg/commands/ptmalloc2.py:1103:81: error[unsupported-operator] Operator `<<` is unsupported between objects of type `property` and `Literal[2]`
- pwndbg/commands/ptmalloc2.py:1115:59: error[unsupported-operator] Operator `<<` is unsupported between objects of type `property` and `Literal[1]`
- pwndbg/commands/ptmalloc2.py:1116:55: error[unsupported-operator] Operator `<<` is unsupported between objects of type `property` and `Literal[1]`
- pwndbg/commands/ptmalloc2.py:1128:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `int` and `property`
- pwndbg/commands/ptmalloc2.py:1134:53: error[invalid-argument-type] Argument to function `read` is incorrect: Expected `int`, found `property`
- pwndbg/commands/ptmalloc2.py:1136:38: error[unsupported-operator] Operator `*` is unsupported between objects of type `property` and `Literal[2]`
- pwndbg/commands/ptmalloc2.py:1152:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `int` and `property`
- pwndbg/commands/ptmalloc2.py:1156:43: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- pwndbg/commands/ptmalloc2.py:1241:19: error[unresolved-attribute] Object of type `property` has no attribute `bit_length`
- pwndbg/commands/ptmalloc2.py:1271:13: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- pwndbg/commands/ptmalloc2.py:1303:28: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- pwndbg/commands/ptmalloc2.py:1304:27: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- pwndbg/commands/ptmalloc2.py:1310:27: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- pwndbg/commands/ptmalloc2.py:1342:24: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- pwndbg/commands/ptmalloc2.py:1392:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- pwndbg/commands/ptmalloc2.py:1395:47: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- pwndbg/commands/ptmalloc2.py:1481:31: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- pwndbg/commands/ptmalloc2.py:1484:47: error[unsupported-operator] Operator `*` is unsupported between objects of type `Literal[2]` and `property`
- Found 2749 diagnostics
+ Found 2709 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index_datetime.py:153:25: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Any & <Protocol with members '__len__'> & ~Top[Index[Any]] & ~str) | (datetime64[date | int | None] & <Protocol with members '__len__'>)`
+ static_frame/core/index_datetime.py:153:25: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `(Any & <Protocol with members '__len__'>) | (datetime64[date | int | None] & <Protocol with members '__len__'>)`
- static_frame/core/node_iter.py:795:55: error[non-subscriptable] Cannot subscript object of type `property` with no `__getitem__` method
+ static_frame/core/node_values.py:128:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/node_values.py:275:20: error[call-non-callable] Object of type `property` is not callable
- static_frame/test/unit/test_container_util.py:359:13: error[unresolved-attribute] Object of type `property` has no attribute `tolist`
- static_frame/test/unit/test_container_util.py:379:13: error[unresolved-attribute] Object of type `property` has no attribute `tolist`
- static_frame/test/unit/test_container_util.py:539:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
- static_frame/test/unit/test_container_util.py:553:13: error[unresolved-attribute] Object of type `property` has no attribute `tolist`
- static_frame/test/unit/test_container_util.py:568:26: error[unresolved-attribute] Object of type `property` has no attribute `tolist`
- static_frame/test/unit/test_frame.py:14678:30: error[unresolved-attribute] Object of type `property` has no attribute `shape`
- static_frame/test/unit/test_frame.py:14680:18: error[unresolved-attribute] Object of type `property` has no attribute `iloc`
- Found 1921 diagnostics
+ Found 1913 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/dtypes/cast.py:406:8: error[unresolved-attribute] Object of type `property` has no attribute `kind`
- pandas/core/dtypes/cast.py:408:10: error[unresolved-attribute] Object of type `property` has no attribute `kind`
- pandas/core/dtypes/cast.py:410:10: error[unresolved-attribute] Object of type `property` has no attribute `kind`
- Found 3360 diagnostics
+ Found 3357 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/sonos/helpers.py:86:45: warning[possibly-missing-attribute] Attribute `uid` may be missing on object of type `Unknown | property`
- Found 14485 diagnostics
+ Found 14484 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/sparse/_data.py:149:9: error[invalid-assignment] Object of type `Unknown | property` is not assignable to attribute `__name__` of type `str`
- scipy/sparse/_data.py:153:27: error[invalid-argument-type] Argument to function `setattr` is incorrect: Expected `str`, found `Unknown | property`
- Found 9195 diagnostics
+ Found 9193 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @AlexWaygood on 2025-11-13 11:42_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-13 11:42_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-13 11:42_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-13 11:42_

---

_@AlexWaygood reviewed on 2025-11-13 11:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/descriptor_protocol.md`:702 on 2025-11-13 11:43_

```suggestion
# attribute is accessed on the instance or the class is by overloading `__get__`.
```

---

_@AlexWaygood reviewed on 2025-11-13 11:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3971 on 2025-11-13 11:50_

actually, not even this makes sense. If you have a `__get__` method that isn't callable, then accessing an attribute with that `__get__` method will always raise an exception at runtime. So it makes more sense for us to infer `Unknown` rather than infer the type of the bad descriptor object:

```pycon
>>> class Descriptor:
...     __get__ = None
...     
>>> class Foo:
...     desc = Descriptor()
...     
>>> Foo().desc
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    Foo().desc
TypeError: 'NoneType' object is not callable
```

---

_@sharkdp approved on 2025-11-13 12:03_

Thank you!!

---

_Merged by @AlexWaygood on 2025-11-13 12:05_

---

_Closed by @AlexWaygood on 2025-11-13 12:05_

---

_Branch deleted on 2025-11-13 12:05_

---
