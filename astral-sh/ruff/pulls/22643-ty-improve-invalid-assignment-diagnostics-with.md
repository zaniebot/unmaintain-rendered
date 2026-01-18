```yaml
number: 22643
title: "[ty] Improve invalid assignment diagnostics with type context"
type: pull_request
state: open
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: ibraheem/bidi-diagnostics
created_at: 2026-01-17T03:51:36Z
updated_at: 2026-01-18T21:31:08Z
url: https://github.com/astral-sh/ruff/pull/22643
synced_at: 2026-01-18T22:21:55Z
```

# [ty] Improve invalid assignment diagnostics with type context

---

_@ibraheemdev_

## Summary

Resolves https://github.com/astral-sh/ty/issues/2537.


---

_Review requested from @carljm by @ibraheemdev on 2026-01-17 03:51_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2026-01-17 03:51_

---

_Review requested from @sharkdp by @ibraheemdev on 2026-01-17 03:51_

---

_Review requested from @dcreager by @ibraheemdev on 2026-01-17 03:51_

---

_Label `ty` added by @ibraheemdev on 2026-01-17 03:51_

---

_@ibraheemdev reviewed on 2026-01-17 03:52_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:408 on 2026-01-17 03:52_

I'm not exactly sure why this diagnostic didn't improve, there seems to be something else going on here with the combination of `__new__` and `__init__`

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2026-01-17 03:54_

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 03:54_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-17 03:55_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_base.py:200:9: error[invalid-assignment] Object of type `ReferenceType[BidictBase[VT@BidictBase, KT@BidictBase] | Self@inverse]` is not assignable to attribute `_invweak` of type `ReferenceType[BidictBase[VT@BidictBase, KT@BidictBase]] | None`
+ bidict/_base.py:200:9: error[invalid-assignment] Object of type `ReferenceType[Self@inverse]` is not assignable to attribute `_invweak` of type `ReferenceType[BidictBase[VT@BidictBase, KT@BidictBase]] | None`

