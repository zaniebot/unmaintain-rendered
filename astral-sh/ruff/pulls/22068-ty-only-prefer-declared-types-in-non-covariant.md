```yaml
number: 22068
title: "[ty] Only prefer declared types in non-covariant positions"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/prefer-non-covariant-declared-types
created_at: 2025-12-19T04:24:35Z
updated_at: 2025-12-19T23:02:28Z
url: https://github.com/astral-sh/ruff/pull/22068
synced_at: 2026-01-12T15:57:40Z
```

# [ty] Only prefer declared types in non-covariant positions

---

_@ibraheemdev_

## Summary

The following snippet currently errors because we widen the inferred type, even though `X` is covariant over `T`. If `T` was contravariant or invariant, this would be fine, as it would lead to an assignability error anyways.

```python
class X[T]:
    def __init__(self: X[None]): ...

    def pop(self) -> T:
        raise NotImplementedError

# error: Argument to bound method `__init__` is incorrect: Expected `X[None]`, found `X[int | None]`
x: X[int | None] = X()
```

There are some cases where it is still helpful to prefer covariant declared types, but this error seems hard to fix otherwise, and makes our heuristics more consistent overall.

---

_Review requested from @carljm by @ibraheemdev on 2025-12-19 04:24_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-12-19 04:24_

---

_Label `ty` added by @ibraheemdev on 2025-12-19 04:24_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-12-19 04:24_

---

_Review requested from @dcreager by @ibraheemdev on 2025-12-19 04:24_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 04:26_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 04:27_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:2564:50: error[invalid-assignment] Object of type `Future[T_Retval@run_async_from_thread] | Future[_T@run_coroutine_threadsafe]` is not assignable to `Future[T_Retval@run_async_from_thread]`
+ src/anyio/_backends/_asyncio.py:2564:50: error[invalid-assignment] Object of type `Future[_T@run_coroutine_threadsafe] | Future[T_Retval@run_async_from_thread]` is not assignable to `Future[T_Retval@run_async_from_thread]`

beartype (https://github.com/beartype/beartype)
+ beartype/claw/_ast/_kind/clawastimport.py:622:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/claw/_ast/_kind/clawastimport.py:631:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 491 diagnostics
+ Found 493 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_pipeline_files.py:370:28: error[invalid-assignment] Object of type `dataclasses.Field[list[str] | <class 'list'>]` is not assignable to `list[str]`
+ tests/test_pipeline_files.py:370:28: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[str]]` is not assignable to `list[str]`
- tests/test_pipeline_files.py:371:35: error[invalid-assignment] Object of type `dataclasses.Field[list[dict[str, str]] | <class 'list'>]` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_files.py:371:35: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[dict[str, str]]]` is not assignable to `list[dict[str, str]]`
- tests/test_pipeline_files.py:373:35: error[invalid-assignment] Object of type `dataclasses.Field[list[str] | <class 'list'>]` is not assignable to `list[str]`
+ tests/test_pipeline_files.py:373:35: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[str]]` is not assignable to `list[str]`
- tests/test_pipeline_files.py:374:42: error[invalid-assignment] Object of type `dataclasses.Field[list[dict[str, str]] | <class 'list'>]` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_files.py:374:42: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[dict[str, str]]]` is not assignable to `list[dict[str, str]]`
- tests/test_pipeline_images.py:332:29: error[invalid-assignment] Object of type `dataclasses.Field[list[str] | <class 'list'>]` is not assignable to `list[str]`
+ tests/test_pipeline_images.py:332:29: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[str]]` is not assignable to `list[str]`
- tests/test_pipeline_images.py:333:36: error[invalid-assignment] Object of type `dataclasses.Field[list[dict[str, str]] | <class 'list'>]` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_images.py:333:36: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[dict[str, str]]]` is not assignable to `list[dict[str, str]]`
- tests/test_pipeline_images.py:335:36: error[invalid-assignment] Object of type `dataclasses.Field[list[str] | <class 'list'>]` is not assignable to `list[str]`
+ tests/test_pipeline_images.py:335:36: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[str]]` is not assignable to `list[str]`
- tests/test_pipeline_images.py:336:43: error[invalid-assignment] Object of type `dataclasses.Field[list[dict[str, str]] | <class 'list'>]` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_images.py:336:43: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[dict[str, str]]]` is not assignable to `list[dict[str, str]]`

