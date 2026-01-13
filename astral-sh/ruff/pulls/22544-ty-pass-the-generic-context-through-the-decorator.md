```yaml
number: 22544
title: "[ty] Pass the generic context through the decorator"
type: pull_request
state: open
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
draft: true
base: main
head: dhruv/paramspec-decorator
created_at: 2026-01-13T10:38:33Z
updated_at: 2026-01-13T10:41:30Z
url: https://github.com/astral-sh/ruff/pull/22544
synced_at: 2026-01-13T11:27:31Z
```

# [ty] Pass the generic context through the decorator

---

_@dhruvmanila_

## Summary

fixes: https://github.com/astral-sh/ty/issues/2336
fixes: https://github.com/astral-sh/ty/issues/2382

## Test Plan

<!-- How was it tested? -->


---

_Label `bug` added by @dhruvmanila on 2026-01-13 10:38_

---

_Label `ty` added by @dhruvmanila on 2026-01-13 10:38_

---

_Comment by @astral-sh-bot[bot] on 2026-01-13 10:40_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-13 10:40:12.765719090 +0000
+++ new-output.txt	2026-01-13 10:40:13.091720349 +0000
@@ -260,16 +260,8 @@
 constructors_callable.py:164:1: error[type-assertion-failure] Type `Class7[int]` does not match asserted type `Class7[int] | Class7[str]`
 constructors_callable.py:165:1: error[type-assertion-failure] Type `Class7[str]` does not match asserted type `Class7[int] | Class7[str]`
 constructors_callable.py:182:13: info[revealed-type] Revealed type: `(x: list[T@Class8], y: list[T@Class8]) -> Class8[T@Class8]`
-constructors_callable.py:183:1: error[type-assertion-failure] Type `Class8[str]` does not match asserted type `Class8[T@Class8]`
-constructors_callable.py:183:16: error[invalid-argument-type] Argument is incorrect: Expected `list[T@Class8]`, found `list[Unknown | str]`
-constructors_callable.py:183:22: error[invalid-argument-type] Argument is incorrect: Expected `list[T@Class8]`, found `list[Unknown | str]`
-constructors_callable.py:184:4: error[invalid-argument-type] Argument is incorrect: Expected `list[T@Class8]`, found `list[Unknown | int]`
-constructors_callable.py:184:9: error[invalid-argument-type] Argument is incorrect: Expected `list[T@Class8]`, found `list[Unknown | str]`
+constructors_callable.py:183:1: error[type-assertion-failure] Type `Class8[str]` does not match asserted type `Class8[Unknown | str]`
 constructors_callable.py:193:13: info[revealed-type] Revealed type: `(x: list[T@__init__], y: list[T@__init__]) -> Class9`
