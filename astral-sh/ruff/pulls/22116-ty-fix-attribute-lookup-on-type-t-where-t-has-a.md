```yaml
number: 22116
title: "[ty] Fix attribute lookup on `type[T]` where `T` has a union bound"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
assignees: []
merged: true
base: charlie/unions
head: charlie/met
created_at: 2025-12-20T18:47:36Z
updated_at: 2025-12-21T23:23:48Z
url: https://github.com/astral-sh/ruff/pull/22116
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Fix attribute lookup on `type[T]` where `T` has a union bound

---

_Pull request opened by @charliermarsh on 2025-12-20 18:47_

## Summary

For `type[T]` where `T: Replace | Multiply`, we now do the following:

- `try_from_instance(Replace | Multiply)` becomes `type[Replace] | type[Multiply]`
- `to_meta_type(type[Replace] | type[Multiply])` becomes `type[type] | type[type]` which becomes `type[type]`

See: https://github.com/astral-sh/ruff/pull/22115#issuecomment-3677977751


---

_Label `ty` added by @charliermarsh on 2025-12-20 18:47_

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 18:49_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-20 18:50_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreterbase/baseobjects.py:199:16: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `type[Unknown] | type[InterpreterObjectTypeVar@ObjectHolder]`
- mesonbuild/interpreterbase/baseobjects.py:216:51: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `type[Unknown] | type[InterpreterObjectTypeVar@ObjectHolder]`
- Found 1947 diagnostics
+ Found 1945 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataarray.py:5737:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`
- xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `T_Xarray@map_blocks | DataArray | Dataset`
+ xarray/core/dataset.py:8866:16: error[invalid-return-type] Return type does not match returned value: expected `T_Xarray@map_blocks`, found `DataArray | Dataset`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/auxs/calibtools.py:1814:34: error[unresolved-attribute] Object of type `type[TypeRule2@get_rule]` has no attribute `__name__`
- hydpy/auxs/calibtools.py:1979:62: error[unresolved-attribute] Object of type `type[TypeRule1@CalibrationInterface]` has no attribute `__name__`
- Found 680 diagnostics
+ Found 678 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[TypeBlocks | Batch | SeriesAssign | ... omitted 6 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | Top[Yarn[Any]] | ... omitted 6 union elements, generic[object]]`
- Found 1844 diagnostics
+ Found 1843 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2025-12-20 18:57_

---

_Review requested from @carljm by @charliermarsh on 2025-12-20 18:57_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-20 18:57_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-20 18:57_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-20 18:57_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:75 on 2025-12-20 19:13_

this isn't _totally_ accurate... more accurate would be something like this :P

```suggestion
`type[T]` with a union upper bound `T: A | B` represents the metatype of a type variable `T` where `T` can be solved to any subtype of `A` or any subtype of `B`. It
behaves similarly to a type variable that can be solved to any subclass of `A` or any subclass of `B`.
Since all classes are instances of `type`, attributes on instances of `type` like `__name__` and `__qualname__` should still be accessible:
```

---

_@AlexWaygood reviewed on 2025-12-20 19:17_

Haven't looked too closely yet but it surprises me a bit that we'd have to map over the bounds/constraints like this... would appreciate it if @ibraheemdev could take a look, since he originally implemented `type[T]` where `T` is a type variable

---

_Comment by @AlexWaygood on 2025-12-20 21:31_

Okay _that_ looks a lot more like what I'd expect! 😃 but I'd still love it if @ibraheemdev could take a look

---

_@ibraheemdev approved on 2025-12-20 21:51_

Looks good to me!

---

_Comment by @charliermarsh on 2025-12-20 21:55_

Let’s go!

---

_Comment by @charliermarsh on 2025-12-20 22:04_

(The test case here does depend on https://github.com/astral-sh/ruff/pull/22115 being merged though.)

---

_Merged by @charliermarsh on 2025-12-21 23:23_

---

_Closed by @charliermarsh on 2025-12-21 23:23_

---

_Branch deleted on 2025-12-21 23:23_

---
