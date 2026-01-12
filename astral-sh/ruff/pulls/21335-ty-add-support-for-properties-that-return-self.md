```yaml
number: 21335
title: "[ty] Add support for properties that return `Self`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1502
created_at: 2025-11-08T08:49:48Z
updated_at: 2025-11-10T10:13:38Z
url: https://github.com/astral-sh/ruff/pull/21335
synced_at: 2026-01-12T15:57:21Z
```

# [ty] Add support for properties that return `Self`

---

_@sharkdp_

## Summary

Detect usages of implicit `self` in property getters, which allows us to treat their signature as being generic.

closes https://github.com/astral-sh/ty/issues/1502

## Typing conformance

Two new type assertions that are succeeding.

## Ecosystem results

Mostly look good. There are a few new false positives related to a bug with constrained typevars that is unrelated to the work here. I reported this as https://github.com/astral-sh/ty/issues/1503.

## Test Plan

Added regression tests.


---

_Label `ty` added by @sharkdp on 2025-11-08 08:49_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-08 08:49_

---

_Comment by @astral-sh-bot[bot] on 2025-11-08 08:51_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-10 09:58:36.256463363 +0000
+++ new-output.txt	2025-11-10 09:58:39.754484899 +0000
@@ -492,8 +492,6 @@
 generics_scoping.py:65:11: error[invalid-generic-class] Generic class `MyGeneric` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
 generics_scoping.py:75:11: error[invalid-generic-class] Generic class `Bad` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
 generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@prop1`
-generics_self_advanced.py:18:1: error[type-assertion-failure] Argument does not have asserted type `ParentA`
-generics_self_advanced.py:19:1: error[type-assertion-failure] Argument does not have asserted type `ChildA`
 generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@method1`
 generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@method2]`
 generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self@method2`
