```yaml
number: 21893
title: "[ty] Generics: Respect typevar bounds when matching against a union"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1837
created_at: 2025-12-10T12:16:48Z
updated_at: 2025-12-10T13:59:00Z
url: https://github.com/astral-sh/ruff/pull/21893
synced_at: 2026-01-12T15:57:36Z
```

# [ty] Generics: Respect typevar bounds when matching against a union

---

_@sharkdp_

## Summary

Respect typevar bounds and constraints when matching against a union. For example:

```py
def accepts_t_or_int[T_str: str](x: T_str | int) -> T_str:
    raise NotImplementedError

reveal_type(accepts_t_or_int("a"))  # ok, reveals `Literal["a"]`
reveal_type(accepts_t_or_int(1))  # ok, reveals `Unknown`

class Unrelated: ...

# error: [invalid-argument-type] "Argument type `Unrelated` does not
# satisfy upper bound `str` of type variable `T_str`"
accepts_t_or_int(Unrelated())
```

Previously, the last call succeed without any errors. Worse than that, we also incorrectly solved `T_str = Unrelated`, which often lead to downstream errors.

closes https://github.com/astral-sh/ty/issues/1837

## Ecosystem impact

Looks good!

* Lots of removed false positives, often because we previously selected a wrong overload for a generic function (because we didn't respect the typevar bound in an earlier overload).
* We now understand calls to functions accepting an argument of type `GenericPath: TypeAlias = AnyStr | PathLike[AnyStr]`. Previously, we would incorrectly match a `Path` argument against the `AnyStr` typevar (violating its constraints), but now we match against `PathLike`.

## Performance

Another regression on `colour`. This package uses `numpy` heavily. And `numpy` is the codebase that originally lead me to this bug. The fix here allows us to infer more precise `np.array` types in some cases, so it's reasonable that we just need to perform more work.

The fix here also requires us to look at more union elements when we would previously short-circuit incorrectly, so some more work needs to be done in the solver.

## Test Plan

New Markdown tests


---

_Label `bug` added by @sharkdp on 2025-12-10 12:16_

---

_Label `ty` added by @sharkdp on 2025-12-10 12:16_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-10 12:16_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 12:18_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-10 12:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/environment/environment.py:590:24: error[unresolved-attribute] Object of type `Path` has no attribute `startswith`
- Found 7971 diagnostics
+ Found 7970 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:1676:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
+ src/graphql/execution/execute.py:1676:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
- src/graphql/execution/execute.py:1828:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[CoroutineType[Any, Any, StreamItemResult]]`
+ src/graphql/execution/execute.py:1828:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[CoroutineType[Any, Any, StreamItemResult] | StreamItemResult]`

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/listing.py:300:19: error[unsupported-operator] Operator `/` is unsupported between objects of type `LocalPath` and `LocalPath`
- boostedblob/listing.py:300:29: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `LocalPath`
- Found 21 diagnostics
+ Found 19 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/pathlib.py:177:12: error[unresolved-attribute] Object of type `Path` has no attribute `lower`
- Found 443 diagnostics
+ Found 442 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/client.py:3689:28: error[invalid-return-type] Return type does not match returned value: expected `tuple[dict[Ref, ObjectID | None], set[bytes], str, dict[Ref, Ref], dict[Ref, ObjectID]]`, found `tuple[dict[Ref, ObjectID | None] | dict[Ref, ObjectID], set[bytes], str | @Todo, dict[Ref, Ref], dict[Ref, ObjectID]]`
+ dulwich/client.py:3689:28: error[invalid-return-type] Return type does not match returned value: expected `tuple[dict[Ref, ObjectID | None], set[bytes], str, dict[Ref, Ref], dict[Ref, ObjectID]]`, found `tuple[dict[Ref, ObjectID | None] | dict[Ref, ObjectID], set[bytes], str, dict[Ref, Ref], dict[Ref, ObjectID]]`

pandera (https://github.com/pandera-dev/pandera)
- tests/pandas/test_dtypes.py:169:43: error[invalid-assignment] Object of type `list[tuple[dict[Unknown, Unknown], list[Unknown]] | tuple[dict[Unknown | <class 'datetime'> | <class 'datetime64'> | ... omitted 4 union elements, Unknown | str], Series[pandas._libs.tslibs.timestamps.Timestamp]] | tuple[dict[Unknown | PeriodDtype, Unknown | str], Series[Period]] | tuple[dict[Unknown | <class 'SparseDtype'> | SparseDtype, Unknown | SparseDtype], Series[list[Unknown | int | None]]] | tuple[dict[Unknown | IntervalDtype, Unknown | str], Series[Interval[int | float]]]]` is not assignable to `list[tuple[dict[Unknown, Unknown], list[Unknown]]]`
+ tests/pandas/test_dtypes.py:169:43: error[invalid-assignment] Object of type `list[tuple[dict[Unknown, Unknown], list[Unknown]] | tuple[dict[Unknown | <class 'datetime'> | <class 'datetime64'> | ... omitted 4 union elements, Unknown | str], Series[pandas._libs.tslibs.timestamps.Timestamp]] | tuple[dict[Unknown | PeriodDtype, Unknown | str], Series[Period]] | tuple[dict[Unknown | <class 'SparseDtype'> | SparseDtype, Unknown | SparseDtype], Series[Any]] | tuple[dict[Unknown | IntervalDtype, Unknown | str], Series[Interval[int | float]]]]` is not assignable to `list[tuple[dict[Unknown, Unknown], list[Unknown]]]`
- tests/pandas/test_schemas.py:157:39: error[invalid-argument-type] Argument to bound method `validate` is incorrect: Expected `DataFrame`, found `Series[Any]`
- tests/pandas/test_schemas.py:175:25: error[invalid-argument-type] Argument to bound method `validate` is incorrect: Expected `DataFrame`, found `Series[Any]`
- tests/pandas/test_schemas.py:177:25: error[invalid-argument-type] Argument to bound method `validate` is incorrect: Expected `DataFrame`, found `Series[Any]`
- Found 1615 diagnostics
+ Found 1612 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/prototype/datasets/_folder.py:51:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[Unknown, list[str]]`, found `tuple[Unknown, list[Path]]`
- Found 1476 diagnostics
+ Found 1475 diagnostics

antidote (https://github.com/Finistere/antidote)
- tests/lib/interface/test_class.py:282:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_conditions.py:150:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_conditions.py:193:59: error[invalid-argument-type] Argument to bound method `when` is incorrect: Expected `Predicate[Weighted] | Predicate[NeutralWeight] | Weighted | ... omitted 3 union elements`, found `Weight | None`
- Found 278 diagnostics
+ Found 275 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/data/converter/converter.py:153:18: error[invalid-assignment] Object of type `Series[Any]` is not assignable to `DataFrame`
- freqtrade/data/converter/converter.py:155:14: error[invalid-assignment] Object of type `Series[Any]` is not assignable to `DataFrame`
- freqtrade/data/converter/trade_converter.py:89:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any]`
- freqtrade/freqai/data_kitchen.py:389:18: error[invalid-assignment] Object of type `Series[Any]` is not assignable to `DataFrame`
- freqtrade/freqai/data_kitchen.py:391:18: error[invalid-assignment] Object of type `Series[Any]` is not assignable to `DataFrame`
- freqtrade/rpc/rpc.py:1435:29: error[invalid-assignment] Object of type `Series[Any]` is not assignable to `DataFrame`
- Found 689 diagnostics
+ Found 683 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- pyodide-build/pyodide_build/recipe/unvendor.py:101:37: error[invalid-argument-type] Argument to function `fnmatchcase` is incorrect: Argument type `Path` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- pyodide-build/pyodide_build/recipe/unvendor.py:101:37: error[invalid-argument-type] Argument to function `fnmatchcase` is incorrect: Expected `str`, found `Path`
- pyodide-build/pyodide_build/recipe/unvendor.py:102:40: error[invalid-argument-type] Argument to function `fnmatchcase` is incorrect: Argument type `Path` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- pyodide-build/pyodide_build/recipe/unvendor.py:102:40: error[invalid-argument-type] Argument to function `fnmatchcase` is incorrect: Expected `str`, found `Path`
- pyodide-build/pyodide_build/recipe/unvendor.py:105:44: error[invalid-argument-type] Argument to function `fnmatchcase` is incorrect: Argument type `Path` does not satisfy constraints (`str`, `bytes`) of type variable `AnyStr`
- pyodide-build/pyodide_build/recipe/unvendor.py:105:44: error[invalid-argument-type] Argument to function `fnmatchcase` is incorrect: Expected `str`, found `Path`
- Found 876 diagnostics
+ Found 870 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_distutils/command/install.py:671:45: error[invalid-argument-type] Argument to function `change_root` is incorrect: Expected `str | PathLike[str]`, found `str | None`
+ setuptools/_distutils/tests/test_version.py:55:42: error[invalid-argument-type] Argument to bound method `_cmp` is incorrect: Argument type `object` does not satisfy upper bound `StrictVersion` of type variable `Self`
+ setuptools/_distutils/tests/test_version.py:55:42: error[invalid-argument-type] Argument to bound method `_cmp` is incorrect: Expected `StrictVersion | str`, found `object`
+ setuptools/_distutils/tests/test_version.py:77:41: error[invalid-argument-type] Argument to bound method `_cmp` is incorrect: Argument type `object` does not satisfy upper bound `LooseVersion` of type variable `Self`
+ setuptools/_distutils/tests/test_version.py:77:41: error[invalid-argument-type] Argument to bound method `_cmp` is incorrect: Expected `LooseVersion | str`, found `object`
- Found 1284 diagnostics
+ Found 1289 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/sources/DataSourceWSL.py:399:13: error[unresolved-attribute] Object of type `PurePath` has no attribute `casefold`
- Found 1180 diagnostics
+ Found 1179 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/schema/schema.py:826:69: error[invalid-assignment] Object of type `CoroutineType[Any, Any, AsyncIterator[ExecutionResult] | ExecutionResult]` is not assignable to `ExecutionResult | AsyncIterator[ExecutionResult]`
+ strawberry/schema/schema.py:826:69: error[invalid-assignment] Object of type `AsyncIterator[ExecutionResult] | ExecutionResult | CoroutineType[Any, Any, AsyncIterator[ExecutionResult] | ExecutionResult]` is not assignable to `ExecutionResult | AsyncIterator[ExecutionResult]`

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/internal/test_module.py:560:16: error[unresolved-attribute] Object of type `Path` has no attribute `endswith`
- tests/internal/test_module.py:562:57: error[non-subscriptable] Cannot subscript object of type `Path` with no `__getitem__` method
- Found 8473 diagnostics
+ Found 8471 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 43 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/coding/strings.py:243:12: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- xarray/groupers.py:556:23: error[no-matching-overload] No overload of bound method `groupby` matches arguments
- xarray/tests/test_rolling.py:203:21: error[invalid-argument-type] Argument to bound method `rolling` is incorrect: Argument type `Series[ndarray[tuple[int], dtype[signedinteger[Any]]]]` does not satisfy upper bound `Series[S1@Series]` of type variable `Self`
- xarray/tests/test_rolling.py:222:21: error[invalid-argument-type] Argument to bound method `rolling` is incorrect: Argument type `Series[ndarray[tuple[int], dtype[signedinteger[Any]]]]` does not satisfy upper bound `Series[S1@Series]` of type variable `Self`
- xarray/tests/test_variable.py:2825:36: error[invalid-assignment] Object of type `Series[Index[Any]]` is not assignable to `T_DuckArray@TestAsCompatibleData`
+ xarray/tests/test_variable.py:2825:36: error[invalid-assignment] Object of type `Series[Any]` is not assignable to `T_DuckArray@TestAsCompatibleData`
- Found 1758 diagnostics
+ Found 1754 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
- tests/special/test_logsumexp.pyi:23:1: error[type-assertion-failure] Type `float64` does not match asserted type `list[int | float]`
+ tests/special/test_logsumexp.pyi:23:1: error[type-assertion-failure] Type `float64` does not match asserted type `Unknown`
- tests/special/test_logsumexp.pyi:25:1: error[type-assertion-failure] Type `complex128` does not match asserted type `list[int | float | complex]`
+ tests/special/test_logsumexp.pyi:25:1: error[type-assertion-failure] Type `complex128` does not match asserted type `Unknown`
- tests/special/test_logsumexp.pyi:27:1: error[type-assertion-failure] Type `floating[_16Bit]` does not match asserted type `ndarray[tuple[int], dtype[floating[_16Bit]]]`
+ tests/special/test_logsumexp.pyi:27:1: error[type-assertion-failure] Type `floating[_16Bit]` does not match asserted type `Unknown`
- tests/special/test_logsumexp.pyi:29:1: error[type-assertion-failure] Type `complexfloating[_32Bit, _32Bit]` does not match asserted type `ndarray[tuple[int], dtype[complexfloating[_32Bit, _32Bit]]]`
+ tests/special/test_logsumexp.pyi:29:1: error[type-assertion-failure] Type `complexfloating[_32Bit, _32Bit]` does not match asserted type `Unknown`
- tests/special/test_logsumexp.pyi:50:1: error[type-assertion-failure] Type `tuple[float64, float64]` does not match asserted type `tuple[list[int | float], list[int | float]]`
+ tests/special/test_logsumexp.pyi:50:1: error[type-assertion-failure] Type `tuple[float64, float64]` does not match asserted type `tuple[Unknown, Unknown]`
- tests/special/test_logsumexp.pyi:52:1: error[type-assertion-failure] Type `tuple[float64, complex128]` does not match asserted type `tuple[list[int | float | complex], list[int | float | complex]]`
+ tests/special/test_logsumexp.pyi:52:1: error[type-assertion-failure] Type `tuple[float64, complex128]` does not match asserted type `tuple[Unknown, Unknown]`
- tests/special/test_logsumexp.pyi:54:1: error[type-assertion-failure] Type `tuple[floating[_16Bit], floating[_16Bit]]` does not match asserted type `tuple[ndarray[tuple[int], dtype[floating[_16Bit]]], ndarray[tuple[int], dtype[floating[_16Bit]]]]`
+ tests/special/test_logsumexp.pyi:54:1: error[type-assertion-failure] Type `tuple[floating[_16Bit], floating[_16Bit]]` does not match asserted type `tuple[Unknown, Unknown]`
- tests/special/test_logsumexp.pyi:55:1: error[type-assertion-failure] Type `tuple[floating[Any], complexfloating[_32Bit, _32Bit]]` does not match asserted type `tuple[complexfloating[_32Bit, _32Bit], complexfloating[_32Bit, _32Bit]]`
+ tests/special/test_logsumexp.pyi:55:1: error[type-assertion-failure] Type `tuple[floating[Any], complexfloating[_32Bit, _32Bit]]` does not match asserted type `tuple[Unknown, Unknown]`
- tests/special/test_logsumexp.pyi:56:1: error[type-assertion-failure] Type `tuple[floating[Any], complexfloating[_32Bit, _32Bit]]` does not match asserted type `tuple[ndarray[tuple[int], dtype[complexfloating[_32Bit, _32Bit]]], ndarray[tuple[int], dtype[complexfloating[_32Bit, _32Bit]]]]`
+ tests/special/test_logsumexp.pyi:56:1: error[type-assertion-failure] Type `tuple[floating[Any], complexfloating[_32Bit, _32Bit]]` does not match asserted type `tuple[Unknown, Unknown]`
- tests/stats/test_mode.pyi:30:1: error[type-assertion-failure] Type `float64` does not match asserted type `list[int | float]`
+ tests/stats/test_mode.pyi:30:1: error[type-assertion-failure] Type `float64` does not match asserted type `integer[Any] | floating[Any]`
- tests/stats/test_mode.pyi:35:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `list[list[int | float]]`
+ tests/stats/test_mode.pyi:35:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `integer[Any] | floating[Any]`
- tests/stats/test_mode.pyi:45:1: error[type-assertion-failure] Type `float64` does not match asserted type `list[int | float]`
+ tests/stats/test_mode.pyi:45:1: error[type-assertion-failure] Type `float64` does not match asserted type `integer[Any] | floating[Any]`
- tests/stats/test_mode.pyi:50:1: error[type-assertion-failure] Type `float64` does not match asserted type `list[list[int | float]]`
+ tests/stats/test_mode.pyi:50:1: error[type-assertion-failure] Type `float64` does not match asserted type `integer[Any] | floating[Any]`

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/arrays/_mixins.py:135:20: error[invalid-return-type] Return type does not match returned value: expected `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]]`, found `ndarray[tuple[Any, ...], type]`
- pandas/core/arrays/datetimelike.py:1142:42: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[datetime64[date | int | None]]]`, found `ndarray[tuple[Any, ...], str]`
- pandas/core/arrays/datetimelike.py:1204:43: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[Any, ...], str]`
- pandas/core/arrays/datetimelike.py:1313:20: error[invalid-return-type] Return type does not match returned value: expected `ndarray[tuple[Any, ...], dtype[Any]]`, found `ndarray[tuple[int, ...], dtype[Any] | str]`
- pandas/core/arrays/datetimelike.py:1769:47: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[Any, ...], str]`
- pandas/core/arrays/datetimes.py:534:32: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[datetime64[date | int | None]]]`, found `ndarray[tuple[Any, ...], str]`
- pandas/core/arrays/datetimes.py:1115:65: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], str], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], str], (key: str, /) -> ndarray[tuple[Any, ...], dtype[Any]], (key: list[str], /) -> ndarray[tuple[Any, ...], dtype[void]]]` cannot be called with key of type `Literal[0]` on object of type `ndarray[tuple[Any, ...], str]`
- pandas/core/arrays/datetimes.py:1122:33: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[datetime64[date | int | None]]]`, found `ndarray[tuple[Any, ...], str]`
- pandas/core/arrays/masked.py:185:30: error[invalid-assignment] Object of type `ndarray[tuple[int, ...], dtype[Any] | type]` is not assignable to `ndarray[tuple[Any, ...], dtype[Any]]`
- pandas/core/arrays/timedeltas.py:324:32: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[Any, ...], str] | Unknown`
- pandas/core/dtypes/missing.py:392:12: error[unsupported-operator] Unary operator `~` is unsupported for type `~bool`
+ pandas/core/indexes/datetimelike.py:995:45: error[unsupported-operator] Operator `-` is unsupported between objects of type `Timestamp | NaTType | Timedelta` and `BaseOffset`
- pandas/core/internals/managers.py:2501:42: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[datetime64[date | int | None]]]`, found `ndarray[tuple[int, ...], str]`
- pandas/core/tools/datetimes.py:454:41: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[datetime64[date | int | None]]]`, found `ndarray[tuple[Any, ...], str]`
- pandas/core/util/hashing.py:314:16: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/plotting/_matplotlib/converter.py:785:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["val"]` and value of type `ndarray[tuple[int], dtype[signedinteger[Any]]]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:786:14: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["val"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:787:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["fmt"]` and value of type `Literal[""]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:789:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["maj"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:790:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["fmt"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:794:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["min"]` and value of type `Literal[True]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:811:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["fmt"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:812:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["min"]` and value of type `Literal[True]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:819:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["min"]` and value of type `Literal[True]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:828:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["min"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:838:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["min"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:842:12: error[invalid-return-type] Return type does not match returned value: expected `ndarray[tuple[Any, ...], dtype[Any]]`, found `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:855:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["val"]` and value of type `ndarray[tuple[int], dtype[signedinteger[Any]]]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:856:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["fmt"]` and value of type `Literal[""]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:857:14: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["val"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:858:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["maj"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:859:16: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["fmt"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:864:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["min"]` and value of type `Literal[True]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:877:9: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["min"]` and value of type `Literal[True]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:887:9: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["min"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:890:12: error[invalid-return-type] Return type does not match returned value: expected `ndarray[tuple[Any, ...], dtype[Any]]`, found `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:902:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["val"]` and value of type `ndarray[tuple[int], dtype[signedinteger[Any]]]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:903:5: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["fmt"]` and value of type `Literal[""]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:904:14: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["val"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:909:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["maj"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:910:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["min"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:911:5: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["fmt"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/plotting/_matplotlib/converter.py:913:12: error[invalid-return-type] Return type does not match returned value: expected `ndarray[tuple[Any, ...], dtype[Any]]`, found `ndarray[tuple[int], list[Unknown | tuple[str, <class 'int'>] | tuple[str, <class 'bool'>] | tuple[str, str]]]`
- pandas/tests/arrays/test_datetimes.py:62:41: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[datetime64[date | int | None]]]`, found `ndarray[tuple[int], str]`
- pandas/tests/arrays/test_datetimes.py:109:41: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[datetime64[date | int | None]]]`, found `ndarray[tuple[int], str]`
- pandas/tests/arrays/test_datetimes.py:114:39: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[datetime64[date | int | None]]]`, found `ndarray[tuple[int], str]`
- pandas/tests/arrays/test_timedeltas.py:23:43: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[int], str]`
- pandas/tests/arrays/test_timedeltas.py:27:42: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[int], str]`
- pandas/tests/base/test_value_counts.py:300:14: error[unsupported-operator] Operator `+` is unsupported between objects of type `ndarray[tuple[int], str]` and `Timedelta`
- pandas/tests/base/test_value_counts.py:314:11: error[unsupported-operator] Operator `+` is unsupported between objects of type `Timedelta` and `ndarray[tuple[int], str]`
- pandas/tests/io/formats/test_to_string.py:600:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]] | tuple[ndarray[tuple[Any, ...], dtype[integer[Any] | bool[bool]]], ...], /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'object'>]]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 5 union elements, /) -> ndarray[tuple[Any, ...], list[Unknown | tuple[str, <class 'object'>]]], (key: str, /) -> ndarray[tuple[int], dtype[Any]], (key: list[str], /) -> ndarray[tuple[int], dtype[void]]]` cannot be called with key of type `Literal["err"]` on object of type `ndarray[tuple[int], list[Unknown | tuple[str, <class 'object'>]]]`
- pandas/tests/scalar/interval/test_contains.py:29:16: error[unsupported-operator] Operator `in` is not supported between two objects of type `Interval[int]`
- pandas/tests/scalar/interval/test_contains.py:30:16: error[unsupported-operator] Operator `in` is not supported between two objects of type `Interval[int]`
- pandas/tests/scalar/interval/test_contains.py:31:16: error[unsupported-operator] Operator `in` is not supported between two objects of type `Interval[int]`
- pandas/tests/scalar/interval/test_contains.py:32:16: error[unsupported-operator] Operator `not in` is not supported between two objects of type `Interval[int]`
- pandas/tests/scalar/interval/test_contains.py:44:16: error[unsupported-operator] Operator `not in` is not supported between two objects of type `Interval[int]`
- pandas/tests/scalar/interval/test_contains.py:47:16: error[unsupported-operator] Operator `not in` is not supported between two objects of type `Interval[int]`
- pandas/tests/scalar/interval/test_contains.py:69:20: error[unsupported-operator] Operator `in` is not supported between two objects of type `Interval[int]`
- pandas/tests/scalar/interval/test_contains.py:73:17: error[unsupported-operator] Operator `in` is not supported between two objects of type `Interval[int]`
+ pandas/tests/scalar/timedelta/test_arithmetic.py:728:13: error[unsupported-operator] Operator `//` is unsupported between objects of type `unsignedinteger[_8Bit]` and `Timedelta`
+ pandas/tests/scalar/timedelta/test_arithmetic.py:731:13: error[unsupported-operator] Operator `//` is unsupported between objects of type `signedinteger[_32Bit]` and `Timedelta`
- pandas/tests/test_nanops.py:1165:15: error[no-matching-overload] No overload of bound method `reshape` matches arguments
- pandas/tests/tslibs/test_timedeltas.py:134:31: error[invalid-argument-type] Argument to function `ints_to_pytimedelta` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[int], str]`
- pandas/tests/tslibs/test_timedeltas.py:137:16: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/tests/tslibs/test_timedeltas.py:140:31: error[invalid-argument-type] Argument to function `ints_to_pytimedelta` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[int], str]`
- pandas/tests/tslibs/test_timedeltas.py:150:29: error[invalid-argument-type] Argument to function `ints_to_pytimedelta` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[int], str]`
- pandas/tests/tslibs/test_timedeltas.py:153:29: error[invalid-argument-type] Argument to function `ints_to_pytimedelta` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[int], str]`
- Found 3763 diagnostics
+ Found 3702 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/layouts.py:189:37: error[invalid-argument-type] Argument to function `_parse_children_arg` is incorrect: Argument type `UIElement` does not satisfy upper bound `LayoutDOM` of type variable `L`
- src/bokeh/models/annotations/annotation.py:67:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[DataSource | InstanceDefault[ColumnDataSource]]`
+ src/bokeh/models/annotations/annotation.py:67:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `InstanceDefault[ColumnDataSource]` does not satisfy upper bound `Serializable` of type variable `S`
+ src/bokeh/models/annotations/annotation.py:67:35: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DataSource | UndefinedType | IntrinsicType`, found `InstanceDefault[ColumnDataSource]`
+ src/bokeh/models/annotations/geometry.py:298:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `() -> Unknown` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/annotations/geometry.py:298:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[BoxInteractionHandles | (() -> Unknown)]`
+ src/bokeh/models/annotations/geometry.py:298:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `BoxInteractionHandles | UndefinedType | IntrinsicType`, found `() -> Unknown`
+ src/bokeh/models/annotations/legends.py:170:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `InstanceDefault[NoOverlap]` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/annotations/legends.py:170:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[LabelingPolicy | InstanceDefault[NoOverlap]]`
+ src/bokeh/models/annotations/legends.py:170:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `LabelingPolicy | UndefinedType | IntrinsicType`, found `InstanceDefault[NoOverlap]`
+ src/bokeh/models/annotations/legends.py:592:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `InstanceDefault[MetricLength]` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/annotations/legends.py:592:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Dimensional | InstanceDefault[MetricLength]]`
+ src/bokeh/models/annotations/legends.py:592:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Dimensional | UndefinedType | IntrinsicType`, found `InstanceDefault[MetricLength]`
+ src/bokeh/models/annotations/legends.py:722:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `InstanceDefault[FixedTicker]` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/annotations/legends.py:722:14: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Ticker | InstanceDefault[FixedTicker]]`
+ src/bokeh/models/annotations/legends.py:722:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Ticker | UndefinedType | IntrinsicType`, found `InstanceDefault[FixedTicker]`
+ src/bokeh/models/annotations/legends.py:823:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `InstanceDefault[NoOverlap]` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/annotations/legends.py:823:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[LabelingPolicy | InstanceDefault[NoOverlap]]`
+ src/bokeh/models/annotations/legends.py:823:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `LabelingPolicy | UndefinedType | IntrinsicType`, found `InstanceDefault[NoOverlap]`
+ src/bokeh/models/axes.py:201:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `InstanceDefault[AllLabels]` does not satisfy upper bound `Serializable` of type variable `S`
- src/bokeh/models/axes.py:201:26: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[LabelingPolicy | InstanceDefault[AllLabels]]`
+ src/bokeh/models/axes.py:201:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `LabelingPolicy | UndefinedType | IntrinsicType`, found `InstanceDefault[AllLabels]`
- src/bokeh/models/coordinates.py:48:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Range | InstanceDefault[DataRange1d]]`
- src/bokeh/models/coordinates.py:52:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Range | InstanceDefault[DataRange1d]]`
- src/bokeh/models/coordinates.py:56:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Scale | InstanceDefault[LinearScale]]`
- src/bokeh/models/coordinates.py:61:15: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance[S@Instance]`, found `Instance[Scale | InstanceDefault[LinearScale]]`
+ src/bokeh/models/coordinates.py:48:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `InstanceDefault[DataRange1d]` does not satisfy upper bound `Serializable` of type variable `S`
+ src/bokeh/models/coordinates.py:48:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Range | UndefinedType | IntrinsicType`, found `InstanceDefault[DataRange1d]`
+ src/bokeh/models/coordinates.py:52:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `InstanceDefault[DataRange1d]` does not satisfy upper bound `Serializable` of type variable `S`
+ src/bokeh/models/coordinates.py:52:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Range | UndefinedType | IntrinsicType`, found `InstanceDefault[DataRange1d]`
+ src/bokeh/models/coordinates.py:56:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `InstanceDefault[LinearScale]` does not satisfy upper bound `Serializable` of type variable `S`
+ src/bokeh/models/coordinates.py:56:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Scale | UndefinedType | IntrinsicType`, found `InstanceDefault[LinearScale]`
+ src/bokeh/models/coordinates.py:61:31: error[invalid-argument-type] Argument to bound method `__ini