attrs (https://github.com/python-attrs/attrs)
- tests/test_make.py:2872:17: error[invalid-assignment] Object of type `type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15 | tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15]` is not assignable to `<class 'tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15'>`
+ tests/test_make.py:2872:17: error[invalid-assignment] Object of type `type[tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15]` is not assignable to `<class 'tests.test_make.TestAutoDetect.<locals of function 'test_total_ordering'>.C @ tests/test_make.py:2858:15'>`

anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:2541:50: error[invalid-assignment] Object of type `Future[_T@run_coroutine_threadsafe] | Future[T_Retval@run_async_from_thread]` is not assignable to `Future[T_Retval@run_async_from_thread]`
+ src/anyio/_backends/_asyncio.py:2541:50: error[invalid-assignment] Object of type `Future[_T@run_coroutine_threadsafe]` is not assignable to `Future[T_Retval@run_async_from_thread]`

scrapy (https://github.com/scrapy/scrapy)
- scrapy/utils/decorators.py:40:16: error[invalid-assignment] Object of type `(func: (**_P@deprecated) -> _T@deprecated) -> _T@deprecated` is not assignable to `def deco[**_P](func: (**_P@deprecated) -> _T) -> (**_P@deprecated) -> _T`
+ scrapy/utils/decorators.py:40:16: error[invalid-assignment] Object of type `(...) -> _T@deprecated` is not assignable to `def deco[**_P](func: (**_P@deprecated) -> _T) -> (**_P@deprecated) -> _T`
- tests/mockserver/http_base.py:49:28: error[unresolved-attribute] Object of type `str` has no attribute `decode`
- tests/mockserver/http_base.py:49:28: warning[possibly-missing-attribute] Attribute `readline` may be missing on object of type `IO[str] | None`
+ tests/mockserver/http_base.py:49:28: warning[possibly-missing-attribute] Attribute `readline` may be missing on object of type `IO[bytes] | None`
- tests/mockserver/http_base.py:53:29: error[unresolved-attribute] Object of type `str` has no attribute `decode`
- tests/mockserver/http_base.py:53:29: warning[possibly-missing-attribute] Attribute `readline` may be missing on object of type `IO[str] | None`
+ tests/mockserver/http_base.py:53:29: warning[possibly-missing-attribute] Attribute `readline` may be missing on object of type `IO[bytes] | None`
- tests/test_pipeline_files.py:370:28: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[str]]` is not assignable to `list[str]`
+ tests/test_pipeline_files.py:370:28: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'>]` is not assignable to `list[str]`
- tests/test_pipeline_files.py:371:35: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[dict[str, str]]]` is not assignable to `list[dict[str, str]]`
- tests/test_pipeline_files.py:373:35: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[str]]` is not assignable to `list[str]`
+ tests/test_pipeline_files.py:371:35: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'>]` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_files.py:373:35: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'>]` is not assignable to `list[str]`
- tests/test_pipeline_files.py:374:42: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[dict[str, str]]]` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_files.py:374:42: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'>]` is not assignable to `list[dict[str, str]]`
- tests/test_pipeline_images.py:332:29: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[str]]` is not assignable to `list[str]`
+ tests/test_pipeline_images.py:332:29: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'>]` is not assignable to `list[str]`
- tests/test_pipeline_images.py:333:36: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[dict[str, str]]]` is not assignable to `list[dict[str, str]]`
- tests/test_pipeline_images.py:335:36: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[str]]` is not assignable to `list[str]`
+ tests/test_pipeline_images.py:333:36: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'>]` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_images.py:335:36: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'>]` is not assignable to `list[str]`
- tests/test_pipeline_images.py:336:43: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'> | list[dict[str, str]]]` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_images.py:336:43: error[invalid-assignment] Object of type `dataclasses.Field[<class 'list'>]` is not assignable to `list[dict[str, str]]`
- Found 1789 diagnostics
+ Found 1787 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/utils.py:3441:12: error[invalid-return-type] Return type does not match returned value: expected `InstanceConfigDict`, found `(Unknown & ~None) | dict[str, Any] | InstanceConfigDict`
+ paasta_tools/utils.py:3441:12: error[invalid-return-type] Return type does not match returned value: expected `InstanceConfigDict`, found `(Unknown & ~None) | dict[str, Any]`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/execute.py:1657:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult | CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
+ src/graphql/execution/execute.py:1657:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
- src/graphql/execution/execute.py:1809:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[StreamItemResult | CoroutineType[Any, Any, StreamItemResult]]`
+ src/graphql/execution/execute.py:1809:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[CoroutineType[Any, Any, StreamItemResult] | StreamItemResult]`
- tests/pyutils/test_boxed_awaitabe_or_value.py:29:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[int | Future[Unknown]]` is not assignable to `BoxedAwaitableOrValue[int]`
+ tests/pyutils/test_boxed_awaitabe_or_value.py:29:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[Future[Unknown] | Unknown]` is not assignable to `BoxedAwaitableOrValue[int]`
- tests/pyutils/test_boxed_awaitabe_or_value.py:36:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[int | CoroutineType[Any, Any, int]]` is not assignable to `BoxedAwaitableOrValue[int]`
+ tests/pyutils/test_boxed_awaitabe_or_value.py:36:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, int] | int]` is not assignable to `BoxedAwaitableOrValue[int]`
- tests/pyutils/test_boxed_awaitabe_or_value.py:41:51: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[list[int] | Future[Unknown]]` is not assignable to `BoxedAwaitableOrValue[list[int]]`
+ tests/pyutils/test_boxed_awaitabe_or_value.py:41:51: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[Future[Unknown] | Unknown]` is not assignable to `BoxedAwaitableOrValue[list[int]]`
- tests/pyutils/test_boxed_awaitabe_or_value.py:56:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[int | CoroutineType[Any, Any, int]]` is not assignable to `BoxedAwaitableOrValue[int]`
+ tests/pyutils/test_boxed_awaitabe_or_value.py:56:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, int] | int]` is not assignable to `BoxedAwaitableOrValue[int]`