@@ -1005,5 +1003,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1007 diagnostics
+Found 1005 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-08 08:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/data/btanalysis/bt_fileutils.py:578:5: error[invalid-assignment] Object of type `Series[Any] | Unknown` is not assignable to `DataFrame`
+ freqtrade/data/btanalysis/bt_fileutils.py:578:5: error[invalid-assignment] Object of type `Series[Any] | DataFrame` is not assignable to `DataFrame`
- freqtrade/data/converter/converter.py:153:13: error[invalid-assignment] Object of type `Series[Any] | Unknown` is not assignable to `DataFrame`
+ freqtrade/data/converter/converter.py:153:13: error[invalid-assignment] Object of type `Series[Any] | DataFrame` is not assignable to `DataFrame`
- freqtrade/data/converter/converter.py:155:9: error[invalid-assignment] Object of type `Series[Any] | Unknown` is not assignable to `DataFrame`
+ freqtrade/data/converter/converter.py:155:9: error[invalid-assignment] Object of type `Series[Any] | DataFrame` is not assignable to `DataFrame`
- freqtrade/data/converter/orderflow.py:109:9: error[invalid-assignment] Object of type `Series[Any] | Unknown` is not assignable to `DataFrame`
+ freqtrade/data/converter/orderflow.py:109:9: error[invalid-assignment] Object of type `Series[Any] | DataFrame` is not assignable to `DataFrame`
- freqtrade/data/converter/trade_converter.py:89:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | Unknown`
+ freqtrade/data/converter/trade_converter.py:89:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | DataFrame`
- freqtrade/data/dataprovider.py:378:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `DataFrame | Series[Any] | Unknown`
+ freqtrade/data/dataprovider.py:378:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `DataFrame | Series[Any]`
- freqtrade/data/entryexitanalysis.py:70:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | Unknown | DataFrame`
+ freqtrade/data/entryexitanalysis.py:70:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | DataFrame`
- freqtrade/data/metrics.py:243:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Series[Any] | Unknown`
+ freqtrade/data/metrics.py:243:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Series[Any] | DataFrame`
- freqtrade/data/metrics.py:244:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Series[Any] | Unknown`
+ freqtrade/data/metrics.py:244:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Series[Any] | DataFrame`
- freqtrade/data/metrics.py:245:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Series[Any] | Unknown`
+ freqtrade/data/metrics.py:245:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Series[Any] | DataFrame`
- freqtrade/data/metrics.py:246:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Series[Any] | Unknown`
+ freqtrade/data/metrics.py:246:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Series[Any] | DataFrame`
- freqtrade/data/metrics.py:247:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Series[Any] | Unknown`
+ freqtrade/data/metrics.py:247:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Series[Any] | DataFrame`
- freqtrade/data/metrics.py:249:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Series[Any] | Unknown`
+ freqtrade/data/metrics.py:249:9: error[invalid-argument-type] Argument is incorrect: Expected `Timestamp`, found `Series[Any] | DataFrame`
- freqtrade/exchange/binance_public_data.py:96:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | Unknown`
+ freqtrade/exchange/binance_public_data.py:96:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | DataFrame`
- freqtrade/freqai/data_kitchen.py:389:13: error[invalid-assignment] Object of type `Series[Any] | Unknown` is not assignable to `DataFrame`
+ freqtrade/freqai/data_kitchen.py:389:13: error[invalid-assignment] Object of type `Series[Any] | DataFrame` is not assignable to `DataFrame`
- freqtrade/freqai/data_kitchen.py:391:13: error[invalid-assignment] Object of type `Series[Any] | Unknown` is not assignable to `DataFrame`
+ freqtrade/freqai/data_kitchen.py:391:13: error[invalid-assignment] Object of type `Series[Any] | DataFrame` is not assignable to `DataFrame`
- freqtrade/freqai/freqai_interface.py:346:21: error[invalid-argument-type] Argument to bound method `set_freqai_targets` is incorrect: Expected `DataFrame`, found `Unknown | Series[Any]`
+ freqtrade/freqai/freqai_interface.py:346:21: error[invalid-argument-type] Argument to bound method `set_freqai_targets` is incorrect: Expected `DataFrame`, found `Unknown | Series[Any] | DataFrame`
- freqtrade/freqai/freqai_interface.py:350:21: error[invalid-argument-type] Argument to bound method `set_freqai_targets` is incorrect: Expected `DataFrame`, found `Unknown | Series[Any]`
+ freqtrade/freqai/freqai_interface.py:350:21: error[invalid-argument-type] Argument to bound method `set_freqai_targets` is incorrect: Expected `DataFrame`, found `Unknown | Series[Any] | DataFrame`
+ freqtrade/optimize/analysis/lookahead.py:82:31: error[non-subscriptable] Cannot subscript object of type `Hashable` with no `__getitem__` method
+ freqtrade/optimize/analysis/lookahead.py:85:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | slice[Any, Any, Any]` and `Literal[1]`
+ freqtrade/optimize/analysis/lookahead.py:89:80: error[non-subscriptable] Cannot subscript object of type `Hashable` with no `__getitem__` method
+ freqtrade/optimize/analysis/lookahead.py:90:71: error[non-subscriptable] Cannot subscript object of type `Hashable` with no `__getitem__` method
+ freqtrade/optimize/analysis/lookahead.py:93:32: error[non-subscriptable] Cannot subscript object of type `Hashable` with no `__getitem__` method
- freqtrade/optimize/optimize_reports/optimize_reports.py:529:9: error[invalid-argument-type] Argument to function `generate_pair_metrics` is incorrect: Expected `DataFrame`, found `Series[Any] | Unknown`
+ freqtrade/optimize/optimize_reports/optimize_reports.py:529:9: error[invalid-argument-type] Argument to function `generate_pair_metrics` is incorrect: Expected `DataFrame`, found `Series[Any] | DataFrame`
- freqtrade/rpc/rpc.py:1434:17: error[invalid-assignment] Object of type `Series[Any] | Unknown` is not assignable to `DataFrame`
+ freqtrade/rpc/rpc.py:1434:17: error[invalid-assignment] Object of type `Series[Any] | DataFrame` is not assignable to `DataFrame`
+ freqtrade/strategy/interface.py:1311:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[DataFrame | None, datetime | None]`, found `tuple[Any | Series[Any], Any]`
- freqtrade/templates/strategy_analysis_example.ipynb:cell 23:14:5: error[invalid-argument-type] Argument to function `generate_candlestick_graph` is incorrect: Expected `DataFrame | None`, found `Series[Any] | Unknown`
+ freqtrade/templates/strategy_analysis_example.ipynb:cell 23:14:5: error[invalid-argument-type] Argument to function `generate_candlestick_graph` is incorrect: Expected `DataFrame | None`, found `Series[Any] | DataFrame`
- Found 608 diagnostics
+ Found 614 diagnostics

vision (https://github.com/pytorch/vision)
- setup.py:506:30: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown]` and `str | list[str] | list[Unknown]`
+ setup.py:506:30: error[unsupported-operator] Operator `+` is unsupported between objects of type `list[Unknown | Path]` and `str | list[str] | list[Unknown]`

