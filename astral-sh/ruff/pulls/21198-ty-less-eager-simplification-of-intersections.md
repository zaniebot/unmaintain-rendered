```yaml
number: 21198
title: "[ty] Less eager simplification of intersections that include constrained type variables"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
draft: true
base: main
head: alex/dont-simplify-constraints
created_at: 2025-11-02T04:33:59Z
updated_at: 2025-11-02T04:49:46Z
url: https://github.com/astral-sh/ruff/pull/21198
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Less eager simplification of intersections that include constrained type variables

---

_Pull request opened by @AlexWaygood on 2025-11-02 04:33_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-11-02 04:34_

---

_Comment by @github-actions[bot] on 2025-11-02 04:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-02 04:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ mitmproxy/net/check.py:23:9: error[invalid-argument-type] Argument to bound method `decode` is incorrect: Expected `bytes`, found `AnyStr@is_valid_host`
+ mitmproxy/net/check.py:23:9: warning[possibly-missing-attribute] Attribute `decode` may be missing on object of type `AnyStr@is_valid_host & ~str`
+ mitmproxy/net/check.py:29:23: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str`, found `AnyStr@is_valid_host`
+ mitmproxy/net/check.py:29:23: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `bytes`, found `AnyStr@is_valid_host`
+ mitmproxy/net/check.py:29:43: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str | tuple[str, ...]`, found `Literal[b"."]`
+ mitmproxy/net/check.py:32:43: error[invalid-argument-type] Argument to bound method `split` is incorrect: Expected `bytes`, found `AnyStr@is_valid_host`
+ mitmproxy/net/check.py:32:43: error[no-matching-overload] No overload of bound method `split` matches arguments
+ mitmproxy/net/check.py:36:30: error[invalid-argument-type] Argument to bound method `decode` is incorrect: Expected `bytes`, found `AnyStr@is_valid_host`
+ mitmproxy/net/check.py:36:30: warning[possibly-missing-attribute] Attribute `decode` may be missing on object of type `(AnyStr@is_valid_host & ~str) | @Todo`
- Found 1888 diagnostics
+ Found 1897 diagnostics

artigraph (https://github.com/artigraph/artigraph)
- src/arti/internal/type_hints.py:177:37: error[invalid-argument-type] Argument to function `_check_issubclass` is incorrect: Expected `type`, found `T@lenient_issubclass & ~tuple[object, ...]`
- Found 169 diagnostics
+ Found 168 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/datasets/utils.py:416:16: error[invalid-return-type] Return type does not match returned value: expected `T@verify_str_arg`, found `str`
- torchvision/datasets/utils.py:426:12: error[invalid-return-type] Return type does not match returned value: expected `T@verify_str_arg`, found `str`
- Found 1491 diagnostics
+ Found 1489 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/annotations.py:1949:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
- tanjun/annotations.py:1996:74: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `(int | float) & ~int`
- tanjun/annotations.py:2110:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `_NumberT@__getitem__ & ~float`
- Found 143 diagnostics
+ Found 140 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/coding/cftime_offsets.py:1517:16: error[invalid-return-type] Return type does not match returned value: expected `T_FreqStr@_legacy_to_new_freq`, found `str & ~AlwaysFalsy`
- xarray/coding/cftime_offsets.py:1520:9: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1522:9: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1524:9: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1530:13: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1532:13: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1535:13: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1538:13: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1540:9: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1542:9: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1544:9: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1546:9: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1548:9: error[invalid-assignment] Object of type `str` is not assignable to `T_FreqStr@_legacy_to_new_freq`
- xarray/coding/cftime_offsets.py:1550:12: error[invalid-return-type] Return type does not match returned value: expected `T_FreqStr@_legacy_to_new_freq`, found `(str & ~AlwaysFalsy) | T_FreqStr@_legacy_to_new_freq`
+ xarray/coding/cftime_offsets.py:1519:49: error[unsupported-operator] Operator `not in` is not supported for types `str` and `T_FreqStr@_legacy_to_new_freq`, in comparing `Literal["ME"]` with `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1520:16: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1520:16: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1521:53: error[unsupported-operator] Operator `not in` is not supported for types `str` and `T_FreqStr@_legacy_to_new_freq`, in comparing `Literal["QE"]` with `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1522:16: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1522:16: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1523:52: error[unsupported-operator] Operator `not in` is not supported for types `str` and `T_FreqStr@_legacy_to_new_freq`, in comparing `Literal["YS"]` with `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1524:16: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1524:16: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1526:12: error[unsupported-operator] Operator `in` is not supported for types `str` and `T_FreqStr@_legacy_to_new_freq`, in comparing `Literal["A-"]` with `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1530:20: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1530:20: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1531:14: error[unsupported-operator] Operator `in` is not supported for types `str` and `T_FreqStr@_legacy_to_new_freq`, in comparing `Literal["Y-"]` with `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1532:20: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1532:20: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1533:14: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str`, found `T_FreqStr@_legacy_to_new_freq`
+ xarray/coding/cftime_offsets.py:1533:14: warning[possibly-missing-attribute] Attribute `endswith` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1535:20: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1535:20: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1536:14: error[unsupported-operator] Operator `not in` is not supported for types `str` and `T_FreqStr@_legacy_to_new_freq`, in comparing `Literal["YE"]` with `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1536:35: error[invalid-argument-type] Argument to bound method `endswith` is incorrect: Expected `str`, found `T_FreqStr@_legacy_to_new_freq`
+ xarray/coding/cftime_offsets.py:1536:35: warning[possibly-missing-attribute] Attribute `endswith` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1538:20: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1538:20: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1540:16: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1540:16: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1542:16: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1542:16: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1544:16: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1544:16: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1546:16: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1546:16: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
+ xarray/coding/cftime_offsets.py:1548:16: error[no-matching-overload] No overload of bound method `replace` matches arguments
+ xarray/coding/cftime_offsets.py:1548:16: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `T_FreqStr@_legacy_to_new_freq & ~AlwaysFalsy`
- xarray/computation/rolling.py:1192:19: error[invalid-argument-type] Argument to bound method `_to_temp_dataset` is incorrect: Expected `DataArray`, found `T_Xarray@Coarsen`
- xarray/computation/rolling.py:1216:20: error[invalid-argument-type] Argument to bound method `_from_temp_dataset` is incorrect: Argument type `T_Xarray@Coarsen` does not satisfy upper bound `DataArray` of type variable `Self`
+ xarray/core/indexes.py:1893:24: error[invalid-argument-type] Argument to bound method `create_variables` is incorrect: Expected `Index`, found `T_PandasOrXarrayIndex@Indexes`
+ xarray/core/indexes.py:1893:24: warning[possibly-missing-attribute] Attribute `create_variables` may be missing on object of type `PandasIndex | (T_PandasOrXarrayIndex@Indexes & ~Top[Index[Any]])`
- Found 1733 diagnostics
+ Found 1753 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/utilities/templating.py:92:47: error[invalid-argument-type] Argument to function `find_placeholders` is incorrect: Argument type `object` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/utilities/templating.py:94:47: error[invalid-argument-type] Argument to function `find_placeholders` is incorrect: Argument type `object` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/utilities/templating.py:166:20: error[invalid-return-type] Return type does not match returned value: expected `T@apply_values | type[NotSet]`, found `str`
- src/prefect/utilities/templating.py:199:25: error[invalid-assignment] Object of type `str` is not assignable to `T@apply_values`
- src/prefect/utilities/templating.py:201:21: error[invalid-assignment] Object of type `str` is not assignable to `T@apply_values`
- src/prefect/utilities/templating.py:203:20: error[invalid-return-type] Return type does not match returned value: expected `T@apply_values | type[NotSet]`, found `str | T@apply_values`
- src/prefect/utilities/templating.py:207:29: error[no-matching-overload] No overload of function `apply_values` matches arguments
- src/prefect/utilities/templating.py:214:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, Any].__setitem__(key: str, value: Any, /) -> None` cannot be called with a key of type `object` and a value of type `Unknown & ~<class 'NotSet'>` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:216:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, Any].__setitem__(key: str, value: Any, /) -> None` cannot be called with a key of type `object` and a value of type `object` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:222:29: error[no-matching-overload] No overload of function `apply_values` matches arguments
- src/prefect/utilities/templating.py:311:29: error[no-matching-overload] No overload of bound method `get` matches arguments
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, Any].__setitem__(key: str, value: Any, /) -> None` cannot be called with a key of type `object` and a value of type `Unknown` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:336:20: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `str`
- src/prefect/utilities/templating.py:412:20: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `str`
- src/prefect/utilities/templating.py:432:25: error[invalid-assignment] Object of type `str` is not assignable to `T@resolve_variables`
- src/prefect/utilities/templating.py:434:25: error[invalid-assignment] Object of type `str` is not assignable to `T@resolve_variables`
- src/prefect/utilities/templating.py:435:20: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `str | T@resolve_variables`
- Found 3301 diagnostics
+ Found 3284 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/devicetools.py:2541:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[t@_make_tuple | None, t@_make_tuple | None]`, found `tuple[None | str | int, None | str | int]`
- hydpy/core/devicetools.py:2724:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[t@_make_tuple | None, t@_make_tuple | None]`, found `tuple[None | str | int, None | str | int]`
- Found 678 diagnostics
+ Found 676 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_values.py:176:20: error[invalid-return-type] Return type does not match returned value: expected `TVContainer_co@InterfaceValues`, found `@Todo | IndexHierarchy | Series[Any, Any] | Index[Any]`
- static_frame/core/node_values.py:178:23: warning[possibly-missing-attribute] Attribute `index` may be missing on object of type `TVContainer_co@InterfaceValues`
- Found 1989 diagnostics
+ Found 1987 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/algorithms.py:214:16: error[invalid-return-type] Return type does not match returned value: expected `ArrayLikeT@_reconstruct_data`, found `ExtensionArray`
- pandas/core/arrays/datetimes.py:2891:13: error[invalid-assignment] Object of type `Timestamp` is not assignable to `_TimestampNoneT1@_maybe_normalize_endpoints`
- pandas/core/arrays/datetimes.py:2894:13: error[invalid-assignment] Object of type `Timestamp` is not assignable to `_TimestampNoneT2@_maybe_normalize_endpoints`
+ pandas/core/arrays/datetimes.py:2891:21: error[invalid-argument-type] Argument to bound method `normalize` is incorrect: Argument type `_TimestampNoneT1@_maybe_normalize_endpoints` does not satisfy upper bound `Timestamp` of type variable `Self`
+ pandas/core/arrays/datetimes.py:2891:21: warning[possibly-missing-attribute] Attribute `normalize` may be missing on object of type `_TimestampNoneT1@_maybe_normalize_endpoints & ~None`
+ pandas/core/arrays/datetimes.py:2894:19: error[invalid-argument-type] Argument to bound method `normalize` is incorrect: Argument type `_TimestampNoneT2@_maybe_normalize_endpoints` does not satisfy upper bound `Timestamp` of type variable `Self`
+ pandas/core/arrays/datetimes.py:2894:19: warning[possibly-missing-attribute] Attribute `normalize` may be missing on object of type `_TimestampNoneT2@_maybe_normalize_endpoints & ~None`
+ pandas/io/stata.py:2257:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr@_pad_bytes & ~bytes` and `LiteralString`
- pandas/io/stata.py:2256:16: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@_pad_bytes`, found `bytes`
- pandas/io/stata.py:2257:12: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@_pad_bytes`, found `str`

```
</details>
No memory usage changes detected ✅


---

_Closed by @AlexWaygood on 2025-11-02 04:49_

---

_Branch deleted on 2025-11-02 04:49_

---
