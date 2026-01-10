```yaml
number: 21651
title: "[ty] more precise inference for a failed specialization"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/failedspec
created_at: 2025-11-27T00:57:32Z
updated_at: 2025-11-27T12:44:31Z
url: https://github.com/astral-sh/ruff/pull/21651
synced_at: 2026-01-10T16:48:02Z
```

# [ty] more precise inference for a failed specialization

---

_Pull request opened by @carljm on 2025-11-27 00:57_

## Summary

Previously if an explicit specialization failed (e.g. wrong number of type arguments or violates an upper bound) we just inferred `Unknown` for the entire type. This actually caused us to panic on an a case of a recursive upper bound with invalid specialization; the upper bound would oscillate indefinitely in fixpoint iteration between `Unknown` and the given specialization. This could be fixed with a cycle recovery function, but in this case there's a simpler fix: if we infer `C[Unknown]` instead of `Unknown` for an invalid attempt to specialize `C`, that allows fixpoint iteration to quickly converge, as well as giving a more precise type inference.

Other type checkers actually just go with the attempted specialization even if it's invalid. So if `C` has a type parameter with upper bound `int`, and you say `C[str]`, they'll emit a diagnostic but just go with `C[str]`. Even weirder, if `C` has a single type parameter and you say `C[str, bytes]`, they'll just go with `C[str]` as the type. I'm not convinced by this approach; it seems odd to have specializations floating around that explicitly violate the declared upper bound, or in the latter case aren't even the specialization the annotation requested. I prefer `C[Unknown]` for this case.

Fixing this revealed an issue with `collections.namedtuple`, which returns `type[tuple[Any, ...]]`. Due to https://github.com/astral-sh/ty/issues/1649 we consider that to be an invalid specialization. So previously we returned `Unknown`; after this PR it would be `type[tuple[Unknown]]`, leading to more false positives from our lack of functional namedtuple support. To avoid that I added an explicit Todo type for functional namedtuples for now.

## Test Plan

Added and updated mdtests.

The conformance suite changes have to do with `ParamSpec`, so no meaningful signal there.