trio (https://github.com/python-trio/trio)
- src/trio/_tests/type_tests/path.py:42:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- src/trio/_tests/type_tests/path.py:43:5: error[type-assertion-failure] Argument does not have asserted type `Path`
- Found 595 diagnostics
+ Found 593 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- tests/pandas/test_schemas.py:156:39: error[invalid-argument-type] Argument to bound method `validate` is incorrect: Expected `DataFrame`, found `Series[Any] | Unknown`
+ tests/pandas/test_schemas.py:156:39: error[invalid-argument-type] Argument to bound method `validate` is incorrect: Expected `DataFrame`, found `Series[Any] | DataFrame`
- tests/pandas/test_schemas.py:174:25: error[invalid-argument-type] Argument to bound method `validate` is incorrect: Expected `DataFrame`, found `Series[Any] | Unknown`
+ tests/pandas/test_schemas.py:174:25: error[invalid-argument-type] Argument to bound method `validate` is incorrect: Expected `DataFrame`, found `Series[Any] | DataFrame`
- tests/pandas/test_schemas.py:176:25: error[invalid-argument-type] Argument to bound method `validate` is incorrect: Expected `DataFrame`, found `Series[Any] | Unknown`
+ tests/pandas/test_schemas.py:176:25: error[invalid-argument-type] Argument to bound method `validate` is incorrect: Expected `DataFrame`, found `Series[Any] | DataFrame`

meson (https://github.com/mesonbuild/meson)
+ unittests/allplatformstests.py:4413:61: error[invalid-argument-type] Argument to function `parse_machine_files` is incorrect: Expected `list[str]`, found `list[Unknown | Path]`
+ unittests/machinefiletests.py:61:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `list[Unknown | Path]`
- Found 1662 diagnostics
+ Found 1664 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/computation/rolling.py:115:57: error[unsupported-operator] Operator `not in` is not supported for types `Hashable` and `property`
+ xarray/computation/rolling.py:120:37: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `property`
- xarray/computation/rolling.py:1094:16: error[unsupported-operator] Operator `not in` is not supported for types `Hashable` and `str`, in comparing `Hashable` with `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | dict[Hashable, str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]]`
+ xarray/computation/rolling.py:1082:63: error[unsupported-operator] Operator `not in` is not supported for types `Any` and `property`
+ xarray/computation/rolling.py:1086:37: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `property`
+ xarray/computation/rolling.py:1092:30: error[no-matching-overload] No overload of bound method `fromkeys` matches arguments
+ xarray/computation/rolling.py:1093:18: error[not-iterable] Object of type `property` is not iterable
+ xarray/computation/rolling.py:1094:16: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `(...) -> Unknown`, in comparing `Unknown` with `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | Unknown`
- xarray/computation/rolling.py:1096:9: error[invalid-assignment] Object of type `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | dict[Hashable, str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)]]` is not assignable to attribute `coord_func` of type `Mapping[Hashable, str | ((...) -> Unknown)]`
+ xarray/computation/rolling.py:1096:9: error[invalid-assignment] Object of type `str | ((...) -> Unknown) | Mapping[Any, str | ((...) -> Unknown)] | Unknown` is not assignable to attribute `coord_func` of type `Mapping[Hashable, str | ((...) -> Unknown)]`
+ xarray/computation/rolling.py:1198:25: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `Frozen[Hashable, Variable] | Any | property`
+ xarray/computation/rolling.py:1211:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
+ xarray/computation/rolling.py:1212:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
+ xarray/computation/weighted.py:203:24: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
+ xarray/core/coordinates.py:1194:14: error[not-iterable] Object of type `property` is not iterable
+ xarray/core/coordinates.py:1196:28: error[unsupported-operator] Operator `in` is not supported for types `Unknown` and `property`
- xarray/core/coordinates.py:1196:69: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Hashable` on object of type `T_Xarray@assert_coordinate_consistent`
+ xarray/core/coordinates.py:1196:69: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Unknown` on object of type `T_Xarray@assert_coordinate_consistent`
- xarray/core/coordinates.py:1199:51: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Hashable` on object of type `T_Xarray@assert_coordinate_consistent`
+ xarray/core/coordinates.py:1199:51: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@assert_coordinate_consistent.__getitem__(key: Any) -> T_Xarray@assert_coordinate_consistent) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@assert_coordinate_consistent])` cannot be called with key of type `Unknown` on object of type `T_Xarray@assert_coordinate_consistent`
+ xarray/core/dataset.py:4082:32: error[unresolved-attribute] Object of type `property` has no attribute `get_all_coords`
+ xarray/core/groupby.py:772:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `DatasetCoordinates | Any | property` may be missing
+ xarray/core/groupby.py:944:40: error[unresolved-attribute] Object of type `property` has no attribute `items`
+ xarray/core/groupby.py:960:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
+ xarray/core/groupby.py:960:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `property`
- xarray/core/groupby.py:961:24: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Hashable` on object of type `T_Xarray@GroupBy`
+ xarray/core/groupby.py:961:24: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Unknown` on object of type `T_Xarray@GroupBy`
- xarray/core/groupby.py:962:35: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Hashable` on object of type `T_Xarray@GroupBy`
+ xarray/core/groupby.py:962:35: error[invalid-argument-type] Method `__getitem__` of type `(bound method T_Xarray@GroupBy.__getitem__(key: Any) -> T_Xarray@GroupBy) | (Overload[(key: Hashable) -> DataArray, (key: Iterable[Hashable]) -> T_Xarray@GroupBy])` cannot be called with key of type `Unknown` on object of type `T_Xarray@GroupBy`
+ xarray/core/groupby.py:1078:25: error[unsupported-operator] Operator `not in` is not supported for types `Any` and `property`
+ xarray/core/groupby.py:1104:32: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `property`
+ xarray/core/groupby.py:1112:49: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `property`
+ xarray/core/indexing.py:145:24: error[unresolved-attribute] Object of type `property` has no attribute `get`
+ xarray/core/indexing.py:151:14: error[unsupported-operator] Operator `in` is not supported for types `Any` and `property`
+ xarray/core/indexing.py:153:14: error[unsupported-operator] Operator `not in` is not supported for types `Any` and `property`
+ xarray/core/resample.py:127:21: error[unresolved-attribute] Object of type `property` has no attribute `items`
+ xarray/core/treenode.py:133:13: error[invalid-assignment] Object of type `dict[str | Unknown, Self@_detach | Unknown]` is not assignable to attribute `_children` of type `dict[str, typing.Self]`
- Found 1735 diagnostics
+ Found 1760 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/ext/graphviz.py:266:24: error[no-matching-overload] No overload of function `urlunsplit` matches arguments
- Found 514 diagnostics
+ Found 515 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- arviz/tests/base_tests/test_stats.py:397:12: error[invalid-argument-type] Method `__getitem__` of type `bound method _LocIndexer[Unknown].__getitem__(key: Mapping[Any, Any]) -> Unknown` cannot be called with key of type `Literal["theta[Deerfield]"]` on object of type `_LocIndexer[Unknown]`
+ arviz/tests/base_tests/test_stats.py:397:12: error[invalid-argument-type] Method `__getitem__` of type `bound method _LocIndexer[Dataset].__getitem__(key: Mapping[Any, Any]) -> Dataset` cannot be called with key of type `Literal["theta[Deerfield]"]` on object of type `_LocIndexer[Dataset]`
- arviz/tests/base_tests/test_stats.py:399:9: error[invalid-argument-type] Method `__getitem__` of type `bound method _LocIndexer[Unknown].__getitem__(key: Mapping[Any, Any]) -> Unknown` cannot be called with key of type `list[Unknown]` on object of type `_LocIndexer[Unknown]`
+ arviz/tests/base_tests/test_stats.py:399:9: error[invalid-argument-type] Method `__getitem__` of type `bound method _LocIndexer[Dataset].__getitem__(key: Mapping[Any, Any]) -> Dataset` cannot be called with key of type `list[Unknown]` on object of type `_LocIndexer[Dataset]`
- arviz/tests/base_tests/test_stats.py:410:15: error[invalid-argument-type] Method `__getitem__` of type `bound method _LocIndexer[Unknown].__getitem__(key: Mapping[Any, Any]) -> Unknown` cannot be called with key of type `Literal["theta[Deerfield]"]` on object of type `_LocIndexer[Unknown]`
+ arviz/tests/base_tests/test_stats.py:410:15: error[invalid-argument-type] Method `__getitem__` of type `bound method _LocIndexer[Dataset].__getitem__(key: Mapping[Any, Any]) -> Dataset` cannot be called with key of type `Literal["theta[Deerfield]"]` on object of type `_LocIndexer[Dataset]`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/linear_model/tests/test_coordinate_descent.py:1064:20: warning[possibly-missing-attribute] Attribute `dot` may be missing on object of type `Unknown | float64`
- Found 2564 diagnostics
+ Found 2565 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
+ setup.py:549:13: error[invalid-argument-type] Argument to bound method `spawn` is incorrect: Expected `MutableSequence[str]`, found `list[Unknown | str | Path]`
- Found 2610 diagnostics
+ Found 2611 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/test_frame.py:3371:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3372:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3373:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3374:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3375:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3376:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3377:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3378:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3379:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3380:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3399:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3402:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3413:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:3874:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:4156:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | Unknown`
+ tests/test_frame.py:4156:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | DataFrame`
- tests/test_frame.py:4297:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- tests/test_frame.py:4547:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any] | DataFrame`
- Found 6040 diagnostics
+ Found 6024 diagnostics

pandas (https://github.com/pandas-dev/pandas)
+ pandas/core/dtypes/cast.py:406:8: error[unresolved-attribute] Object of type `property` has no attribute `kind`
+ pandas/core/dtypes/cast.py:408:10: error[unresolved-attribute] Object of type `property` has no attribute `kind`
+ pandas/core/dtypes/cast.py:410:10: error[unresolved-attribute] Object of type `property` has no attribute `kind`
+ pandas/core/generic.py:6519:16: warning[redundant-cast] Value is already of type `Self@astype`
- Found 3369 diagnostics
+ Found 3373 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/node_iter.py:795:55: error[non-subscriptable] Cannot subscript object of type `property` with no `__getitem__` method
+ static_frame/core/node_values.py:275:20: error[call-non-callable] Object of type `property` is not callable
+ static_frame/test/unit/test_frame.py:14678:30: error[unresolved-attribute] Object of type `property` has no attribute `shape`
+ static_frame/test/unit/test_frame.py:14680:18: error[unresolved-attribute] Object of type `property` has no attribute `iloc`
- Found 2000 diagnostics
+ Found 2004 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/matrices/tests/test_matrices.py:2605:22: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2605:22: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2605:22: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2605:22: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2606:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2606:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2606:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2606:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2607:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2607:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2607:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrices.py:2607:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3338:22: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3338:22: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3338:22: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3338:22: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3339:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3339:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3339:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3339:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3340:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3340:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3340:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/matrices/tests/test_matrixbase.py:3340:24: error[invalid-argument-type] Argument to bound method `cross` is incorrect: Expected `MatrixExpr`, found `MutableDenseMatrix | MutableSparseMatrix | ImmutableDenseMatrix | ImmutableSparseMatrix`
+ sympy/solvers/solvers.py:3094:20: error[invalid-argument-type] Argument to bound method `jacobian` is incorrect: Expected `MatrixBase | list[Expr]`, found `Unknown | None`
- Found 14471 diagnostics
+ Found 14496 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/tests/utils/crash_test.py:28:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1672 diagnostics
+ Found 1671 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/components/sonos/helpers.py:86:45: warning[possibly-missing-attribute] Attribute `uid` may be missing on object of type `Unknown | property`
- Found 14444 diagnostics
+ Found 14445 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/sparse/_data.py:149:9: error[invalid-assignment] Object of type `Unknown | property` is not assignable to attribute `__name__` of type `str`
+ scipy/sparse/_data.py:153:27: error[invalid-argument-type] Argument to function `setattr` is incorrect: Expected `str`, found `Unknown | property`
- Found 9153 diagnostics
+ Found 9155 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~10MB
+     struct fields = ~11MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-08 08:55_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 37 | 0 | 20 |
| `type-assertion-failure` | 0 | 18 | 0 |
| `invalid-assignment` | 2 | 0 | 8 |
| `unsupported-operator` | 8 | 0 | 2 |
| `unresolved-attribute` | 9 | 0 | 0 |
| `invalid-return-type` | 1 | 0 | 5 |
| `non-subscriptable` | 5 | 0 | 0 |
| `possibly-missing-attribute` | 3 | 0 | 0 |
| `no-matching-overload` | 2 | 0 | 0 |
| `not-iterable` | 2 | 0 | 0 |
| `call-non-callable` | 1 | 0 | 0 |
| `possibly-missing-implicit-call` | 1 | 0 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **72** | **19** | **35** |

**[Full report with detailed diff](https://david-fix-1502.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1502.ecosystem-663.pages.dev/timing))




---

_Comment by @codspeed-hq[bot] on 2025-11-08 09:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1502?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21335 will **degrade performances by 8.39%**

<sub>Comparing <code>david/fix-1502</code> (08bb155) with <code>main</code> (a6f2dee)</sub>



### Summary

`❌ 1 (👁 1)` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | Simulation | [`` DateType ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1502?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Adatetype%3A%3Aproject%3A%3ADateType&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 211 ms | 230.3 ms | -8.39% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1502?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @sharkdp on 2025-11-08 10:19_

---

_Review requested from @carljm by @sharkdp on 2025-11-08 10:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-08 10:19_

---

_Review requested from @dcreager by @sharkdp on 2025-11-08 10:19_

---

_@AlexWaygood approved on 2025-11-08 17:20_

How extensible will this approach be for other decorated methods, like e.g. methods decorated with `functools.lru_cache`? (Mind you, I don't know why a method decorated with `@lru_cache` would ever be annotated as returning `Self`, but you get the idea.)

Even if this approach won't work in other cases, though, properties are common enough that I think even a narrow fix is worthwhile for now!

---

_Comment by @sharkdp on 2025-11-10 09:59_

> How extensible will this approach be for other decorated methods, like e.g. methods decorated with `functools.lru_cache`? (Mind you, I don't know why a method decorated with `@lru_cache` would ever be annotated as returning `Self`, but you get the idea.)

I thought about this, but there was no way to test this yet. I should have commented on that, though. I have now pushed up a fix that is not specific to properties and *should* work for other decorators as well. The only way I can decorate a method right now without being restricted by our generics solver is to use `def identity[T](t: T) -> T`, but that worked with the previous fix as well, as it keeps the FunctionType intact. But I modified an existing test and added a TODO.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-10 09:59_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-10 09:59_

---

_Merged by @sharkdp on 2025-11-10 10:13_

---

_Closed by @sharkdp on 2025-11-10 10:13_

---

_Branch deleted on 2025-11-10 10:13_

---