porcupine (https://github.com/Akuli/porcupine)
+ porcupine/plugins/run/no_terminal.py:106:13: error[invalid-assignment] Object of type `Popen[str]` is not assignable to attribute `_shell_process` of type `Popen[bytes] | None`
+ porcupine/plugins/run/no_terminal.py:121:16: warning[possibly-missing-attribute] Attribute `stdout` may be missing on object of type `Popen[bytes] | None`
+ porcupine/plugins/run/no_terminal.py:129:18: warning[possibly-missing-attribute] Attribute `wait` may be missing on object of type `Popen[bytes] | None`
- Found 18 diagnostics
+ Found 21 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[bytes | str]`
+ pydantic/_internal/_validators.py:161:16: error[invalid-return-type] Return type does not match returned value: expected `Pattern[bytes]`, found `Pattern[str]`
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

pandera (https://github.com/pandera-dev/pandera)
- tests/mypy/pandas_modules/pandas_dataframe.py:35:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[AnotherSchema] | DataFrame[SchemaOut]`
+ tests/mypy/pandas_modules/pandas_dataframe.py:35:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[AnotherSchema]`
- tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[Schema] | DataFrame[SchemaOut]`
+ tests/mypy/pandas_modules/pandas_dataframe.py:41:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame[SchemaOut]`, found `DataFrame[Schema]`

Expression (https://github.com/cognitedata/Expression)
- expression/collections/maptree.py:112:28: error[invalid-return-type] Return type does not match returned value: expected `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`, found `Option[MapTreeLeaf[Key@rebalance, object]]`
+ expression/collections/maptree.py:112:28: error[invalid-return-type] Return type does not match returned value: expected `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`, found `Option[MapTreeLeaf[Key@rebalance | Unknown, object]]`
- expression/collections/maptree.py:114:25: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Key@rebalance`, found `object`
- expression/collections/maptree.py:121:24: error[invalid-return-type] Return type does not match returned value: expected `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`, found `Option[MapTreeLeaf[Key@rebalance, object]]`
+ expression/collections/maptree.py:121:24: error[invalid-return-type] Return type does not match returned value: expected `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`, found `Option[MapTreeLeaf[Key@rebalance | Unknown, object]]`
- expression/collections/maptree.py:121:51: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Key@rebalance`, found `object`
- expression/collections/maptree.py:121:71: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Key@rebalance, object]]`, found `Option[Top[MapTreeLeaf[Unknown, Unknown]]]`
+ expression/collections/maptree.py:121:71: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Key@rebalance | Unknown, object]]`, found `Option[Top[MapTreeLeaf[Unknown, Unknown]]]`
- expression/collections/maptree.py:132:32: error[invalid-return-type] Return type does not match returned value: expected `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`, found `Option[MapTreeLeaf[Key@rebalance, object]]`
+ expression/collections/maptree.py:132:32: error[invalid-return-type] Return type does not match returned value: expected `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`, found `Option[MapTreeLeaf[Unknown | Key@rebalance, object]]`
- expression/collections/maptree.py:134:29: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Key@rebalance`, found `object`
- expression/collections/maptree.py:141:28: error[invalid-return-type] Return type does not match returned value: expected `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`, found `Option[MapTreeLeaf[Key@rebalance, object]]`
+ expression/collections/maptree.py:141:28: error[invalid-return-type] Return type does not match returned value: expected `Option[MapTreeLeaf[Key@rebalance, Value@rebalance]]`, found `Option[MapTreeLeaf[Unknown | Key@rebalance, object]]`
- expression/collections/maptree.py:141:31: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Key@rebalance, object]]`, found `Option[Top[MapTreeLeaf[Unknown, Unknown]]]`
+ expression/collections/maptree.py:141:31: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Option[MapTreeLeaf[Unknown | Key@rebalance, object]]`, found `Option[Top[MapTreeLeaf[Unknown, Unknown]]]`
- expression/collections/maptree.py:141:41: error[invalid-argument-type] Argument to function `mk` is incorrect: Expected `Key@rebalance`, found `object`
+ tests/test_compose.py:21:16: error[invalid-assignment] Object of type `(Never, /) -> Never` is not assignable to `(int, /) -> int`
- tests/test_result.py:491:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Literal[42], Any]`, found `Result[Literal[0, 42], Any]`
+ tests/test_result.py:491:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Literal[42], Any]`, found `Result[Literal[0], Any]`
- tests/test_result.py:509:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Any, Literal["original error"]]`, found `Result[Any, Literal["new error", "original error"]]`
+ tests/test_result.py:509:21: error[invalid-argument-type] Argument to bound method `or_else` is incorrect: Expected `Result[Any, Literal["original error"]]`, found `Result[Any, Literal["new error"]]`
- Found 205 diagnostics
+ Found 202 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/proxy/layers/http/test_http.py:150:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Connection | _Placeholder[Connection]`
+ test/mitmproxy/proxy/layers/http/test_http.py:150:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http.py:1641:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Connection | _Placeholder[Connection]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1641:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:1796:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Connection | _Placeholder[Connection]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1796:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:1838:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Connection | _Placeholder[Connection]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1838:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http1.py:59:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `HttpEvent | _Placeholder[HttpEvent]`
+ test/mitmproxy/proxy/layers/http/test_http1.py:59:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `RequestHeaders | _Placeholder[RequestHeaders]`
- test/mitmproxy/proxy/layers/http/test_http1.py:81:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `HttpEvent | _Placeholder[HttpEvent]`
+ test/mitmproxy/proxy/layers/http/test_http1.py:81:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `RequestHeaders | _Placeholder[RequestHeaders]`
- test/mitmproxy/proxy/layers/http/test_http1.py:104:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `HttpEvent | _Placeholder[HttpEvent]`
+ test/mitmproxy/proxy/layers/http/test_http1.py:104:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `RequestHeaders | _Placeholder[RequestHeaders]`
- test/mitmproxy/proxy/layers/http/test_http1.py:110:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `HttpEvent | _Placeholder[HttpEvent]`
+ test/mitmproxy/proxy/layers/http/test_http1.py:110:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `RequestHeaders | _Placeholder[RequestHeaders]`
- test/mitmproxy/proxy/layers/http/test_http2.py:79:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `bytes | _Placeholder[bytes]`
+ test/mitmproxy/proxy/layers/http/test_http2.py:79:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/http/test_http2.py:1085:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `HttpEvent | _Placeholder[HttpEvent]`
+ test/mitmproxy/proxy/layers/http/test_http2.py:1085:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `ResponseHeaders | _Placeholder[ResponseHeaders]`
- test/mitmproxy/proxy/layers/http/test_http3.py:1086:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `HttpEvent | _Placeholder[HttpEvent]`
+ test/mitmproxy/proxy/layers/http/test_http3.py:1086:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `ResponseHeaders | _Placeholder[ResponseHeaders]`
- test/mitmproxy/proxy/layers/http/test_http_fuzz.py:343:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `bytes | _Placeholder[bytes]`
+ test/mitmproxy/proxy/layers/http/test_http_fuzz.py:343:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1042:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `bytes | _Placeholder[bytes]`
+ test/mitmproxy/proxy/layers/quic/test__stream_layers.py:1042:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `Unknown | _Placeholder[Unknown]`
- test/mitmproxy/proxy/layers/test_tls.py:795:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `bytes | _Placeholder[bytes]`
+ test/mitmproxy/proxy/layers/test_tls.py:795:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `Unknown | _Placeholder[Unknown]`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/util/_serialise.py:49:16: error[no-matching-overload] No overload of function `sorted` matches arguments
- Found 344 diagnostics
+ Found 343 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/optimize/analysis/lookahead.py:48:13: error[invalid-argument-type] Argument to bound method `backtest` is incorrect: Expected `dict[Unknown, Unknown]`, found `DataFrame | dict[Unknown, Unknown]`
+ freqtrade/optimize/analysis/lookahead.py:48:13: error[invalid-argument-type] Argument to bound method `backtest` is incorrect: Expected `dict[Unknown, Unknown]`, found `DataFrame`