-constructors_callable.py:194:16: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
-constructors_callable.py:194:22: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
-constructors_callable.py:195:4: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | int]`
-constructors_callable.py:195:9: error[invalid-argument-type] Argument is incorrect: Expected `list[T@__init__]`, found `list[Unknown | str]`
 dataclasses_descriptors.py:23:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | Desc1`
 dataclasses_descriptors.py:50:63: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[T@Desc2] | T@Desc2`
 dataclasses_descriptors.py:66:1: error[type-assertion-failure] Type `int` does not match asserted type `int | Desc2[int]`
@@ -1029,4 +1021,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1031 diagnostics
+Found 1023 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2026-01-13 10:41_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:2542:13: error[invalid-argument-type] Argument to bound method `run` is incorrect: Expected `Coroutine[Any, Any, _T@run_coroutine_threadsafe]`, found `CoroutineType[Any, Any, T_Retval@run_async_from_thread]`
- Found 93 diagnostics
+ Found 92 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
- src/async_utils/gen_transform.py:109:36: error[invalid-argument-type] Argument to function `to_thread` is incorrect: Expected `Queue[Y@_consumer]`, found `Queue[Y@_sync_to_async_gen]`
- src/async_utils/gen_transform.py:109:54: error[invalid-argument-type] Argument to function `to_thread` is incorrect: Expected `(**P@_consumer) -> Generator[Y@_consumer, None, None]`, found `(**P@_sync_to_async_gen) -> Generator[Y@_sync_to_async_gen, None, None]`
- src/async_utils/gen_transform.py:109:66: error[invalid-argument-type] Argument to function `to_thread` is incorrect: Expected `P@_consumer.args`, found `P@_sync_to_async_gen.args`
- src/async_utils/gen_transform.py:109:69: error[invalid-argument-type] Argument to function `to_thread` is incorrect: Expected `P@_consumer.kwargs`, found `P@_sync_to_async_gen.kwargs`
- Found 9 diagnostics
+ Found 5 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/tests/test_tools.py:72:39: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilter]`, found `list[Unknown | None | int]`
- asynq/tests/test_tools.py:74:34: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilter]`, found `list[Unknown | None | int]`
- asynq/tests/test_tools.py:82:53: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@afilterfalse]`, found `list[Unknown | None | int]`
- asynq/tests/test_tools.py:90:53: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asift]`, found `list[Unknown | None | int]`
- asynq/tests/test_tools.py:95:45: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[Unknown | None]`
- asynq/tests/test_tools.py:96:44: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[Unknown | int]`
- asynq/tests/test_tools.py:98:58: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amap]`, found `list[Unknown | None | str]`
- asynq/tests/test_tools.py:103:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[Unknown | None]`
- asynq/tests/test_tools.py:104:37: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[Unknown | bool | None]`
- asynq/tests/test_tools.py:105:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@asorted]`, found `list[Unknown | int]`
- asynq/tests/test_tools.py:109:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `Literal[1]`
+ asynq/tests/test_tools.py:109:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[Unknown]`, found `Literal[1]`
- asynq/tests/test_tools.py:110:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | int | None]`
- asynq/tests/test_tools.py:111:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `GeneratorType[Literal[1] | None, None, None]`
- asynq/tests/test_tools.py:112:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | int]`
- asynq/tests/test_tools.py:113:31: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | list[Unknown | int]]`
- asynq/tests/test_tools.py:115:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `Literal[1]`
+ asynq/tests/test_tools.py:115:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[Unknown]`, found `Literal[1]`
- asynq/tests/test_tools.py:126:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `Literal[1]`
+ asynq/tests/test_tools.py:126:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[Unknown]`, found `Literal[1]`
- asynq/tests/test_tools.py:127:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[Unknown | int | None]`
- asynq/tests/test_tools.py:128:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `GeneratorType[Literal[1] | None, None, None]`
- asynq/tests/test_tools.py:129:25: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[Unknown | int]`
- asynq/tests/test_tools.py:129:30: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((_T@amin, /) -> Any) | None`, found `list[Unknown | int]`
+ asynq/tests/test_tools.py:129:30: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((int, /) -> Any) | None`, found `list[Unknown | int]`
- asynq/tests/test_tools.py:130:25: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `list[Unknown | list[Unknown | int]]`
- asynq/tests/test_tools.py:132:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amin]`, found `Literal[1]`
+ asynq/tests/test_tools.py:132:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[Unknown]`, found `Literal[1]`
- asynq/tests/test_tools.py:132:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((_T@amin, /) -> Any) | None`, found `Literal[2]`
+ asynq/tests/test_tools.py:132:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((Unknown, /) -> Any) | None`, found `Literal[2]`
- asynq/tests/test_typing.py:41:43: error[invalid-argument-type] Argument to bound method `asyncio` is incorrect: Expected `_T@generic`, found `str`
- asynq/tests/test_typing.py:54:16: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((...) -> _T@async_call) | ((...) -> FutureBase[_T@async_call])`, found `def f(x: int) -> str`
- asynq/tests/test_typing.py:56:20: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((...) -> _T@async_call) | ((...) -> FutureBase[_T@async_call])`, found `def f(x: int) -> str`
- asynq/tests/test_typing.py:61:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `Literal[1]`
+ asynq/tests/test_typing.py:61:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[Sized]`, found `Literal[1]`
- asynq/tests/test_typing.py:61:32: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((_T@amax, /) -> Any) | None`, found `def len(obj: Sized, /) -> int`
- asynq/tests/test_typing.py:62:26: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `Iterable[_T@amax]`, found `list[Unknown | int]`
- asynq/tests/test_typing.py:62:34: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((_T@amax, /) -> Any) | None`, found `def len(obj: Sized, /) -> int`
+ asynq/tests/test_typing.py:62:34: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `((Unknown | int | Sized, /) -> Any) | None`, found `def len(obj: Sized, /) -> int`
- Found 217 diagnostics
+ Found 194 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/porcelain/__init__.py:643:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo | Repo, bool | None]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`
+ dulwich/porcelain/__init__.py:643:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo | Repo, bool | None]`, found `_GeneratorContextManager[(str & BaseRepo) | (PathLike[str] & BaseRepo) | T@open_repo, None, None]`
- dulwich/porcelain/__init__.py:643:38: error[invalid-argument-type] Argument is incorrect: Expected `T@_noop_context_manager`, found `(str & BaseRepo) | (PathLike[str] & BaseRepo) | T@open_repo`
- dulwich/porcelain/__init__.py:697:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo_closing | Repo, bool | None]`, found `_GeneratorContextManager[T@_noop_context_manager, None, None]`
+ dulwich/porcelain/__init__.py:697:16: error[invalid-return-type] Return type does not match returned value: expected `AbstractContextManager[T@open_repo_closing | Repo, bool | None]`, found `_GeneratorContextManager[(str & BaseRepo) | (bytes & BaseRepo) | (PathLike[str] & BaseRepo) | T@open_repo_closing, None, None]`
- dulwich/porcelain/__init__.py:697:38: error[invalid-argument-type] Argument is incorrect: Expected `T@_noop_context_manager`, found `(str & BaseRepo) | (bytes & BaseRepo) | (PathLike[str] & BaseRepo) | T@open_repo_closing`
- Found 231 diagnostics
+ Found 229 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/core.py:683:35: error[invalid-argument-type] Argument is incorrect: Expected `Context[BotT@cog_command_error]`, found `Context[BotT@dispatch_error]`
- discord/ext/commands/core.py:1314:63: error[invalid-argument-type] Argument to function `maybe_coroutine` is incorrect: Expected `Context[BotT@cog_check]`, found `Context[BotT@can_run]`
- discord/ext/commands/hybrid.py:430:45: error[invalid-argument-type] Argument to function `maybe_coroutine` is incorrect: Expected `Interaction[ClientT@interaction_check]`, found `Interaction[Client]`
- Found 548 diagnostics
+ Found 545 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_run.py:2444:27: error[invalid-argument-type] Argument is incorrect: Expected `T@async_yield`, found `object`
- src/trio/_core/_tests/test_run.py:2502:27: error[invalid-argument-type] Argument is incorrect: Expected `T@async_yield`, found `Literal[1]`
- src/trio/_core/_tests/test_run.py:2503:27: error[invalid-argument-type] Argument is incorrect: Expected `T@async_yield`, found `Literal[2]`
- src/trio/_tests/test_highlevel_open_unix_stream.py:28:25: error[invalid-argument-type] Argument is incorrect: Expected `CloseT@close_on_error`, found `CloseMe`
- src/trio/_tests/test_highlevel_open_unix_stream.py:33:29: error[invalid-argument-type] Argument is incorrect: Expected `CloseT@close_on_error`, found `CloseMe`
- Found 482 diagnostics
+ Found 477 diagnostics

mypy (https://github.com/python/mypy)
- mypy/subtypes.py:187:26: error[invalid-argument-type] Argument is incorrect: Expected `list[tuple[T@pop_on_exit, T@pop_on_exit]]`, found `list[tuple[Type, Type]]`
- mypy/subtypes.py:187:71: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Type`
- mypy/subtypes.py:187:77: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Type`
- mypy/subtypes.py:226:26: error[invalid-argument-type] Argument is incorrect: Expected `list[tuple[T@pop_on_exit, T@pop_on_exit]]`, found `list[tuple[Type, Type]]`
- mypy/subtypes.py:226:70: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Type`
- mypy/subtypes.py:226:76: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Type`
- mypy/subtypes.py:1212:22: error[invalid-argument-type] Argument is incorrect: Expected `list[tuple[T@pop_on_exit, T@pop_on_exit]]`, found `Unknown | list[tuple[Instance, Instance]]`
- mypy/subtypes.py:1212:32: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Instance`
- mypy/subtypes.py:1212:38: error[invalid-argument-type] Argument is incorrect: Expected `T@pop_on_exit`, found `Instance`
- Found 1739 diagnostics
+ Found 1730 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/dbg_mod/gdb/__init__.py:185:28: error[invalid-argument-type] Argument is incorrect: Expected `T@selection`, found `Unknown | Frame`
- pwndbg/dbg_mod/gdb/__init__.py:204:24: error[invalid-argument-type] Argument is incorrect: Expected `T@selection`, found `Unknown | Frame`
- pwndbg/dbg_mod/gdb/__init__.py:236:24: error[invalid-argument-type] Argument is incorrect: Expected `T@selection`, found `Unknown | Frame`
- pwndbg/dbg_mod/gdb/__init__.py:402:24: error[invalid-argument-type] Argument is incorrect: Expected `T@selection`, found `Unknown | InferiorThread`
- Found 2038 diagnostics
+ Found 2034 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:461:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:535:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:610:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:685:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:760:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/cli/commands.py:835:21: error[invalid-await] `Unknown | None | Coroutine[Any, Any, Unknown | None]` is not awaitable
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `dict[Any, Any] | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:96:21: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_block_document_references`, found `dict[str, Any] | @Todo`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | dict[Any, Any] | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:63: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `dict[str, Any]`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `bool | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `str | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, str] | list[str] | None`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:27:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool | dict[Unknown | str, Unknown | int] | None`
+ src/integrations/prefect-docker/tests/test_containers.py:42:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_containers.py:55:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_containers.py:68:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_containers.py:81:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerHost | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:16:44: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:21:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
+ src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:29:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:31:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
+ src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:51:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-docker/tests/test_images.py:53:16: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `Unknown | list[Unknown]`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `DockerRegistryCredentials | None`, found `str | bool`
+ src/integrations/prefect-docker/tests/test_images.py:64:47: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str | bool`
+ src/integrations/prefect-kubernetes/prefect_kubernetes/jobs.py:429:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `str`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:20:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:29:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:38:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `None`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:57:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:103:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:149:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:195:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:240:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:286:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_custom_objects.py:344:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:18:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:38:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:70:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:92:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:113:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_deployments.py:141:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:36:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:52:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:68:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:87:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:107:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:131:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_jobs.py:159:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:29:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:46:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:78:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:96:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:115:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:137:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
+ src/integrations/prefect-kubernetes/tests/test_pods.py:167:9: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `Literal["test"]`
- src/prefect/_internal/concurrency/calls.py:341:41: error[invalid-argument-type] Argument to bound method `run` is incorrect: Expected `Generator[Any, None, _T@create_task] | Coroutine[Any, Any, _T@create_task]`, found `CoroutineType[Any, Any, None]`
- src/prefect/_internal/concurrency/calls.py:355:41: error[invalid-argument-type] Argument to bound method `run` is incorrect: Expected `Coroutine[Any, Any, _T@run]`, found `CoroutineType[Any, Any, None]`
- src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/cache_policies.py:311:25: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `dict[Any, Any] | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | dict[Any, Any] | float | ... omitted 3 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:61: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_block_document_references`, found `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:45: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Argument type `dict[Any, Any] | int | dict[str, Any] | ... omitted 4 union elements` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/deployments/steps/core.py:136:54: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_block_document_references`, found `dict[Any, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/flow_runs.py:232:12: error[invalid-return-type] Return type does not match returned value: expected `T@pause_flow_run | None`, found `T@_in_process_pause | None`
- src/prefect/flow_runs.py:236:9: error[invalid-argument-type] Argument is incorrect: Expected `type[T@_in_process_pause] | None`, found `type[T@pause_flow_run] | None`
- src/prefect/task_engine.py:777:25: error[invalid-argument-type] Argument is incorrect: Expected `Task[P@update_for_task, R@update_for_task]`, found `Task[P@SyncTaskRunEngine, R@SyncTaskRunEngine] | Task[P@SyncTaskRunEngine, Coroutine[Any, Any, R@SyncTaskRunEngine]]`
- src/prefect/task_engine.py:1383:25: error[invalid-argument-type] Argument is incorrect: Expected `Task[P@update_for_task, R@update_for_task]`, found `Task[P@AsyncTaskRunEngine, R@AsyncTaskRunEngine] | Task[P@AsyncTaskRunEngine, Coroutine[Any, Any, R@AsyncTaskRunEngine]]`
+ src/prefect/task_engine.py:1613:28: error[invalid-await] `Unknown | R@AsyncTaskRunEngine | Coroutine[Any, Any, R@AsyncTaskRunEngine]` is not awaitable
+ src/prefect/task_engine.py:1721:47: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_task_sync`
+ src/prefect/task_engine.py:1734:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_sync`
+ src/prefect/task_engine.py:1780:48: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_task_async`
+ src/prefect/task_engine.py:1792:29: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_task_async`
- src/prefect/task_runners.py:398:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(**_P@run) -> _T@run`, found `def run[_T](main: Coroutine[Any, Any, _T@run], *, debug: bool | None = None) -> _T@run`
- src/prefect/task_runners.py:399:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `_P@run.args`, found `CoroutineType[Any, Any, Unknown | State[Any] | None]`
- src/prefect/task_runners.py:404:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(**_P@run) -> _T@run`, found `def run_task_sync[**P, R](task: Task[P@run_task_sync, R@run_task_sync], task_run_id: UUID | None = None, task_run: TaskRun | None = None, parameters: dict[str, Any] | None = None, wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, return_type: Literal["state", "result"] = "result", dependencies: dict[str, set[RunInput]] | None = None, context: dict[str, Any] | None = None) -> R@run_task_sync | State[Any] | None`
- src/prefect/task_runners.py:803:13: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `Task[P@_resolve_futures_and_submit, R@_resolve_futures_and_submit | CoroutineType[Any, Any, R@_resolve_futures_and_submit]]`, found `Task[P@submit, R@submit | CoroutineType[Any, Any, R@submit]]`
- src/prefect/task_worker.py:370:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `(**_P@run) -> _T@run`, found `def run_task_sync[**P, R](task: Task[P@run_task_sync, R@run_task_sync], task_run_id: UUID | None = None, task_run: TaskRun | None = None, parameters: dict[str, Any] | None = None, wait_for: PrefectFuture[Any] | Any | Iterable[PrefectFuture[Any] | Any] | None = None, return_type: Literal["state", "result"] = "result", dependencies: dict[str, set[RunInput]] | None = None, context: dict[str, Any] | None = None) -> R@run_task_sync | State[Any] | None`
- src/prefect/task_worker.py:372:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `_P@run.kwargs`, found `UUID`
- src/prefect/task_worker.py:373:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `_P@run.kwargs`, found `TaskRun`
- src/prefect/task_worker.py:374:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `_P@run.kwargs`, found `dict[Unknown, Unknown] | Any`
- src/prefect/task_worker.py:375:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `_P@run.kwargs`, found `list[Unknown] | Any`
- src/prefect/task_worker.py:376:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `_P@run.kwargs`, found `Literal["state"]`
- src/prefect/task_worker.py:377:17: error[invalid-argument-type] Argument to bound method `submit` is incorrect: Expected `_P@run.kwargs`, found `None | Any`
- src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/tasks.py:184:9: warning[possibly-missing-attribute] Attribute `__code__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/tasks.py:922:35: error[invalid-argument-type] Argument is incorrect: Expected `Task[P@update_for_task, R@update_for_task]`, found `Self@create_run`
- src/prefect/tasks.py:1027:35: error[invalid-argument-type] Argument is incorrect: Expected `Task[P@update_for_task, R@update_for_task]`, found `Self@create_local_run`
- src/prefect/tasks.py:1822:21: error[invalid-argument-type] Argument is incorrect: Expected `Task[P@serve, R@serve]`, found `Self@serve`
- src/prefect/tasks.py:1822:21: error[invalid-argument-type] Argument is incorrect: Expected `Task[P@serve, R@serve]`, found `Self@serve`
- src/prefect/utilities/templating.py:318:17: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_block_document_references`, found `object`
+ src/prefect/utilities/templating.py:318:17: error[invalid-argument-type] Argument is incorrect: Argument type `object` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `Unknown | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[Unknown | int | dict[str, Any] | ... omitted 4 union elements]`
- src/prefect/utilities/templating.py:325:17: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_block_document_references`, found `object`
+ src/prefect/utilities/templating.py:325:17: error[invalid-argument-type] Argument is incorrect: Argument type `object` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | Unknown | float | ... omitted 4 union elements]`
- src/prefect/utilities/templating.py:438:42: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `object`
+ src/prefect/utilities/templating.py:438:42: error[invalid-argument-type] Argument is incorrect: Argument type `object` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | Unknown | float | ... omitted 4 union elements]`
- src/prefect/utilities/templating.py:442:41: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `object`
+ src/prefect/utilities/templating.py:442:41: error[invalid-argument-type] Argument is incorrect: Argument type `object` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Argument type `str | int | dict[str, Any] | ... omitted 3 union elements` does not satisfy constraints (`str`, `int`, `int | float`, `bool`, `dict[Any, Any]`, `list[Any]`, `None`) of type variable `T`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | Unknown | float | ... omitted 4 union elements`
- Found 5298 diagnostics
+ Found 5346 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`

zulip (https://github.com/zulip/zulip)
- zerver/data_import/slack.py:1794:13: error[invalid-argument-type] Argument to function `convert_slack_workspace_messages` is incorrect: Expected `(UploadFileRequest, /) -> None`, found `(ParallelRecordType@run_parallel_queue, /) -> None`
- zerver/lib/export.py:2288:9: error[invalid-argument-type] Argument is incorrect: Expected `(ParallelRecordType@run_parallel_queue, /) -> None`, found `def _save_s3_key_to_file(key_name: str) -> None`
- zerver/lib/export.py:2299:65: error[invalid-argument-type] Argument to function `iterate_attachments` is incorrect: Expected `(str, /) -> Any`, found `(ParallelRecordType@run_parallel_queue, /) -> None`
- zerver/lib/parallel.py:54:9: error[invalid-argument-type] Argument is incorrect: Expected `(ParallelRecordType@run_parallel_queue, /) -> None`, found `(ParallelRecordType@run_parallel, /) -> None`
- zerver/lib/parallel.py:63:20: error[invalid-argument-type] Argument is incorrect: Expected `ParallelRecordType@run_parallel_queue`, found `ParallelRecordType@run_parallel`
- zerver/tests/test_parallel.py:135:21: error[invalid-argument-type] Argument is incorrect: Expected `ParallelRecordType@run_parallel_queue`, found `Literal[100]`
- zerver/tests/test_parallel.py:143:21: error[invalid-argument-type] Argument is incorrect: Expected `ParallelRecordType@run_parallel_queue`, found `Literal[101]`
- zerver/tests/test_parallel.py:144:21: error[invalid-argument-type] Argument is incorrect: Expected `ParallelRecordType@run_parallel_queue`, found `Literal[102]`
- Found 3677 diagnostics
+ Found 3669 diagnostics


```

</details>


No memory usage changes detected ✅



---