mypy (https://github.com/python/mypy)
- mypyc/irbuild/util.py:35:44: error[invalid-assignment] Object of type `frozenset[str | Unknown]` is not assignable to `frozenset[Literal["native_class", "allow_interpreted_subclasses", "serializable", "free_list_len"]]`
+ mypyc/irbuild/util.py:35:44: error[invalid-assignment] Object of type `frozenset[Unknown | str]` is not assignable to `frozenset[Literal["native_class", "allow_interpreted_subclasses", "serializable", "free_list_len"]]`

psycopg (https://github.com/psycopg/psycopg)
- tests/typing_example.py:100:13: error[invalid-assignment] Object of type `Connection[int] | Connection[tuple[Any, ...]]` is not assignable to `Connection[int]`
+ tests/typing_example.py:100:13: error[invalid-assignment] Object of type `Connection[tuple[Any, ...]] | Connection[int]` is not assignable to `Connection[int]`
- tests/typing_example.py:110:13: error[invalid-assignment] Object of type `Connection[Person] | Connection[tuple[Any, ...]]` is not assignable to `Connection[Person]`
+ tests/typing_example.py:110:13: error[invalid-assignment] Object of type `Connection[tuple[Any, ...]] | Connection[Person]` is not assignable to `Connection[Person]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/optimize/analysis/lookahead.py:48:13: error[invalid-argument-type] Argument to bound method `backtest` is incorrect: Expected `dict[Unknown, Unknown]`, found `dict[Unknown, Unknown] | DataFrame`
+ freqtrade/optimize/analysis/lookahead.py:48:13: error[invalid-argument-type] Argument to bound method `backtest` is incorrect: Expected `dict[Unknown, Unknown]`, found `DataFrame | dict[Unknown, Unknown]`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/context/menu.py:57:96: error[invalid-assignment] Object of type `frozenset[CommandType | Unknown]` is not assignable to `frozenset[Literal[CommandType.USER, CommandType.MESSAGE]]`
+ tanjun/context/menu.py:57:96: error[invalid-assignment] Object of type `frozenset[Unknown | CommandType]` is not assignable to `frozenset[Literal[CommandType.USER, CommandType.MESSAGE]]`

Expression (https://github.com/cognitedata/Expression)
- expression/collections/array.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `TypedArray[_TSource@TypedArray]`, found `TypedArray[_TSource@TypedArray] | Unknown | _TState@unfold`
+ expression/collections/array.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `TypedArray[_TSource@TypedArray]`, found `Unknown | _TState@unfold | TypedArray[_TSource@TypedArray]`
- expression/collections/block.py:486:16: error[invalid-return-type] Return type does not match returned value: expected `Block[_TSource@Block]`, found `Block[_TSource@Block] | Unknown | _TState@unfold`
+ expression/collections/block.py:486:16: error[invalid-return-type] Return type does not match returned value: expected `Block[_TSource@Block]`, found `Unknown | _TState@unfold | Block[_TSource@Block]`
- expression/collections/block.py:595:12: error[invalid-return-type] Return type does not match returned value: expected `Block[_TSource@concat]`, found `Block[_TSource@concat] | Unknown | Iterable[Block[_TSource@concat]]`
+ expression/collections/block.py:595:12: error[invalid-return-type] Return type does not match returned value: expected `Block[_TSource@concat]`, found `Unknown | Iterable[Block[_TSource@concat]] | Block[_TSource@concat]`
- expression/core/result.py:121:24: error[invalid-return-type] Return type does not match returned value: expected `Result[_TResult@map2, _TErrorOut@Result]`, found `Result[_TResult@map2 | Unknown | _TOther@map2, _TErrorOut@Result]`
+ expression/core/result.py:121:24: error[invalid-return-type] Return type does not match returned value: expected `Result[_TResult@map2, _TErrorOut@Result]`, found `Result[Unknown | _TOther@map2 | _TResult@map2, _TErrorOut@Result]`
- expression/effect/result.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Result[_TResult@bind, _TError@ResultBuilder]`, found `Result[_TResult@bind, _TError@ResultBuilder] | Unknown | Result[_TSource@ResultBuilder, _TError@ResultBuilder]`
+ expression/effect/result.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Result[_TResult@bind, _TError@ResultBuilder]`, found `Unknown | Result[_TSource@ResultBuilder, _TError@ResultBuilder] | Result[_TResult@bind, _TError@ResultBuilder]`
- tests/test_array.py:47:27: error[invalid-assignment] Object of type `TypedArray[str] | Unknown | TypedArray[Unknown | int]` is not assignable to `TypedArray[str]`
+ tests/test_array.py:47:27: error[invalid-assignment] Object of type `Unknown | TypedArray[Unknown | int] | TypedArray[str]` is not assignable to `TypedArray[str]`
- tests/test_array.py:58:35: error[invalid-assignment] Object of type `TypedArray[uint8] | Unknown | TypedArray[Unknown | str]` is not assignable to `TypedArray[uint8]`
+ tests/test_array.py:58:35: error[invalid-assignment] Object of type `Unknown | TypedArray[Unknown | str] | TypedArray[uint8]` is not assignable to `TypedArray[uint8]`
- tests/test_array.py:69:36: error[invalid-assignment] Object of type `TypedArray[uint16] | Unknown | TypedArray[Unknown | str]` is not assignable to `TypedArray[uint16]`
+ tests/test_array.py:69:36: error[invalid-assignment] Object of type `Unknown | TypedArray[Unknown | str] | TypedArray[uint16]` is not assignable to `TypedArray[uint16]`
- tests/test_array.py:80:36: error[invalid-assignment] Object of type `TypedArray[uint32] | Unknown | TypedArray[Unknown | str]` is not assignable to `TypedArray[uint32]`
+ tests/test_array.py:80:36: error[invalid-assignment] Object of type `Unknown | TypedArray[Unknown | str] | TypedArray[uint32]` is not assignable to `TypedArray[uint32]`
- tests/test_array.py:91:37: error[invalid-assignment] Object of type `TypedArray[float32] | Unknown | TypedArray[Unknown | str]` is not assignable to `TypedArray[float32]`
+ tests/test_array.py:91:37: error[invalid-assignment] Object of type `Unknown | TypedArray[Unknown | str] | TypedArray[float32]` is not assignable to `TypedArray[float32]`
- tests/test_array.py:99:35: error[invalid-assignment] Object of type `TypedArray[int16] | Unknown | TypedArray[Unknown | int]` is not assignable to `TypedArray[int16]`
+ tests/test_array.py:99:35: error[invalid-assignment] Object of type `Unknown | TypedArray[Unknown | int] | TypedArray[int16]` is not assignable to `TypedArray[int16]`
+ tests/test_result.py:491:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Literal[42], Any]`, found `Result[Literal[0, 42], Any]`
+ tests/test_result.py:509:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Any, Literal["original error"]]`, found `Result[Any, Literal["new error", "original error"]]`
- Found 226 diagnostics
+ Found 228 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/interactions.py:386:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionResponse[ClientT@Interaction]`, found `InteractionResponse[Client]`
- Found 557 diagnostics
+ Found 558 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/synchronous/database.py:938:20: error[no-matching-overload] No overload of bound method `_command` matches arguments
- Found 446 diagnostics
+ Found 445 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- tests/mypy/pandas_modules/pandas_dataframe.py:35:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[SchemaOut] | DataFrame[AnotherSchema]`
+ tests/mypy/pandas_modules/pandas_dataframe.py:35:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[AnotherSchema] | DataFrame[SchemaOut]`
- tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[SchemaOut] | DataFrame[Schema]`
+ tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[Schema] | DataFrame[SchemaOut]`

cloud-init (https://github.com/canonical/cloud-init)
+ tests/unittests/sources/test_lxd.py:40:5: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
+ tests/unittests/test_net.py:5345:41: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Unknown | list[Unknown | str] | dict[Unknown | str, Unknown | str | None] | dict[Unknown, Unknown]`
+ tests/unittests/test_net.py:5502:41: warning[possibly-missing-attribute] Attribute `keys` may be missing on object of type `Unknown | list[Unknown | str] | dict[Unknown | str, Unknown | str | None] | dict[Unknown | str, Unknown | str]`
- Found 1181 diagnostics
+ Found 1184 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/upstream/mybooks.py:477:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `SupportsKeysAndGetItem[Literal["want-to-read", "already-read", "currently-reading"], Literal["Want to Read", "Already Read", "Currently Reading"]]`, found `dict[Unknown | str, Unknown | str]`
- Found 1153 diagnostics
+ Found 1152 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/dataset.py:912:47: error[invalid-argument-type] Argument to bound method `_construct_direct` is incorrect: Expected `dict[Unknown, Unknown] | None`, found `dict[Hashable, Any] | None | Default`
- xarray/core/dataset.py:912:63: error[invalid-argument-type] Argument to bound method `_construct_direct` is incorrect: Expected `dict[Unknown, Unknown] | None`, found `dict[Unknown, Unknown] | None | Default`
+ xarray/core/dataset.py:2565:52: error[invalid-argument-type] Argument to function `either_dict_or_kwargs` is incorrect: Expected `Mapping[Any, Resampler | str | int | tuple[int, ...] | None] | None`, found `(str & Top[Mapping[Unknown, object]]) | (int & Top[Mapping[Unknown, object]]) | (tuple[int, ...] & Top[Mapping[Unknown, object]]) | ... omitted 3 union elements`
- Found 1770 diagnostics
+ Found 1769 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-databricks/prefect_databricks/models/jobs.py:3849:38: error[invalid-assignment] Object of type `dataclasses.Field[dict[str, Any] | None | Literal["{}"]]` is not assignable to `dict[str, Any] | None`
+ src/integrations/prefect-databricks/prefect_databricks/models/jobs.py:3849:38: error[invalid-assignment] Object of type `dataclasses.Field[Literal["{}"] | dict[str, Any] | None]` is not assignable to `dict[str, Any] | None`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_block_document_references` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `dict[str, Any] | T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/_states.py:219:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[State[Any] | R@return_value_to_state_sync]`
+ src/prefect/_states.py:219:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[R@return_value_to_state_sync | State[Any]]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/states.py:369:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[State[Any] | R@return_value_to_state]`
+ src/prefect/states.py:369:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[R@return_value_to_state | State[Any]]`
- src/prefect/tasks.py:1200:16: error[no-matching-overload] No overload of function `run_task` matches arguments
+ src/prefect/tasks.py:1200:16: error[invalid-return-type] Return type does not match returned value: expected `T@__call__ | State[T@__call__] | None`, found `State[T@__call__] | T@__call__ | State[Never]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
- src/prefect/variables.py:90:16: error[invalid-return-type] Return type does not match returned value: expected `Variable[str | int | float | ... omitted 3 union elements]`, found `Variable[str | int | float | ... omitted 3 union elements] | Self@aset`
+ src/prefect/variables.py:90:16: error[invalid-return-type] Return type does not match returned value: expected `Variable[str | int | float | ... omitted 3 union elements]`, found `Self@aset | Variable[str | int | float | ... omitted 3 union elements]`
- src/prefect/variables.py:139:20: error[invalid-return-type] Return type does not match returned value: expected `Variable[str | int | float | ... omitted 3 union elements]`, found `Variable[str | int | float | ... omitted 3 union elements] | Self@set`
+ src/prefect/variables.py:139:20: error[invalid-return-type] Return type does not match returned value: expected `Variable[str | int | float | ... omitted 3 union elements]`, found `Self@set | Variable[str | int | float | ... omitted 3 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/typing_extensions.py:3546:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[str]`, found `object`
+ setuptools/_vendor/typing_extensions.py:3546:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `object`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- tests/testing/mocks.py:668:25: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[dict[str, Any]]`, found `Iterable[dict[str, Any]]`
+ tests/testing/mocks.py:668:25: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Iterable[dict[str, Any]]`

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/algorithms.py:546:33: error[invalid-argument-type] Argument to bound method `isin` is incorrect: Expected `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]]`, found `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | ... omitted 3 union elements`
+ pandas/core/algorithms.py:546:33: error[invalid-argument-type] Argument to bound method `isin` is incorrect: Expected `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]]`, found `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | (Index & ~MultiIndex) | (Series & ~MultiIndex) | (SequenceNotStr[Unknown] & ndarray[tuple[object, ...], dtype[object]] & ~MultiIndex)`
- pandas/core/algorithms.py:551:30: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | ... omitted 3 union elements`
- pandas/core/algorithms.py:555:30: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | ... omitted 3 union elements`
- pandas/core/algorithms.py:556:34: warning[possibly-missing-attribute] Attribute `astype` may be missing on object of type `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | ... omitted 3 union elements`
- pandas/core/algorithms.py:558:21: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | ... omitted 3 union elements`
- pandas/core/algorithms.py:586:38: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | ... omitted 3 union elements`
- pandas/core/algorithms.py:587:18: warning[possibly-missing-attribute] Attribute `astype` may be missing on object of type `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | ... omitted 3 union elements`
+ pandas/core/algorithms.py:556:34: error[no-matching-overload] No overload of bound method `astype` matches arguments
+ pandas/core/algorithms.py:556:34: error[no-matching-overload] No overload of bound method `astype` matches arguments
+ pandas/core/algorithms.py:556:34: error[no-matching-overload] No overload of bound method `astype` matches arguments
+ pandas/core/algorithms.py:587:18: error[no-matching-overload] No overload of bound method `astype` matches arguments
+ pandas/core/algorithms.py:587:18: error[no-matching-overload] No overload of bound method `astype` matches arguments
+ pandas/core/algorithms.py:587:18: error[no-matching-overload] No overload of bound method `astype` matches arguments
- pandas/core/algorithms.py:591:27: error[invalid-argument-type] Argument to function `ismember` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[Any]]`, found `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index | ... omitted 5 union elements`
+ pandas/core/algorithms.py:591:27: error[invalid-argument-type] Argument to function `ismember` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[Any]]`, found `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | (Index & ~MultiIndex) | ... omitted 4 union elements`
+ pandas/core/arrays/datetimelike.py:2585:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas/core/arrays/period.py:1469:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[tuple[Any, ...], dtype[Any]], BaseOffset]`, found `tuple[ndarray[tuple[Any, ...], dtype[Any]], BaseOffset | Unknown | None]`
+ pandas/core/arrays/period.py:1469:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[tuple[Any, ...], dtype[Any]], BaseOffset]`, found `tuple[ndarray[tuple[Any, ...], dtype[Unknown]], BaseOffset | Unknown | None]`
- pandas/core/sorting.py:428:16: error[invalid-return-type] Return type does not match returned value: expected `ndarray[tuple[Any, ...], dtype[signedinteger[_64Bit]]]`, found `ndarray[tuple[Any, ...], dtype[signedinteger[_64Bit]]] | Series | ndarray[tuple[Any, ...], dtype[Any]]`
+ pandas/core/sorting.py:428:16: error[invalid-return-type] Return type does not match returned value: expected `ndarray[tuple[Any, ...], dtype[signedinteger[_64Bit]]]`, found `ndarray[tuple[Any, ...], dtype[Any]] | ndarray[tuple[Any, ...], dtype[signedinteger[_64Bit]]] | Series`
- Found 3736 diagnostics
+ Found 3737 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 7 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[ndarray[Any, Any]]`, found `InterfaceString[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[ndarray[Any, Any]]`, found `InterfaceString[ndarray[object, object] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:781:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[ndarray[Any, Any]]`, found `InterfaceDatetime[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index.py:781:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[ndarray[Any, Any]]`, found `InterfaceDatetime[ndarray[object, object] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:801:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[ndarray[Any, Any]]`, found `InterfaceRe[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index.py:801:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[ndarray[Any, Any]]`, found `InterfaceRe[ndarray[object, object] | Bottom[Series[Any, Any]]]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Top[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[SeriesHE[Any, Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | TypeBlocks | Batch | ... omitted 7 union elements, generic[object]]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Top[Index[Any]] | TypeBlocks | ... omitted 7 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
- static_frame/test/unit/test_series.py:4489:37: error[no-matching-overload] No overload of function `round` matches arguments
- Found 1839 diagnostics
+ Found 1840 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/indexes/test_indexes.py:533:9: error[type-assertion-failure] Type `IntervalIndex[Interval[int]]` does not match asserted type `Unknown`
- tests/indexes/test_indexes.py:541:9: error[type-assertion-failure] Type `IntervalIndex[Interval[int | float]]` does not match asserted type `IntervalIndex[Interval[int]]`
+ tests/indexes/test_indexes.py:541:9: error[type-assertion-failure] Type `IntervalIndex[Interval[int | float]]` does not match asserted type `Unknown`
+ tests/indexes/test_indexes.py:644:9: error[type-assertion-failure] Type `IntervalIndex[Interval[int]]` does not match asserted type `Unknown`
- tests/indexes/test_indexes.py:655:9: error[type-assertion-failure] Type `IntervalIndex[Interval[int | float]]` does not match asserted type `IntervalIndex[Interval[int]]`
+ tests/indexes/test_indexes.py:655:9: error[type-assertion-failure] Type `IntervalIndex[Interval[int | float]]` does not match asserted type `Unknown`
- tests/test_timefuncs.py:1464:9: error[type-assertion-failure] Type `DatetimeIndex` does not match asserted type `Unknown`
- Found 5085 diagnostics
+ Found 5086 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/components/tado/climate.py:813:20: error[unsupported-operator] Operator `in` is not supported between objects of type `str | None` and `@Todo | str | list[Unknown]`
- homeassistant/core.py:1668:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `(Event[_DataT@async_listen_once], /) -> Coroutine[Any, Any, None] | None`
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14415 diagnostics
+ Found 14412 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-19 04:29_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 04:35_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 4 | 1 | 22 |
| `invalid-assignment` | 0 | 0 | 23 |
| `invalid-argument-type` | 6 | 4 | 10 |
| `no-matching-overload` | 6 | 3 | 0 |
| `possibly-missing-attribute` | 2 | 6 | 0 |
| `type-assertion-failure` | 2 | 1 | 2 |
| `unused-ignore-comment` | 3 | 0 | 0 |
| `non-subscriptable` | 1 | 0 | 0 |
| `unsupported-operator` | 0 | 1 | 0 |
| **Total** | **24** | **16** | **57** |

**[Full report with detailed diff](https://ibraheem-prefer-non-covarian.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-prefer-non-covarian.ecosystem-663.pages.dev/timing))




---

_@AlexWaygood approved on 2025-12-19 14:04_

Makes sense to me!

---

_Comment by @ibraheemdev on 2025-12-19 22:27_

This seems to have a mixed ecosystem impact, which is somewhat unavoidable given any heuristic. Given that the impact is relatively small, and this becomes more important after https://github.com/astral-sh/ruff/pull/21930 (where we are able to infer a lot more type context from covariant types), I'm going to go ahead and merge this. Some of the regressions may also be improved by the new constraint solver.

---

_Merged by @ibraheemdev on 2025-12-19 22:27_

---

_Closed by @ibraheemdev on 2025-12-19 22:27_

---

_Branch deleted on 2025-12-19 22:27_

---

_Comment by @AlexWaygood on 2025-12-19 23:02_

That was my conclusion from skimming through the ecosystem diff too, yeah

---
