```yaml
number: 20284
title: "[ty] Treat `Hashable`, and similar protocols, equivalently to `object` for subtyping/assignability"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/hashable
created_at: 2025-09-07T12:28:17Z
updated_at: 2025-09-10T10:46:56Z
url: https://github.com/astral-sh/ruff/pull/20284
synced_at: 2026-01-12T15:56:58Z
```

# [ty] Treat `Hashable`, and similar protocols, equivalently to `object` for subtyping/assignability

---

_@AlexWaygood_

## Summary

This PR fixes https://github.com/astral-sh/ty/issues/1132 by treating any supertype of `object` the same as `object` for the purposes of subtyping/assignability. This fixes some serious false positives that we have on `main`, which make ty hard to use on code that makes heavy use of the popular `pandas` library. It does so at the cost of introducing serious false negatives for the same users. For now, this seems like a good trade-off; in the future, however, we may wish to explore rolling this back and explore doing less eager simplifications of unions when the unions include structural types. This would improve compatibility with other type checkers and (in my opinion) probably lead to a more intuitive user experience. If this is merged, I'll open a followup issue to track doing that.

## Test Plan

Mdtests added.


---

_Label `ty` added by @AlexWaygood on 2025-09-07 12:28_

---

_Comment by @github-actions[bot] on 2025-09-07 12:30_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-10 10:36:03.633952560 +0000
+++ new-output.txt	2025-09-10 10:36:06.644971417 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(b416)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1267a)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(b816)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(12a7a)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-07 12:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
yarl (https://github.com/aio-libs/yarl)
+ yarl/_quoting_py.py:148:19: warning[redundant-cast] Value is already of type `BufferedIncrementalDecoder`
- Found 48 diagnostics
+ Found 49 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/data/btanalysis/bt_fileutils.py:377:22: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable | Index[Any]`, found `list[@Todo(list literal element type)]`
- freqtrade/freqai/RL/BaseReinforcementLearningModel.py:371:13: error[no-matching-overload] No overload of bound method `drop` matches arguments
- freqtrade/optimize/backtesting.py:479:32: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable | Index[Any]`, found `Unknown | list[@Todo(list literal element type)]`
- freqtrade/optimize/optimize_reports/optimize_reports.py:283:25: warning[possibly-unbound-attribute] Attribute `strftime` on type `Hashable | Unknown` is possibly unbound
+ freqtrade/optimize/optimize_reports/optimize_reports.py:283:25: error[unresolved-attribute] Type `Hashable` has no attribute `strftime`
- freqtrade/optimize/optimize_reports/optimize_reports.py:284:32: warning[possibly-unbound-attribute] Attribute `to_pydatetime` on type `Hashable | Unknown` is possibly unbound
+ freqtrade/optimize/optimize_reports/optimize_reports.py:284:32: error[unresolved-attribute] Type `Hashable` has no attribute `to_pydatetime`
- Found 386 diagnostics
+ Found 383 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/series/test_series.py:304:30: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable | Index[Any]`, found `list[@Todo(list literal element type)]`
- tests/series/test_series.py:306:12: error[type-assertion-failure] Argument does not have asserted type `None`
- tests/series/test_series.py:306:24: error[no-matching-overload] No overload of bound method `drop` matches arguments
- tests/series/test_series.py:307:12: error[type-assertion-failure] Argument does not have asserted type `None`
- tests/series/test_series.py:307:24: error[no-matching-overload] No overload of bound method `drop` matches arguments
- tests/test_frame.py:462:31: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable | Index[Any]`, found `list[@Todo(list literal element type)]`
- tests/test_frame.py:463:31: error[invalid-argument-type] Argument to bound method `drop` is incorrect: Expected `Hashable | Index[Any]`, found `list[@Todo(list literal element type)]`
- tests/test_frame.py:466:12: error[type-assertion-failure] Argument does not have asserted type `None`
- tests/test_frame.py:466:24: error[no-matching-overload] No overload of bound method `drop` matches arguments
+ tests/test_frame.py:3300:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_frame.py:3301:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_frame.py:3302:34: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_frame.py:3932:54: error[invalid-argument-type] Argument to bound method `reset_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- Found 4969 diagnostics
+ Found 4962 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/computation/rolling.py:448:13: error[invalid-assignment] Object of type `dict[@Todo(dict comprehension key type), @Todo(dict comprehension value type)]` is not assignable to `Hashable`
- xarray/computation/rolling.py:984:13: error[invalid-assignment] Object of type `dict[@Todo(dict comprehension key type), @Todo(dict comprehension value type)]` is not assignable to `Hashable`
- xarray/core/dataarray.py:2754:13: error[invalid-assignment] Object of type `dict[Unknown, Literal[1]]` is not assignable to `Hashable`
- xarray/core/dataarray.py:2756:13: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to `Hashable`
- xarray/core/dataarray.py:2758:37: error[invalid-argument-type] Argument to function `either_dict_or_kwargs` is incorrect: Expected `Mapping[Any, Any] | None`, found `Hashable`
- xarray/core/dataset.py:1340:49: error[invalid-argument-type] Argument to function `did_you_mean` is incorrect: Expected `Hashable`, found `Hashable | Iterable[Hashable]`
- xarray/core/dataset.py:1406:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Hashable | Iterable[Hashable]`
+ xarray/core/dataset.py:1406:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Hashable`
- xarray/core/dataset.py:4561:13: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `Hashable`
- xarray/core/dataset.py:4567:13: error[invalid-assignment] Object of type `dict[@Todo(dict literal key type), @Todo(dict literal value type)]` is not assignable to `Hashable`
- xarray/core/dataset.py:4571:13: error[invalid-assignment] Object of type `dict[Unknown, Literal[1]]` is not assignable to `Hashable`
- xarray/core/dataset.py:4865:13: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `Hashable`
- xarray/core/dataset.py:4867:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Hashable`
- xarray/core/dataset.py:4888:21: error[not-iterable] Object of type `Hashable` is not iterable
- xarray/core/dataset.py:4904:24: error[unsupported-operator] Operator `not in` is not supported for types `@Todo(Type::Intersection.call())` and `Hashable`
- xarray/core/dataset.py:4907:20: error[unsupported-operator] Operator `not in` is not supported for types `Unknown` and `Hashable`, in comparing `Unknown & Hashable` with `Hashable`
- xarray/core/dataset.py:8067:13: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `Hashable | DataArray`
- xarray/core/dataset.py:8068:71: error[not-iterable] Object of type `Top[list[Unknown]] | Hashable | DataArray` may not be iterable
- xarray/structure/concat.py:334:22: warning[possibly-unbound-attribute] Attribute `dims` on type `Hashable | Any` is possibly unbound
+ xarray/structure/concat.py:334:22: error[unresolved-attribute] Type `Hashable` has no attribute `dims`
- xarray/tests/test_backends.py:1588:13: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:567:34: error[invalid-argument-type] Argument to bound method `rename` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataarray.py:1183:27: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:1893:33: error[invalid-argument-type] Argument to bound method `rename` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataarray.py:1909:35: error[invalid-argument-type] Argument to bound method `rename` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataarray.py:1922:34: error[invalid-argument-type] Argument to bound method `rename` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataarray.py:2057:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2060:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2063:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2065:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2067:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2071:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2073:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2075:27: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2076:27: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2085:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataarray.py:2089:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataarray.py:2111:36: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2123:36: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2137:36: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2174:36: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataarray.py:2221:36: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:2224:33: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:4343:28: error[invalid-argument-type] Argument to bound method `sortby` is incorrect: Expected `Hashable | DataArray`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataarray.py:4351:28: error[invalid-argument-type] Argument to bound method `sortby` is incorrect: Expected `Hashable | DataArray`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:1895:27: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:3568:34: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:3590:34: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataset.py:3592:34: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataset.py:3605:39: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:3624:39: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:3647:39: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `dict[str, list[@Todo(list literal element type)]]`
- xarray/tests/test_dataset.py:3675:39: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataset.py:3787:36: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataset.py:3797:28: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[Hashable]`
- xarray/tests/test_dataset.py:3842:31: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:3906:32: error[invalid-argument-type] Argument to bound method `reset_index` is incorrect: Expected `Hashable`, found `str | list[str]`
- xarray/tests/test_dataset.py:3944:40: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:5080:36: error[invalid-argument-type] Argument to bound method `rename` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_dataset.py:6842:28: error[invalid-argument-type] Argument to bound method `sortby` is incorrect: Expected `Hashable | DataArray`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:6848:28: error[invalid-argument-type] Argument to bound method `sortby` is incorrect: Expected `Hashable | DataArray`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:6891:28: error[invalid-argument-type] Argument to bound method `sortby` is incorrect: Expected `Hashable | DataArray`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_dataset.py:6895:28: error[invalid-argument-type] Argument to bound method `sortby` is incorrect: Expected `Hashable | DataArray`, found `list[@Todo(list literal element type)]`
+ xarray/tests/test_dataset.py:7957:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ xarray/tests/test_groupby.py:1156:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[Any, Any] | None`, found `DataArray`
- xarray/tests/test_indexing.py:271:31: error[invalid-argument-type] Argument to bound method `expand_dims` is incorrect: Expected `Hashable`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- xarray/tests/test_interp.py:580:27: error[invalid-argument-type] Argument to bound method `sortby` is incorrect: Expected `Hashable | DataArray`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_plot.py:260:27: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- xarray/tests/test_plot.py:1514:27: error[invalid-argument-type] Argument to bound method `set_index` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- Found 1678 diagnostics
+ Found 1617 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/indexes/base.py:1815:17: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `Hashable`
- pandas/core/indexes/base.py:1822:17: error[invalid-assignment] Object of type `list[Hashable]` is not assignable to `Hashable`
- pandas/core/indexes/base.py:1824:17: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `Hashable`
- pandas/core/indexes/base.py:1826:16: error[invalid-return-type] Return type does not match returned value: expected `list[Hashable]`, found `Hashable`
+ pandas/core/indexes/base.py:1826:16: error[invalid-return-type] Return type does not match returned value: expected `list[Hashable]`, found `(Top[list[Unknown]] & ~AlwaysFalsy) | list[Hashable] | list[@Todo(list literal element type)]`
- pandas/core/indexes/base.py:6406:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `Unknown | (Unknown & FrozenList) | list[@Todo(list literal element type)] | None`
- pandas/core/indexing.py:2013:60: error[non-subscriptable] Cannot subscript object of type `slice[Any, Any, Any]` with no `__getitem__` method
- pandas/tests/copy_view/test_indexing.py:861:67: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/copy_view/test_methods.py:937:43: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/copy_view/test_methods.py:951:43: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/copy_view/test_methods.py:965:62: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/copy_view/test_methods.py:1048:62: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/extension/base/reshaping.py:256:43: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/extension/test_sparse.py:180:43: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/indexing/test_indexing.py:1320:61: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/indexing/test_indexing.py:1325:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/indexing/test_indexing.py:1412:43: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/indexing/test_indexing.py:1767:60: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/indexing/test_indexing.py:1770:64: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/indexing/test_xs.py:142:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/indexing/test_xs.py:344:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/indexing/test_xs.py:351:43: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_drop.py:173:54: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_drop.py:344:60: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_droplevel.py:15:39: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_droplevel.py:17:64: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_join.py:522:57: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_join.py:524:57: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_join.py:526:57: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_join.py:531:62: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:100:54: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:101:58: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:112:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:115:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:127:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:135:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:145:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:153:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:162:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:202:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:368:39: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_rename.py:381:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_reset_index.py:572:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_reset_index.py:610:46: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_reset_index.py:749:16: error[no-matching-overload] No overload of bound method `reset_index` matches arguments
- pandas/tests/frame/methods/test_reset_index.py:762:9: error[no-matching-overload] No overload of bound method `reset_index` matches arguments
- pandas/tests/frame/methods/test_reset_index.py:765:9: error[no-matching-overload] No overload of bound method `reset_index` matches arguments
- pandas/tests/frame/methods/test_reset_index.py:801:40: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_reset_index.py:807:54: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_sort_index.py:284:61: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/frame/methods/test_sort_index.py:359:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/frame/methods/test_sort_index.py:364:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/frame/methods/test_sort_index.py:372:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/frame/methods/test_sort_index.py:469:58: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/frame/methods/test_sort_index.py:476:58: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/frame/methods/test_sort_index.py:484:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/frame/methods/test_sort_index.py:583:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_sort_index.py:793:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_sort_index.py:812:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_sort_index.py:963:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_sort_index.py:980:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_to_csv.py:607:75: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `(Unknown & ~Literal[True]) | None | list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_to_dict.py:344:62: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_to_dict.py:354:58: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/methods/test_to_records.py:446:63: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/frame/methods/test_to_records.py:449:60: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/frame/test_arithmetic.py:793:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_arithmetic.py:809:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_arithmetic.py:826:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_arithmetic.py:845:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_arithmetic.py:863:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_repr.py:57:58: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_repr.py:123:49: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:309:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:322:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:326:21: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:344:21: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:450:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:509:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:715:64: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:793:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:796:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:804:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:815:43: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:818:47: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:835:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:1123:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:1158:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:1289:72: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:1321:67: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:1537:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:2365:43: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:2449:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:2575:46: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:2589:65: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:2686:58: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_stack_unstack.py:2687:69: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:234:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:237:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:254:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:278:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:296:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:299:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:316:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:340:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:368:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:371:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:380:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:395:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:412:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:415:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:427:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:445:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:607:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/frame/test_subclass.py:610:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/generic/test_series.py:21:54: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/aggregate/test_aggregate.py:111:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/aggregate/test_aggregate.py:375:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_nlargest_nsmallest.py:57:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_nth.py:675:41: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_quantile.py:124:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_quantile.py:444:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_quantile.py:450:59: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_value_counts.py:261:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_value_counts.py:618:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_value_counts.py:656:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_value_counts.py:883:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_value_counts.py:959:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/methods/test_value_counts.py:1057:53: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_all_methods.py:65:61: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_apply.py:314:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_apply.py:1182:72: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_apply.py:1204:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_apply.py:1285:58: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_groupby.py:492:39: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_groupby.py:1528:47: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_groupby.py:1540:50: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_groupby.py:1891:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_groupby.py:1909:41: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_groupby.py:2467:62: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_groupby.py:2934:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_groupby_dropna.py:46:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/groupby/test_groupby_dropna.py:90:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/groupby/test_groupby_dropna.py:237:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/groupby/test_groupby_dropna.py:330:47: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_grouping.py:149:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/test_grouping.py:365:72: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/transform/test_transform.py:953:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/groupby/transform/test_transform.py:1211:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_constructors.py:360:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_constructors.py:371:58: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_constructors.py:382:41: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_constructors.py:526:47: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_constructors.py:547:47: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_constructors.py:655:59: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_conversion.py:68:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_conversion.py:170:42: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_conversion.py:179:42: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_drop.py:127:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_drop.py:147:60: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_get_set.py:362:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_join.py:134:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_join.py:146:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_join.py:158:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_monotonic.py:187:42: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_names.py:13:66: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_names.py:59:58: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_setops.py:497:43: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_setops.py:500:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_setops.py:503:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_setops.py:524:37: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_sorting.py:42:57: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[Unknown]`
- pandas/tests/indexes/multi/test_sorting.py:113:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_sorting.py:179:9: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_take.py:47:52: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_take.py:57:52: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexes/multi/test_take.py:67:52: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_chaining_and_caching.py:28:47: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_datetime.py:38:67: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_getitem.py:30:59: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_getitem.py:358:53: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_loc.py:597:44: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_loc.py:657:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_loc.py:668:53: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_loc.py:680:47: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_loc.py:684:52: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_loc.py:688:64: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_loc.py:805:64: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_multiindex.py:212:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_setitem.py:167:28: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_setitem.py:444:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_setitem.py:480:50: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_setitem.py:484:37: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_slice.py:64:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_slice.py:68:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_slice.py:395:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_slice.py:466:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_slice.py:563:55: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_slice.py:567:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/multiindex/test_sorted.py:24:54: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/test_loc.py:1772:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/indexing/test_loc.py:1812:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/excel/test_readers.py:1679:67: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/excel/test_writers.py:1004:61: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/excel/test_writers.py:1017:46: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/formats/style/test_to_latex.py:865:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/formats/style/test_to_latex.py:866:50: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/formats/test_format.py:1746:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/formats/test_to_html.py:513:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/formats/test_to_html.py:1020:46: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/formats/test_to_html.py:1031:46: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/generate_legacy_storage_files.py:167:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/generate_legacy_storage_files.py:181:66: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/generate_legacy_storage_files.py:209:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/parser/common/test_index.py:76:21: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/parser/test_index_col.py:227:63: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/parser/test_index_col.py:374:47: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/parser/test_na_values.py:240:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/parser/test_na_values.py:247:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/parser/test_parse_dates.py:348:21: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/parser/test_python_parser_only.py:200:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/pytables/test_put.py:295:59: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/pytables/test_select.py:738:59: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/io/test_stata.py:692:38: error[invalid-argument-type] Argument to function `raises` is incorrect: Expected `str | Pattern[str] | None`, found `Unknown | Hashable`
+ pandas/tests/io/test_stata.py:692:38: error[invalid-argument-type] Argument to function `raises` is incorrect: Expected `str | Pattern[str] | None`, found `Hashable`
- pandas/tests/plotting/frame/test_frame.py:208:13: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/plotting/frame/test_frame.py:211:53: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/plotting/test_boxplot_method.py:748:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/resample/test_resampler_grouper.py:440:57: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/concat/test_dataframe.py:147:75: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/concat/test_index.py:237:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/concat/test_series.py:107:66: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_join.py:953:52: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_join.py:958:52: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_join.py:1109:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_join.py:1113:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_join.py:1119:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_join.py:1126:48: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_merge.py:2535:53: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_merge.py:2890:56: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_merge_antijoin.py:157:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_merge_antijoin.py:166:51: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_merge_antijoin.py:178:52: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_merge_antijoin.py:190:52: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_multi.py:479:17: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_multi.py:484:46: error[invalid-argument-type] Argument to bound method `from_tuples` is incorrect: Expected `Hashable`, found `list[@Todo(list literal element type)]`
- pandas/tests/reshape/merge/test_multi.py:489:13: error[invalid-argument-type] Argument to bound m...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-07 12:34_

---

_Marked ready for review by @AlexWaygood on 2025-09-07 12:34_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-07 12:34_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-07 12:34_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-07 12:34_

---

_Comment by @github-actions[bot] on 2025-09-07 12:38_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 1 | 378 | 2 |
| `invalid-assignment` | 0 | 12 | 0 |
| `no-matching-overload` | 0 | 7 | 0 |
| `unused-ignore-comment` | 4 | 0 | 0 |
| `possibly-unbound-attribute` | 0 | 3 | 0 |
| `type-assertion-failure` | 0 | 3 | 0 |
| `unresolved-attribute` | 3 | 0 | 0 |
| `not-iterable` | 0 | 2 | 0 |
| `unsupported-operator` | 0 | 2 | 0 |
| `invalid-return-type` | 0 | 0 | 1 |
| `non-subscriptable` | 0 | 1 | 0 |
| `redundant-cast` | 1 | 0 | 0 |
| **Total** | **9** | **408** | **3** |

**[Full report with detailed diff](https://alex-hashable.ecosystem-663.pages.dev/diff)**


---

_Comment by @codspeed-hq[bot] on 2025-09-07 12:40_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fhashable?runnerMode=Instrumentation)

### Merging #20284 will **improve performances by 4.42%**

<sub>Comparing <code>alex/hashable</code> (c518a97) with <code>main</code> (9cb37db)</sub>



### Summary

`⚡ 1` improvements  
`✅ 42` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` DateType `` | 270.1 ms | 258.7 ms | +4.42% |


---

_Comment by @codspeed-hq[bot] on 2025-09-07 12:41_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fhashable?runnerMode=WallTime)

### Merging #20284 will **not alter performance**

<sub>Comparing <code>alex/hashable</code> (c518a97) with <code>main</code> (9cb37db)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Converted to draft by @AlexWaygood on 2025-09-07 12:51_

---

_Comment by @AlexWaygood on 2025-09-07 13:03_

Primer runs are currently failing due to GitLab being down for some planned maintenance, meaning some primer projects cannot be cloned... https://status.gitlab.com/

---

_@AlexWaygood reviewed on 2025-09-07 13:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1356 on 2025-09-07 13:10_

```suggestion

            // Any structural types that are "supertypes of `object`"
            // are in fact equivalent to `object`, and should therefore
            // be treated equivalently to `object` when it comes to assignability/subtyping
            (_, Type::ProtocolInstance(target))
```

---

_Marked ready for review by @AlexWaygood on 2025-09-07 13:11_

---

_Comment by @AlexWaygood on 2025-09-07 17:30_

> ```diff
> trio (https://github.com/python-trio/trio)
> + src/trio/_deprecate.py:49:19: error[unresolved-attribute] Type `<Protocol with members '__qualname__'>` has no attribute `__module__`
> ```

This seems to be a pre-existing bug where we don't recognise attributes available on `object` as being available on synthesized protocols... I'm attempting to fix it in https://github.com/astral-sh/ruff/pull/20286, but not totally sure what the correct fix is.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-08 12:05_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-08 12:05_

---

_Comment by @AlexWaygood on 2025-09-08 20:44_

Applying this diff locally buys back basically all the lost performance on the incremental benchmark:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 46aab242b2..fdaebb0bf9 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -1350,21 +1350,19 @@ impl<'db> Type<'db> {
                 C::always_satisfiable(db)
             }
 
-            // Any structural types that are "supertypes of `object`"
-            // are in fact equivalent to `object`, and should therefore
-            // be treated equivalently to `object` when it comes to assignability/subtyping
-            (_, Type::ProtocolInstance(target))
-                if Type::object(db)
-                    .satisfies_protocol(db, target, relation, visitor)
-                    .is_always_satisfied(db) =>
-            {
-                C::always_satisfiable(db)
-            }
-
             // `Never` is the bottom type, the empty set.
             // It is a subtype of all other types.
             (Type::Never, _) => C::always_satisfiable(db),
 
+            // `Any` is only a subtype of `object` -- but if a protocol's interface can be satisfied by `object`,
+            // then `object` is equivalent to the protocol, meaning that `Any` can also be a subtype of that
+            // equivalent-to-object protocol.
+            (Type::Dynamic(_), Type::ProtocolInstance(proto)) => {
+                C::from_bool(db, relation.is_assignability()).or(db, || {
+                    Type::object(db).satisfies_protocol(db, proto, relation, visitor)
+                })
+            }
+
             // Dynamic is only a subtype of `object` and only a supertype of `Never`; both were
             // handled above. It's always assignable, though.
             (Type::Dynamic(_), _) | (_, Type::Dynamic(_)) => {
diff --git a/crates/ty_python_semantic/src/types/instance.rs b/crates/ty_python_semantic/src/types/instance.rs
index 8714c824aa..c278b181d9 100644
--- a/crates/ty_python_semantic/src/types/instance.rs
+++ b/crates/ty_python_semantic/src/types/instance.rs
@@ -114,6 +114,13 @@ impl<'db> Type<'db> {
             .when_all(db, |member| {
                 member.is_satisfied_by(db, self, relation, visitor)
             })
+            .or(db, || {
+                let object = Type::object(db);
+                if self == object {
+                    return C::unsatisfiable(db);
+                }
+                object.satisfies_protocol(db, protocol, relation, visitor)
+            })
     }
 }
 
@@ -524,13 +531,16 @@ impl<'db> ProtocolInstanceType<'db> {
         self,
         db: &'db dyn Db,
         other: Self,
-        _relation: TypeRelation,
+        relation: TypeRelation,
         visitor: &HasRelationToVisitor<'db, C>,
     ) -> C {
         other
             .inner
             .interface(db)
             .is_sub_interface_of(db, self.inner.interface(db), visitor)
+            .or(db, || {
+                Type::object(db).satisfies_protocol(db, other, relation, visitor)
+            })
     }
```

But I'm really not a fan of how repetitive that makes the logic -- it means encoding in three separate places that protocols which are supertypes of `object` should be treated equivalently to `object` :-(

---

_Comment by @carljm on 2025-09-08 22:33_

Another option that might buy back the performance also is to eagerly simplify when constructing the protocol type (as we elsewhere prefer to do with equivalent types when we can), so that an annotation of `Hashable` is eagerly translated to `object`, and we don't have to keep checking that `object` satisfies it on any subtype check. That has the downside that if you have `x: Hashable` and then `reveal_type(x)`, it'll say `object` -- but this is just accurately reflecting our understanding of the type.

There's probably yet another option where we keep "equivalent to object" as a special flag on the protocol struct, and just check it once when the protocol is constructed rather than on every subtype check.

Maybe those won't actually be net positive for performance, if there are lots of protocol types constructed but then never actually checked for assignability or subtyping?

I think if those aren't good options, it's probably worth doing the repetitive version here (with comments cross-referencing the location). The regression is only >5% on the incremental benchmark, but it shows up as 1-2% across the board. Having to check a protocol against object literally every time it is used in a subtype or assignability check is pretty significant.

---

_Comment by @carljm on 2025-09-09 01:41_

Actually there might be another option here that gets the best of both worlds performance-wise, plus the DRYness -- if we make `is_equivalent_to_object` a cached Salsa query on a `ProtocolInstance`?

---

_Comment by @AlexWaygood on 2025-09-09 10:43_

> Another option that might buy back the performance also is to eagerly simplify when constructing the protocol type (as we elsewhere prefer to do with equivalent types when we can), so that an annotation of `Hashable` is eagerly translated to `object`, and we don't have to keep checking that `object` satisfies it on any subtype check. That has the downside that if you have `x: Hashable` and then `reveal_type(x)`, it'll say `object` -- but this is just accurately reflecting our understanding of the type.

Yeah, I considered that, but I think it'll just be a really confusing user experience if we produce error messages like this:

```py
from typing import Hashable

def f(x: Hashable):
    x.foo  # error: object of type `object` has no attribute `foo`
```

From a user's perspective: why on earth are we talking about `object` there??

---

_Comment by @AlexWaygood on 2025-09-09 11:24_

> Actually there might be another option here that gets the best of both worlds performance-wise, plus the DRYness -- if we make `is_equivalent_to_object` a cached Salsa query on a `ProtocolInstance`?

Nice, that seems to have done the trick! We're now seeing speedups on some of the benchmarks 🥳

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-09 13:16_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:516 on 2025-09-09 21:22_

Probably doesn't matter in practice, but I suspect this should be `true`, or we will fail to recognize some recursive protocols as equivalent to `object` when in fact they should be. The co-inductive assumption here should be `true` -- if there were any part of the protocol that `object` does not satisfy, we would still correctly end up with an overall `false`.

---

_@carljm approved on 2025-09-09 21:23_

Nice!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:516 on 2025-09-09 22:09_

Hmm I'm not sure there *are* any possible recursive protocols that would be equivalent to `object`? But I haven't tried very hard to think of any 😄

---

_@AlexWaygood reviewed on 2025-09-09 22:09_

---

_@carljm reviewed on 2025-09-09 22:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:516 on 2025-09-09 22:19_

If you take any protocol that is equivalent to object, and replace one of the types of its members with itself, I think that would do it?

---

_@AlexWaygood reviewed on 2025-09-09 22:21_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:516 on 2025-09-09 22:21_

Makes sense!

---

_@AlexWaygood reviewed on 2025-09-10 10:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:516 on 2025-09-10 10:38_

I added a regression test for recursive protocols that are supertypes of `object`. It didn't fail with the code as I had it before, but that may be just because we still don't look at the types of methods when doing assignability/subtyping checks for protocols (and this PR blocks doing that!). And the regression tests seem like a useful edge case to test explicitly, anyway.

I switched this to `true`, as per your suggestion!

---

_Merged by @AlexWaygood on 2025-09-10 10:38_

---

_Closed by @AlexWaygood on 2025-09-10 10:38_

---

_Branch deleted on 2025-09-10 10:39_

---