discord.py (https://github.com/Rapptz/discord.py)
+ discord/client.py:365:16: error[invalid-return-type] Return type does not match returned value: expected `ConnectionState[Self@_get_state]`, found `ConnectionState[Client]`
- Found 551 diagnostics
+ Found 552 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- tests/typing_example.py:100:13: error[invalid-assignment] Object of type `Connection[tuple[Any, ...]] | Connection[int]` is not assignable to `Connection[int]`
+ tests/typing_example.py:100:13: error[invalid-assignment] Object of type `Connection[tuple[Any, ...]]` is not assignable to `Connection[int]`
- tests/typing_example.py:110:13: error[invalid-assignment] Object of type `Connection[tuple[Any, ...]] | Connection[Person]` is not assignable to `Connection[Person]`
+ tests/typing_example.py:110:13: error[invalid-assignment] Object of type `Connection[tuple[Any, ...]]` is not assignable to `Connection[Person]`

meson (https://github.com/mesonbuild/meson)
- docs/refman/generatorbase.py:34:16: error[invalid-return-type] Return type does not match returned value: expected `list[_N@sorted_and_filtered]`, found `list[NamedObject]`
+ docs/refman/generatorbase.py:34:16: error[invalid-return-type] Return type does not match returned value: expected `list[_N@sorted_and_filtered]`, found `list[NamedObject | Unknown]`
- mesonbuild/cargo/builder.py:191:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `int | str` does not satisfy constraints (`int`, `str`, `bool`) of type variable `TV_TokenTypes`
- mesonbuild/interpreter/type_checking.py:178:63: error[invalid-assignment] Object of type `KwargInfo[list[str | int] | None]` is not assignable to `KwargInfo[list[str | int]]`
+ mesonbuild/interpreter/type_checking.py:178:63: error[invalid-assignment] Object of type `KwargInfo[list[Unknown] | list[str | int] | None]` is not assignable to `KwargInfo[list[str | int]]`
- mesonbuild/interpreter/type_checking.py:488:48: error[invalid-assignment] Object of type `KwargInfo[list[str | File | CustomTarget | ... omitted 5 union elements] | None]` is not assignable to `KwargInfo[list[str | File | CustomTarget | ... omitted 5 union elements]]`
+ mesonbuild/interpreter/type_checking.py:488:48: error[invalid-assignment] Object of type `KwargInfo[None | list[Unknown]]` is not assignable to `KwargInfo[list[str | File | CustomTarget | ... omitted 5 union elements]]`
- mesonbuild/interpreter/type_checking.py:495:45: error[invalid-assignment] Object of type `KwargInfo[dict[str, str] | str | list[str]]` is not assignable to `KwargInfo[dict[str, str]]`
+ mesonbuild/interpreter/type_checking.py:495:45: error[invalid-assignment] Object of type `KwargInfo[str | list[str] | dict[str, str] | dict[Unknown, Unknown]]` is not assignable to `KwargInfo[dict[str, str]]`
- mesonbuild/rewriter.py:926:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Token[str]`, found `Token[str | int]`
+ mesonbuild/rewriter.py:926:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Token[str]`, found `Token[int]`
- Found 2157 diagnostics
+ Found 2156 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/backends/h5netcdf_.py:465:16: error[invalid-return-type] Return type does not match returned value: expected `str | ReadBuffer[Unknown] | AbstractDataStore`, found `str | (PathLike[Any] & ~bytes) | ReadBuffer[Unknown] | AbstractDataStore`
+ xarray/backends/h5netcdf_.py:465:16: error[invalid-return-type] Return type does not match returned value: expected `str | ReadBuffer[Unknown] | AbstractDataStore`, found `str | (PathLike[Any] & ~bytes) | (ReadBuffer[Unknown] & ~bytes) | (AbstractDataStore & ~bytes)`
- xarray/backends/scipy_.py:326:16: error[invalid-return-type] Return type does not match returned value: expected `str | ReadBuffer[Unknown] | AbstractDataStore`, found `str | (PathLike[Any] & ~bytes) | ReadBuffer[Unknown] | AbstractDataStore`
+ xarray/backends/scipy_.py:326:16: error[invalid-return-type] Return type does not match returned value: expected `str | ReadBuffer[Unknown] | AbstractDataStore`, found `str | (PathLike[Any] & ~bytes) | (ReadBuffer[Unknown] & ~bytes) | (AbstractDataStore & ~bytes)`

setuptools (https://github.com/pypa/setuptools)
- setuptools/_distutils/compilers/C/base.py:484:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[str], str]`, found `tuple[list[str], str | None]`
+ setuptools/_distutils/compilers/C/base.py:484:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[str], str]`, found `tuple[list[Unknown], str | None]`

scipy-stubs (https://github.com/scipy/scipy-stubs)
+ tests/optimize/test_constraints.pyi:32:38: error[invalid-assignment] Object of type `Bounds[tuple[Any, ...], float64]` is not assignable to `Bounds[tuple[int], floating[_32Bit]]`
+ tests/optimize/test_constraints.pyi:33:43: error[invalid-assignment] Object of type `Bounds[tuple[Any, ...], float64]` is not assignable to `Bounds[tuple[int, int], floating[_32Bit]]`
- Found 1298 diagnostics
+ Found 1300 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-databricks/prefect_databricks/models/jobs.py:3849:38: error[invalid-assignment] Object of type `dataclasses.Field[Literal["{}"] | dict[str, Any] | None]` is not assignable to `dict[str, Any] | None`
+ src/integrations/prefect-databricks/prefect_databricks/models/jobs.py:3849:38: error[invalid-assignment] Object of type `dataclasses.Field[Literal["{}"]]` is not assignable to `dict[str, Any] | None`
- src/integrations/prefect-gcp/prefect_gcp/workers/cloud_run_v2.py:147:22: error[invalid-assignment] Object of type `dataclasses.Field[None | str]` is not assignable to `str`
+ src/integrations/prefect-gcp/prefect_gcp/workers/cloud_run_v2.py:147:22: error[invalid-assignment] Object of type `dataclasses.Field[None]` is not assignable to `str`
- src/integrations/prefect-kubernetes/prefect_kubernetes/worker.py:642:69: error[invalid-assignment] Object of type `dataclasses.Field[Literal[KubernetesImagePullPolicy.IF_NOT_PRESENT, "IfNotPresent", "Always", "Never"]]` is not assignable to `Literal["IfNotPresent", "Always", "Never"]`
+ src/integrations/prefect-kubernetes/prefect_kubernetes/worker.py:642:69: error[invalid-assignment] Object of type `dataclasses.Field[Literal[KubernetesImagePullPolicy.IF_NOT_PRESENT]]` is not assignable to `Literal["IfNotPresent", "Always", "Never"]`
- src/prefect/_internal/concurrency/calls.py:355:24: error[invalid-return-type] Return type does not match returned value: expected `Awaitable[None] | None`, found `_T@run | Awaitable[None] | None`
+ src/prefect/_internal/concurrency/calls.py:355:24: error[invalid-return-type] Return type does not match returned value: expected `Awaitable[None] | None`, found `_T@run`
- src/prefect/_states.py:231:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[State[Any]]`
+ src/prefect/_states.py:231:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[Unknown]`
- src/prefect/cli/deploy/_core.py:362:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `dict[str, Any] | None`
+ src/prefect/cli/deploy/_core.py:362:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Unknown | None`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/server/api/block_types.py:125:13: error[invalid-argument-type] Argument to function `_should_update_block_type` is incorrect: Expected `prefect.client.schemas.objects.BlockType`, found `prefect.server.schemas.core.BlockType | prefect.client.schemas.objects.BlockType`
+ src/prefect/server/api/block_types.py:125:13: error[invalid-argument-type] Argument to function `_should_update_block_type` is incorrect: Expected `prefect.client.schemas.objects.BlockType`, found `prefect.server.schemas.core.BlockType`
- src/prefect/states.py:369:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[State[Any]]`
+ src/prefect/states.py:369:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[State[Any]]`, found `Collection[Unknown]`
- src/prefect/transactions.py:251:16: error[invalid-return-type] Return type does not match returned value: expected `Self@get_active | None`, found `None | Self@get_active | <special-form 'typing.Self'>`
+ src/prefect/transactions.py:251:16: error[invalid-return-type] Return type does not match returned value: expected `Self@get_active | None`, found `None | <special-form 'typing.Self'>`
- src/prefect/variables.py:90:16: error[invalid-return-type] Return type does not match returned value: expected `Variable[str | int | float | ... omitted 3 union elements]`, found `Self@aset | Variable[str | int | float | ... omitted 3 union elements]`
+ src/prefect/variables.py:90:16: error[invalid-return-type] Return type does not match returned value: expected `Variable[str | int | float | ... omitted 3 union elements]`, found `Self@aset`
- src/prefect/variables.py:139:20: error[invalid-return-type] Return type does not match returned value: expected `Variable[str | int | float | ... omitted 3 union elements]`, found `Self@set | Variable[str | int | float | ... omitted 3 union elements]`
+ src/prefect/variables.py:139:20: error[invalid-return-type] Return type does not match returned value: expected `Variable[str | int | float | ... omitted 3 union elements]`, found `Self@set`
- Found 5406 diagnostics
+ Found 5411 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/logging/patch.py:73:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | Literal["0"] | int`
+ ddtrace/contrib/internal/logging/patch.py:73:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | Literal["0"]`
- ddtrace/contrib/internal/logging/patch.py:74:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | Literal["0"] | int`
+ ddtrace/contrib/internal/logging/patch.py:74:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | Literal["0"]`

jax (https://github.com/google/jax)
+ jax/_src/interpreters/mlir.py:1477:5: error[invalid-assignment] Object of type `OrderedDict[Effect | str, Unknown]` is not assignable to attribute `_tokens` of type `OrderedDict[Effect, Unknown]`
- Found 2874 diagnostics
+ Found 2875 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79:7]`, found `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79:7 | bokeh.model.model.Model @ src/bokeh/model/model.pyi:40:7]`
+ src/bokeh/model/model.py:496:16: error[invalid-return-type] Return type does not match returned value: expected `set[bokeh.model.model.Model @ src/bokeh/model/model.py:79:7]`, found `set[bokeh.model.model.Model @ src/bokeh/model/model.pyi:40:7]`
- src/bokeh/settings.py:747:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((int | None | str, /) -> int | None | str) | None`, found `def convert_logging(value: str | int) -> int | None`
+ src/bokeh/settings.py:747:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> str) | None`, found `def convert_logging(value: str | int) -> int | None`

ibis (https://github.com/ibis-project/ibis)
- ibis/expr/types/generic.py:2750:16: error[invalid-return-type] Return type does not match returned value: expected `Column`, found `Scalar | Column | @Todo`
+ ibis/expr/types/generic.py:2750:16: error[invalid-return-type] Return type does not match returned value: expected `Column`, found `Scalar | @Todo`
- ibis/expr/types/generic.py:2797:16: error[invalid-return-type] Return type does not match returned value: expected `Column`, found `Scalar | Column | @Todo`
+ ibis/expr/types/generic.py:2797:16: error[invalid-return-type] Return type does not match returned value: expected `Column`, found `Scalar | @Todo`
- ibis/expr/types/logical.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `BooleanColumn`, found `BooleanScalar | BooleanColumn | @Todo`
+ ibis/expr/types/logical.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `BooleanColumn`, found `BooleanScalar | @Todo`
- ibis/expr/types/numeric.py:1140:16: error[invalid-return-type] Return type does not match returned value: expected `NumericColumn`, found `NumericScalar | NumericColumn | @Todo`
+ ibis/expr/types/numeric.py:1140:16: error[invalid-return-type] Return type does not match returned value: expected `NumericColumn`, found `NumericScalar | @Todo`
- ibis/expr/types/numeric.py:1240:16: error[invalid-return-type] Return type does not match returned value: expected `NumericColumn`, found `NumericScalar | NumericColumn | @Todo`
+ ibis/expr/types/numeric.py:1240:16: error[invalid-return-type] Return type does not match returned value: expected `NumericColumn`, found `NumericScalar | @Todo`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bottom[Bus[Any]] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bottom[Bus[Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Bottom[Yarn[Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/indexes/timedeltaindex/test_floordiv.py:28:12: error[invalid-return-type] Return type does not match returned value: expected `TimedeltaIndex`, found `Index[Any] | TimedeltaIndex`
+ tests/indexes/timedeltaindex/test_floordiv.py:28:12: error[invalid-return-type] Return type does not match returned value: expected `TimedeltaIndex`, found `Index[Any]`
- tests/indexes/timedeltaindex/test_mul.py:24:12: error[invalid-return-type] Return type does not match returned value: expected `TimedeltaIndex`, found `Index[Any] | TimedeltaIndex`
+ tests/indexes/timedeltaindex/test_mul.py:24:12: error[invalid-return-type] Return type does not match returned value: expected `TimedeltaIndex`, found `Index[Any]`

core (https://github.com/home-assistant/core)
- homeassistant/auth/models.py:114:23: error[no-matching-overload] No overload of function `attrib` matches arguments
- homeassistant/components/min_max/sensor.py:257:57: error[invalid-assignment] Object of type `Event[EventStateChangedData | dict[str, Unknown | str | State | None]]` is not assignable to `Event[EventStateChangedData]`
+ homeassistant/components/min_max/sensor.py:257:57: error[invalid-assignment] Object of type `Event[dict[str, Unknown | str | State | None]]` is not assignable to `Event[EventStateChangedData]`
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~12MB
+     struct fields = ~11MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-17 03:59_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 2 | 6 | 32 |
| `invalid-return-type` | 2 | 0 | 34 |
| `invalid-assignment` | 5 | 0 | 29 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `possibly-missing-attribute` | 2 | 0 | 2 |
| `no-matching-overload` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 2 | 0 |
| `unused-ignore-comment` | 0 | 2 | 0 |
| **Total** | **11** | **12** | **104** |


**[Full report with detailed diff](https://902123ef.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://902123ef.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @codspeed-hq[bot] on 2026-01-17 04:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **improve performance by 4.94%**




`⚡ 2` improved benchmarks  
`✅ 21` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-diagnostics?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 22 s | 21 s | +4.64% |
| ⚡ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-diagnostics?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 8.3 s | 7.9 s | +4.94% |
---

<sub>Comparing <code>ibraheem/bidi-diagnostics</code> (d6a2820) with <code>main</code> (938c1c5)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-diagnostics?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fbidi-diagnostics?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:8706 on 2026-01-18 17:58_

Is this (only lazily cloning the bindings if it's necessary) the reason for the performance improvement?

It looks like `try_narrow()` could be called multiple times, so I wonder if you could do this to micro-optimize it even more:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 911f9b5253..2063c7a09d 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -8700,9 +8700,12 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
 
         // We silence diagnostics until we successfully narrow to a specific type.
         let was_in_multi_inference = self.context.set_multi_inference(true);
+        let mut speculated_bindings_cache = None;
 
         let mut try_narrow = |narrowed_ty| {
-            let mut speculated_bindings = bindings.clone();
+            let speculated_bindings =
+                speculated_bindings_cache.get_or_insert_with(|| bindings.clone());
+
             let narrowed_tcx = TypeContext::new(Some(narrowed_ty));
```

---

_@AlexWaygood reviewed on 2026-01-18 18:02_

---

_@ibraheemdev reviewed on 2026-01-18 21:31_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/infer/builder.rs`:8706 on 2026-01-18 21:31_

This was actually a bug. We should be creating a fresh `Bindings` every time, because the speculative calls to `check_types` mutate internal state. This led to a few bugs after the changes (I'm not sure why we didn't notice before).

As a performance consideration, it is probably possible to reset the fields that were mutated instead of cloning, but I'm not sure it matters. 

---