The ecosystem changes appear to be the expected effects of having more precise type information (including occurrences of known issues such as https://github.com/astral-sh/ty/issues/1495 ). Most effects are just changes to types in diagnostics.

---

_Label `ty` added by @carljm on 2025-11-27 00:57_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 00:59_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-27 12:32:15.155059641 +0000
+++ new-output.txt	2025-11-27 12:32:18.475105279 +0000
@@ -451,11 +451,9 @@
 generics_defaults.py:55:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
 generics_defaults.py:59:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
 generics_defaults.py:63:1: error[type-assertion-failure] Type `type[AllTheDefaults[int, int | float | complex, str, int, bool]]` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:79:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `<class 'Class_ParamSpec'>`
+generics_defaults.py:79:1: error[type-assertion-failure] Type `type[Class_ParamSpec[Unknown]]` does not match asserted type `<class 'Class_ParamSpec'>`
 generics_defaults.py:79:56: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
-generics_defaults.py:80:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Class_ParamSpec[Unknown]`
 generics_defaults.py:80:53: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
-generics_defaults.py:81:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Class_ParamSpec[Unknown]`
 generics_defaults.py:81:29: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[bool, bool]`?
 generics_defaults.py:81:68: error[invalid-type-arguments] Too many type arguments to class `Class_ParamSpec`: expected between 0 and 1, got 2
 generics_defaults.py:91:26: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
@@ -1031,4 +1029,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1033 diagnostics
+Found 1031 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-27 01:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
+ src/async_utils/corofunc_cache.py:50:58: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `CoroFunc[Unknown]`
+ src/async_utils/task_cache.py:53:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `TaskFunc[Unknown]`
- Found 27 diagnostics
+ Found 29 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/typing/pandas.py:381:45: error[invalid-argument-type] Argument to bound method `from_records` is incorrect: Expected `ndarray[tuple[int, int], Unknown] | Sequence[SequenceNotStr[Unknown]] | Sequence[Mapping[str, Any]] | Mapping[str, Any] | Mapping[str, SequenceNotStr[Any]]`, found `ndarray[tuple[Any, ...], dtype[Any]] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`
+ pandera/typing/pandas.py:381:45: error[invalid-argument-type] Argument to bound method `from_records` is incorrect: Expected `ndarray[tuple[int, int], dtype[Unknown]] | Sequence[SequenceNotStr[Unknown]] | Sequence[Mapping[str, Any]] | Mapping[str, Any] | Mapping[str, SequenceNotStr[Any]]`, found `ndarray[tuple[Any, ...], dtype[Any]] | list[tuple[Any, ...]] | dict[Any, Any] | DataFrame`

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_injection.py:298:9: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `(((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]]) | (Unknown & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]])`
- src/antidote/core/_injection.py:298:30: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `(((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]]) | (Unknown & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]])`
- src/antidote/core/_injection.py:302:17: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `(((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType) | (Unknown & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType)`
- src/antidote/core/_injection.py:302:42: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `(((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType) | (Unknown & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType)`
+ src/antidote/core/_injection.py:298:9: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]]` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:298:30: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]]` has no attribute `__name__`
+ src/antidote/core/_injection.py:302:17: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:302:42: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[object, object] & ~Top[classmethod[Unknown, object, object]] & ~MethodType` has no attribute `__name__`
- tests/core/test_inject.py:143:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/core/test_inject.py:206:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/core/test_inject.py:368:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/core/test_inject_through_annotations.py:41:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/core/test_inject_through_annotations.py:50:14: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/core/test_inject_through_annotations.py:144:14: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/core/test_inject_through_annotations.py:151:15: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_lazy.py:99:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_lazy.py:102:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_lazy.py:137:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_lazy.py:200:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_lazy.py:245:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_lazy.py:248:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_lazy.py:295:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/lazy/test_lazy.py:278:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/lazy/test_lazy.py:291:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 310 diagnostics
+ Found 294 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/optimize/optimize_reports/optimize_reports.py:288:90: error[invalid-argument-type] Argument to bound method `from_records` is incorrect: Expected `ndarray[tuple[int, int], Unknown] | Sequence[SequenceNotStr[Unknown]] | Sequence[Mapping[str, Any]] | Mapping[str, Any] | Mapping[str, SequenceNotStr[Any]]`, found `list[Unknown] | (DataFrame & Top[list[Unknown]])`
+ freqtrade/optimize/optimize_reports/optimize_reports.py:288:90: error[invalid-argument-type] Argument to bound method `from_records` is incorrect: Expected `ndarray[tuple[int, int], dtype[Unknown]] | Sequence[SequenceNotStr[Unknown]] | Sequence[Mapping[str, Any]] | Mapping[str, Any] | Mapping[str, SequenceNotStr[Any]]`, found `list[Unknown] | (DataFrame & Top[list[Unknown]])`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/auth.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `Unknown | None | bytes`
+ pymongo/asynchronous/auth.py:131:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `@Todo | None | bytes`
- pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: Unknown, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: Unknown, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
+ pymongo/asynchronous/auth.py:365:69: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: AsyncConnection) -> CoroutineType[Any, Any, None]) | ((credentials: @Todo, conn: AsyncConnection, reauthenticate: bool) -> CoroutineType[Any, Any, Mapping[str, Any] | None]) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> Coroutine[Any, Any, None]]`
- pymongo/synchronous/auth.py:128:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `Unknown | None | bytes`
+ pymongo/synchronous/auth.py:128:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes | bytearray`, found `@Todo | None | bytes`
- pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: Unknown, conn: Connection) -> None) | ((credentials: Unknown, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> None]`
+ pymongo/synchronous/auth.py:360:48: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((credentials: @Todo, conn: Connection) -> None) | ((credentials: @Todo, conn: Connection, reauthenticate: bool) -> Mapping[str, Any] | None) | partial[Unknown]]` is not assignable to `Mapping[str, (...) -> None]`

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/hybrid.py:463:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 551 diagnostics
+ Found 550 diagnostics

vision (https://github.com/pytorch/vision)
- references/depth/stereo/train.py:508:17: error[unresolved-attribute] Object of type `ndarray[tuple[Any, ...], Unknown]` has no attribute `load_state_dict`
+ references/depth/stereo/train.py:508:17: error[unresolved-attribute] Object of type `ndarray[tuple[Any, ...], dtype[Unknown]]` has no attribute `load_state_dict`

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_highlevel_ssl_helpers.py:99:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 531 diagnostics
+ Found 530 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/datasets/_samples_generator.py:1144:60: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | (float & ~Real)`
+ sklearn/datasets/_samples_generator.py:1144:60: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `@Todo | (float & ~Real)`
+ sklearn/linear_model/_stochastic_gradient.py:777:30: error[no-matching-overload] No overload of bound method `reshape` matches arguments
+ sklearn/manifold/_spectral_embedding.py:406:9: error[unsupported-operator] Operator `-=` is unsupported between objects of type `csr_matrix[Any]` and `dia_matrix[float64]`
+ sklearn/utils/tests/test_extmath.py:1084:20: error[unresolved-attribute] Object of type `spmatrix[complexfloating[Any, Any]]` has no attribute `toarray`
- Found 2321 diagnostics
+ Found 2324 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- arviz/tests/base_tests/test_stats_utils.py:188:16: warning[possibly-missing-attribute] Attribute `all` may be missing on object of type `bool | Unknown`
+ arviz/tests/base_tests/test_stats_utils.py:188:16: warning[possibly-missing-attribute] Attribute `all` may be missing on object of type `bool | @Todo`

altair (https://github.com/vega/altair)
+ altair/datasets/_reader.py:335:16: error[invalid-argument-type] Argument to bound method `lazy` is incorrect: Expected `LazyFrame[Unknown]`, found `IntoFrameT@Reader`
- Found 1025 diagnostics
+ Found 1026 diagnostics

colour (https://github.com/colour-science/colour)
- colour/hints/__init__.py:649:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `ndarray[tuple[Any, ...], Unknown]`
+ colour/hints/__init__.py:649:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `ndarray[tuple[Any, ...], dtype[Unknown]]`
- colour/hints/__init__.py:652:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `ndarray[tuple[Any, ...], Unknown]`
+ colour/hints/__init__.py:652:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `ndarray[tuple[Any, ...], dtype[Unknown]]`
- colour/io/fichet2021.py:706:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `ndarray[tuple[Any, ...], Unknown]`
+ colour/io/fichet2021.py:706:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `ndarray[tuple[Any, ...], dtype[Unknown]]`
- colour/utilities/tests/test_array.py:826:54: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `DataclassInstance | type[@Todo]`, found `ndarray[tuple[Any, ...], Unknown]`
+ colour/utilities/tests/test_array.py:826:54: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `DataclassInstance | type[@Todo]`, found `ndarray[tuple[Any, ...], dtype[Unknown]]`
+ colour/utilities/tests/test_array.py:828:28: error[invalid-return-type] Return type does not match returned value: expected `numpy.bool[builtins.bool] | signedinteger[_8Bit] | signedinteger[_16Bit] | ... omitted 11 union elements`, found `dtype[Unknown]`
- Found 385 diagnostics
+ Found 386 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- tests/assert_type/db/models/test_ordering.py:38:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/assert_type/db/models/test_ordering.py:39:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/assert_type/db/models/test_ordering.py:40:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/assert_type/db/models/test_ordering.py:42:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/assert_type/db/models/test_ordering.py:43:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 444 diagnostics
+ Found 439 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/backends/bigquery/tests/system/udf/test_udf_execute.py:179:46: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Array[Unknown]`
- ibis/backends/sqlite/__init__.py:332:40: error[invalid-argument-type] Argument to bound method `from_records` is incorrect: Expected `ndarray[tuple[int, int], Unknown] | Sequence[SequenceNotStr[Unknown]] | Sequence[Mapping[str, Any]] | Mapping[str, Any] | Mapping[str, SequenceNotStr[Any]]`, found `Cursor`
+ ibis/backends/sqlite/__init__.py:332:40: error[invalid-argument-type] Argument to bound method `from_records` is incorrect: Expected `ndarray[tuple[int, int], dtype[Unknown]] | Sequence[SequenceNotStr[Unknown]] | Sequence[Mapping[str, Any]] | Mapping[str, Any] | Mapping[str, SequenceNotStr[Any]]`, found `Cursor`
+ ibis/expr/operations/generic.py:172:26: error[unknown-argument] Argument `dtype` does not match any known parameter
+ ibis/expr/operations/generic.py:172:39: error[unknown-argument] Argument `counter` does not match any known parameter
+ ibis/expr/types/generic.py:2006:13: error[invalid-argument-type] Argument is incorrect: Expected `Value[Unknown, Unknown]`, found `Value | None | @Todo`
+ ibis/expr/types/generic.py:2045:13: error[invalid-argument-type] Argument is incorrect: Expected `Value[Unknown, Unknown]`, found `Value | None | @Todo`
- Found 4414 diagnostics
+ Found 4419 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/util.py:1771:12: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: @Todo | tuple[@Todo, ...], /) -> ndarray[tuple[Any, ...], list[tuple[str, Any] | Unknown]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 3 union elements, /) -> ndarray[tuple[Any, ...], list[tuple[str, Any] | Unknown]], (key: str, /) -> ndarray[Any, dtype[Any]], (key: list[str], /) -> ndarray[Any, Unknown]]` cannot be called with key of type `tuple[slice[Any, Any, Any], Literal[0]]` on object of type `ndarray[Any, list[tuple[str, Any] | Unknown]]`
+ static_frame/core/util.py:1771:12: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: @Todo | tuple[@Todo, ...], /) -> ndarray[tuple[Any, ...], list[tuple[str, Any] | Unknown]], (key: SupportsIndex | tuple[SupportsIndex, ...], /) -> Any, (key: SupportsIndex | slice[Any, Any, Any] | EllipsisType | ... omitted 3 union elements, /) -> ndarray[tuple[Any, ...], list[tuple[str, Any] | Unknown]], (key: str, /) -> ndarray[Any, dtype[Any]], (key: list[str], /) -> ndarray[Any, @Todo]]` cannot be called with key of type `tuple[slice[Any, Any, Any], Literal[0]]` on object of type `ndarray[Any, list[tuple[str, Any] | Unknown]]`
- static_frame/test/unit/test_type_blocks.py:1401:17: error[invalid-argument-type] Argument to bound method `_assign_from_boolean_blocks_by_unit` is incorrect: Expected `ndarray[Any, Any] | None`, found `tuple[Unknown, Unknown]`
+ static_frame/test/unit/test_type_blocks.py:1401:17: error[invalid-argument-type] Argument to bound method `_assign_from_boolean_blocks_by_unit` is incorrect: Expected `ndarray[Any, Any] | None`, found `tuple[@Todo, @Todo]`
- static_frame/test/unit/test_type_blocks.py:1411:21: error[invalid-argument-type] Argument to bound method `_assign_from_boolean_blocks_by_unit` is incorrect: Expected `ndarray[Any, Any] | None`, found `tuple[Unknown, Unknown]`
+ static_frame/test/unit/test_type_blocks.py:1411:21: error[invalid-argument-type] Argument to bound method `_assign_from_boolean_blocks_by_unit` is incorrect: Expected `ndarray[Any, Any] | None`, found `tuple[@Todo, @Todo]`
+ static_frame/test/unit/test_type_clinic.py:379:35: error[invalid-type-arguments] Type `integer[Unknown]` is not assignable to upper bound `NBitBase` of type variable `_NBit@signedinteger`
+ static_frame/test/unit/test_type_clinic.py:385:35: error[invalid-type-arguments] Type `integer[Unknown]` is not assignable to upper bound `NBitBase` of type variable `_NBit@signedinteger`
+ static_frame/test/unit/test_type_clinic.py:406:50: error[invalid-type-arguments] Type `inexact[Unknown, Unknown]` is not assignable to upper bound `NBitBase` of type variable `_NBit1@floating`
+ static_frame/test/unit/test_type_clinic.py:409:50: error[invalid-type-arguments] Type `inexact[Unknown, Unknown]` is not assignable to upper bound `NBitBase` of type variable `_NBit1@floating`
- Found 1948 diagnostics
+ Found 1952 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/core/arrays/datetimelike.pyi:80:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas-stubs/core/indexes/datetimes.pyi:79:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas-stubs/core/indexes/datetimes.pyi:82:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/indexes/arithmetic/timedeltaindex/test_floordiv.py:115:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `Unknown`
+ tests/indexes/arithmetic/timedeltaindex/test_floordiv.py:115:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:251:11: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `@Todo`
+ tests/indexes/test_indexes.py:251:11: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[Unknown]]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:252:11: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `@Todo`
+ tests/indexes/test_indexes.py:252:11: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[Unknown]]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:253:11: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `@Todo`
+ tests/indexes/test_indexes.py:253:11: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[Unknown]]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:259:9: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `@Todo`
+ tests/indexes/test_indexes.py:259:9: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[Unknown]]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:259:9: error[type-assertion-failure] Type `Interval[Timestamp]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:259:9: error[type-assertion-failure] Type `Interval[Timestamp]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:267:9: error[type-assertion-failure] Type `Interval[Timedelta]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:267:9: error[type-assertion-failure] Type `Interval[Timedelta]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:280:9: error[type-assertion-failure] Type `Interval[Timestamp]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:280:9: error[type-assertion-failure] Type `Interval[Timestamp]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:288:9: error[type-assertion-failure] Type `Interval[Timedelta]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:288:9: error[type-assertion-failure] Type `Interval[Timedelta]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:534:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:534:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:535:11: error[type-assertion-failure] Type `PeriodIndex` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:535:11: error[type-assertion-failure] Type `PeriodIndex` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:536:11: error[type-assertion-failure] Type `DatetimeIndex` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:536:11: error[type-assertion-failure] Type `DatetimeIndex` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:542:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:542:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:586:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:586:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:641:11: error[type-assertion-failure] Type `int | float` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:641:11: error[type-assertion-failure] Type `int | float` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:768:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:768:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:769:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:769:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:771:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:771:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:772:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:772:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:789:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:789:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:790:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:790:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:797:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:797:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:798:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:798:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1039:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1039:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1042:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1042:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1045:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1045:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1046:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1046:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1048:11: error[type-assertion-failure] Type `DatetimeIndex` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1048:11: error[type-assertion-failure] Type `DatetimeIndex` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1052:9: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1052:9: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1072:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1072:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1075:9: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1075:9: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1134:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1134:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1135:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1135:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1137:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1137:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1138:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1138:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1156:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1156:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1157:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1157:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1164:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1164:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1165:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1165:17: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1536:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1536:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1565:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1565:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1566:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1566:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1567:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1567:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1568:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1568:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1581:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1581:11: error[type-assertion-failure] Type `Timestamp` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1747:11: error[type-assertion-failure] Type `Period` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1747:11: error[type-assertion-failure] Type `Period` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1771:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1771:17: error[type-assertion-failure] Type `Literal[False]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1772:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `Unknown`
+ tests/scalars/test_scalars.py:1772:17: error[type-assertion-failure] Type `Literal[True]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_sub.py:290:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Unknown`
+ tests/series/arithmetic/test_sub.py:290:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_sub.py:292:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Unknown`
+ tests/series/arithmetic/test_sub.py:292:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_add.py:63:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `Unknown`
+ tests/series/arithmetic/timedelta/test_add.py:63:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_add.py:64:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `Unknown`
+ tests/series/arithmetic/timedelta/test_add.py:64:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_floordiv.py:190:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `Unknown`
+ tests/series/arithmetic/timedelta/test_floordiv.py:190:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_sub.py:71:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `Unknown`
+ tests/series/arithmetic/timedelta/test_sub.py:71:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_sub.py:72:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `Unknown`
+ tests/series/arithmetic/timedelta/test_sub.py:72:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timestamp/test_add.py:77:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `Unknown`
+ tests/series/arithmetic/timestamp/test_add.py:77:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/series/arithmetic/timestamp/test_sub.py:67:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `Unknown`
+ tests/series/arithmetic/timestamp/test_sub.py:67:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/test_series.py:794:11: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `Unknown`
+ tests/series/test_series.py:794:11: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[Unknown]]` does not match asserted type `Unknown`
- tests/series/test_series.py:2021:11: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `Unknown`
+ tests/series/test_series.py:2021:11: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[Unknown]]` does not match asserted type `Unknown`
- tests/series/test_series.py:3879:9: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `Unknown`
+ tests/series/test_series.py:3879:9: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/test_frame.py:1102:11: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `Unknown`
+ tests/test_frame.py:1102:11: error[type-assertion-failure] Type `ndarray[tuple[int], dtype[Unknown]]` does not match asserted type `Unknown`
- tests/test_groupby.py:74:11: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:74:11: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:83:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:83:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:84:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:84:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:85:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:85:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:86:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:86:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:87:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:87:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:88:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:88:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:89:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:89:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:90:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:90:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:91:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:91:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:92:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:92:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:93:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:93:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:96:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:96:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:98:13: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:98:13: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:101:13: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:101:13: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:106:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:106:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:107:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:107:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:111:13: error[type-assertion-failure] Type `Series[int]` does not match asserted type `Unknown`
+ tests/test_groupby.py:111:13: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/test_groupby.py:113:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:113:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:116:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:116:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:117:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:117:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:118:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:118:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:137:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:137:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:140:19: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:140:19: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:141:19: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:141:19: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:143:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:143:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:149:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:149:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:155:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:155:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:162:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:162:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:180:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:180:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:233:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:233:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:255:19: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:255:19: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:257:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:257:17: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:286:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:286:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:292:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Unknown`
+ tests/test_groupby.py:292:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/test_groupby.py:298:15: error[type-assertion-failure] Type `int | float` does not match asserted type `Unknown`
+ tests/test_groupby.py:298:15: error[type-assertion-failure] Type `int | float` does not match asserted type `@Todo`
- tests/test_groupby.py:304:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:304:15: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:316:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:316:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:319:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:319:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:320:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:320:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:321:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:321:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:322:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:322:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:323:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:323:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:324:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:324:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:325:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:325:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:326:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:326:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:327:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:327:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:328:11: error[type-assertion-failure] Type `DataFrame` does not match asserted type `Unknown`
+ tests/test_groupby.py:328:11: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_groupby.py:329:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `Unknown`
+ tests/test_groupby.py:329:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/test_groupby.py:333:9: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:333:9: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:336:9: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:336:9: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:341:9: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:341:9: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:348:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:348:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:349:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:349:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:352:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `Unknown`
+ tests/test_groupby.py:352:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/test_groupby.py:353:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `Unknown`
+ tests/test_groupby.py:353:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/test_groupby.py:356:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:356:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:357:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:357:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:358:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `Unknown`
+ tests/test_groupby.py:358:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/test_groupby.py:371:13: error[type-assertion-failure] Type `DataFrame | Series[Any]` does not match asserted type `Unknown`
+ tests/test_groupby.py:371:13: error[type-assertion-failure] Type `DataFrame | Series[Any]` does not match asserted type `@Todo`
- tests/test_groupby.py:375:13: error[type-assertion-failure] Type `DataFrame | Series[Any]` does not match asserted type `Unknown`
+ tests/test_groupby.py:375:13: error[type-assertion-failure] Type `DataFrame | Series[Any]` does not match asserted type `@Todo`
- tests/test_groupby.py:379:13: error[type-assertion-failure] Type `DataFrame | Series[Any]` does not match asserted type `Unknown`
+ tests/test_groupby.py:379:13: error[type-assertion-failure] Type `DataFrame | Series[Any]` does not match asserted type `@Todo`
- tests/test_groupby.py:383:13: error[type-assertion-failure] Type `DataFrame | Series[Any]` does not match asserted type `Unknown`
+ tests/test_groupby.py:383:13: error[type-assertion-failure] Type `DataFrame | Series[Any]` does not match asserted type `@Todo`
- tests/test_groupby.py:390:13: error[type-as

... (truncated 202 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-27 01:03_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 01:11_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 1 | 8 | 170 |
| `invalid-argument-type` | 12 | 0 | 44 |
| `unused-ignore-comment` | 0 | 28 | 0 |
| `unresolved-attribute` | 11 | 0 | 1 |
| `invalid-return-type` | 7 | 0 | 2 |
| `possibly-missing-attribute` | 1 | 4 | 3 |
| `invalid-assignment` | 0 | 0 | 2 |
| `unknown-argument` | 2 | 0 | 0 |
| `invalid-type-form` | 0 | 0 | 1 |
| `no-matching-overload` | 1 | 0 | 0 |
| `unsupported-base` | 1 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **37** | **40** | **223** |

**[Full report with detailed diff](https://cjm-failedspec.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-failedspec.ecosystem-663.pages.dev/timing))




---

_Marked ready for review by @carljm on 2025-11-27 01:43_

---

_Review requested from @AlexWaygood by @carljm on 2025-11-27 01:43_

---

_Review requested from @sharkdp by @carljm on 2025-11-27 01:43_

---

_Review requested from @dcreager by @carljm on 2025-11-27 01:43_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 01:52_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@ibraheemdev approved on 2025-11-27 01:55_

This makes sense to me! `C[Unknown]` seems strictly better than `Unknown` in this case, and should lead to less false negatives.

---

_Comment by @dhruvmanila on 2025-11-27 07:09_

Sorry, I didn't see this PR before merging https://github.com/astral-sh/ruff/pull/21635. I can take ownership of changing this PR accordingly merging `main` here.

---

_Merged by @sharkdp on 2025-11-27 12:44_

---

_Closed by @sharkdp on 2025-11-27 12:44_

---

_Branch deleted on 2025-11-27 12:44_

---