... (truncated 693 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-10 12:26_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 2 | 121 | 90 |
| `invalid-argument-type` | 41 | 86 | 36 |
| `no-matching-overload` | 2 | 96 | 0 |
| `unsupported-operator` | 4 | 64 | 1 |
| `invalid-assignment` | 1 | 24 | 4 |
| `unused-ignore-comment` | 2 | 26 | 0 |
| `possibly-missing-attribute` | 3 | 21 | 0 |
| `invalid-return-type` | 0 | 18 | 2 |
| `unresolved-attribute` | 2 | 13 | 4 |
| `non-subscriptable` | 0 | 5 | 0 |
| `missing-argument` | 0 | 4 | 0 |
| `not-iterable` | 0 | 2 | 0 |
| `unsupported-base` | 0 | 1 | 0 |
| **Total** | **57** | **481** | **137** |

**[Full report with detailed diff](https://david-fix-1837.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1837.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-12-10 12:51_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-12-10 12:51_

---

_Comment by @codspeed-hq[bot] on 2025-12-10 13:11_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1837?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21893 will **degrade performances by 9.79%**

<sub>Comparing <code>david/fix-1837</code> (7a85fe2) with <code>main</code> (ff7086d)</sub>



### Summary

`❌ 1 (👁 1)` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1837?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 98.5 s | 109.2 s | -9.79% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1837?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Marked ready for review by @sharkdp on 2025-12-10 13:28_

---

_Review requested from @carljm by @sharkdp on 2025-12-10 13:28_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-10 13:28_

---

_Review requested from @dcreager by @sharkdp on 2025-12-10 13:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:1600 on 2025-12-10 13:39_

Does this work? Might help perf

```suggestion
                            found_matching_element = true;
                            break;
```

---

_@AlexWaygood reviewed on 2025-12-10 13:39_

---

_@AlexWaygood approved on 2025-12-10 13:40_

LGTM. `colour` is known to have pathological types, that's why we added it as a benchmark :/

---

_@sharkdp reviewed on 2025-12-10 13:45_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/generics.rs`:1600 on 2025-12-10 13:45_

That would certainly help with performance, but it's not correct (and several tests fail) :smile:. We need to keep iterating in case other union elements help us solve more typevars.

---

_@AlexWaygood reviewed on 2025-12-10 13:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:1600 on 2025-12-10 13:47_

all my reviews are from my iPad today so unlike usual I am not trying out any of my crazy suggestions locally before posting comments 😶

---

_Merged by @sharkdp on 2025-12-10 13:58_

---

_Closed by @sharkdp on 2025-12-10 13:58_

---

_Branch deleted on 2025-12-10 13:59_

---
