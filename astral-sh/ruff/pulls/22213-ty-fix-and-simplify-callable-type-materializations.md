```yaml
number: 22213
title: "[ty] fix and simplify callable type materializations"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/top-callable
created_at: 2025-12-26T21:22:51Z
updated_at: 2025-12-27T18:45:09Z
url: https://github.com/astral-sh/ruff/pull/22213
synced_at: 2026-01-12T15:57:44Z
```

# [ty] fix and simplify callable type materializations

---

_@carljm_

## Summary

A couple things I noticed when taking another look at the callable type materializations.

1) Previously we wrongly ignored the return type when bottom-materializing a callable with gradual signature, and always changed it to `Never`.
2) We weren't correctly handling overloads that included a gradual signature. Rather than separately materializing each overload, we would just mark the entire callable as "top" or replace the entire callable with the bottom signature.

Really, "top parameters" is something that belongs on the `Parameters`, not on the entire `CallableType`. Conveniently, we already have `ParametersKind` where we can track this, right next to where we already track `ParametersKind::Gradual`. This saves a bit of memory, fixes the two bugs above, and simplifies the implementation considerably (net removal of 100+ LOC, a bunch of places that shouldn't need to care about topness of a callable no longer need to.)

One user-visible change from this is that I now display the "top callable" as `(Top[...]) -> object` instead of `Top[(...) -> object]`. I think this is a (minor) improvement, because it wraps exactly the part in `Top` that needs to be, rather than misleadingly wrapping the entire callable type, including the return type (which has already been separately materialized). I think the prior display would be particularly confusing if the return type also has its own `Top` in it: previously we could have e.g. `Top[(...) -> Top[list[Unknown]]]`, which I think is less clear than the new `(Top[...]) -> Top[list[Unknown]]`.

## Test Plan

Added mdtests that failed before this PR and pass after it.

### Ecosystem

The changed diagnostics are all either the change to `Top` display, or else known non-deterministic output. The added diagnostics are all true positives:

The added diagnostic at https://github.com/pytorch/vision/blob/aa35ca1965bea39b9a0996d5d2d7f15d325e54d2/torchvision/transforms/v2/_utils.py#L149 is a true positive that wasn't caught by the previous version. `str` is not assignable to `Callable[[Any], Any]` (strings are not callable), nor is the top callable (top callable includes callables that do not take a single required positional argument.)

The added diagnostic at https://github.com/Kludex/starlette/blob/081535ad9b46a29ca4d9beea9dea9d422b4c8f7d/starlette/routing.py#L67 is also a (pedantic) true positive. It's the same case as #1567 -- the code assumes that it is impossible for a subclass of `Response` to implement `__await__` (yielding something other than a `Response`).

The pytest added diagnostics are also both similar true positives: they make the assumption that an object cannot simultaneously be a `Sequence` and callable, or an `Iterable` and callable.


---

_Label `ty` added by @carljm on 2025-12-26 21:22_

---

_Comment by @astral-sh-bot[bot] on 2025-12-26 21:24_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-26 21:25_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `((Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]) & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/fixtures.py:1381:9: error[invalid-argument-type] Argument is incorrect: Expected `tuple[object, ...] | ((Any, /) -> object) | None`, found `None | (Sequence[object] & Top[(...) -> object]) | ((Any, /) -> object) | tuple[object, ...]`
+ src/_pytest/python.py:1439:13: error[invalid-argument-type] Argument is incorrect: Expected `((Any, /) -> object) | None`, found `None | (Iterable[object] & Top[(...) -> object]) | ((Any, /) -> object)`
- Found 427 diagnostics
+ Found 429 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/routing.py:67:51: error[invalid-assignment] Object of type `(((Request, /) -> Awaitable[Response] | Response) & Top[(...) -> Awaitable[object]]) | partial[Unknown]` is not assignable to `(Request, /) -> Awaitable[Response]`
- Found 217 diagnostics
+ Found 218 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_injection.py:298:9: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]]` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:298:9: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(...), object] & ~Top[classmethod[Unknown, (...), object]]` has no attribute `__qualname__`
- src/antidote/core/_injection.py:298:30: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]]` has no attribute `__name__`
+ src/antidote/core/_injection.py:298:30: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(...), object] & ~Top[classmethod[Unknown, (...), object]]` has no attribute `__name__`
- src/antidote/core/_injection.py:302:17: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]] & ~MethodType` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:302:17: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(...), object] & ~Top[classmethod[Unknown, (...), object]] & ~MethodType` has no attribute `__qualname__`
- src/antidote/core/_injection.py:302:42: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]] & ~MethodType` has no attribute `__name__`
+ src/antidote/core/_injection.py:302:42: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(...), object] & ~Top[classmethod[Unknown, (...), object]] & ~MethodType` has no attribute `__name__`

vision (https://github.com/pytorch/vision)
+ torchvision/transforms/v2/_utils.py:149:16: error[invalid-return-type] Return type does not match returned value: expected `(Any, /) -> Any`, found `(str & Top[(...) -> object]) | ((Any, /) -> Any)`
- Found 1412 diagnostics
+ Found 1413 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/commands.py:2477:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__discord_app_commands_checks__` on type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>)`
+ discord/app_commands/commands.py:2477:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__discord_app_commands_checks__` on type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>)`
- discord/app_commands/commands.py:2479:13: error[unresolved-attribute] Object of type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu)` has no attribute `__discord_app_commands_checks__`
+ discord/app_commands/commands.py:2479:13: error[unresolved-attribute] Object of type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~ContextMenu)` has no attribute `__discord_app_commands_checks__`
- discord/ext/commands/core.py:1942:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:1942:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:1944:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:1944:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2365:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:2365:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:2367:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:2367:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2368:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_guild_only__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2368:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_guild_only__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
- discord/ext/commands/core.py:2440:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:2440:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:2442:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:2442:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2443:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_is_nsfw__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2443:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_is_nsfw__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
- discord/ext/commands/core.py:2499:13: error[invalid-assignment] Object of type `CooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2499:13: error[invalid-assignment] Object of type `CooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
- discord/ext/commands/core.py:2547:13: error[invalid-assignment] Object of type `DynamicCooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2547:13: error[invalid-assignment] Object of type `DynamicCooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
- discord/ext/commands/core.py:2582:13: error[invalid-assignment] Object of type `MaxConcurrency` is not assignable to attribute `__commands_max_concurrency__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2582:13: error[invalid-assignment] Object of type `MaxConcurrency` is not assignable to attribute `__commands_max_concurrency__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
- discord/ext/commands/core.py:2634:13: error[invalid-assignment] Object of type `((CogT@before_invoke, ContextT@before_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@before_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__before_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2634:13: error[invalid-assignment] Object of type `((CogT@before_invoke, ContextT@before_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@before_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__before_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`
- discord/ext/commands/core.py:2657:13: error[invalid-assignment] Object of type `((CogT@after_invoke, ContextT@after_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@after_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__after_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
+ discord/ext/commands/core.py:2657:13: error[invalid-assignment] Object of type `((CogT@after_invoke, ContextT@after_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@after_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__after_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (...), Unknown]]`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:228:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:230:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/tests/expr/test_table.py:1936:9: error[invalid-argument-type] Argument to bound method `pivot_longer` is incorrect: Expected `((str, /) -> Value) | Mapping[str, (str, /) -> Value] | None`, found `dict[str, (Overload[(self: LiteralString) -> str, (self) -> str]) | <class 'int'>]`
+ ibis/tests/expr/test_table.py:1936:9: error[invalid-argument-type] Argument to bound method `pivot_longer` is incorrect: Expected `((str, /) -> Value) | Mapping[str, (str, /) -> Value] | None`, found `dict[str, Overload[(self: LiteralString) -> str, (self) -> str] | <class 'int'>]`
- ibis/tests/expr/test_table.py:1959:13: error[invalid-argument-type] Argument to bound method `pivot_longer` is incorrect: Expected `((str, /) -> Value) | Mapping[str, (str, /) -> Value] | None`, found `dict[str, (Overload[(self: LiteralString) -> str, (self) -> str]) | <class 'int'>]`
+ ibis/tests/expr/test_table.py:1959:13: error[invalid-argument-type] Argument to bound method `pivot_longer` is incorrect: Expected `((str, /) -> Value) | Mapping[str, (str, /) -> Value] | None`, found `dict[str, Overload[(self: LiteralString) -> str, (self) -> str] | <class 'int'>]`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:300:17: warning[possibly-missing-attribute] Attribute `dtype` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:301:17: warning[possibly-missing-attribute] Attribute `shape` may be missing on object of type `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`
- sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...] | slice[Any, Any, Any] | EllipsisType`
+ sklearn/externals/array_api_extra/_lib/_at.py:308:25: error[invalid-argument-type] Argument to function `apply_where` is incorrect: Expected `Array`, found `int | slice[Any, Any, Any] | EllipsisType | Array | tuple[int | slice[Any, Any, Any] | EllipsisType | Array, ...]`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Yarn[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, generic[object]]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Top[Index[Any]] | TypeBlocks | Top[Bus[Any]] | ... omitted 6 union elements, generic[object]]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/frame/test_groupby.py:228:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:624:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 5093 diagnostics
+ Found 5090 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress) | None`, found `A@BaseDecoderTools | None`
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[(A@BaseDecoderTools & BTCAddress) | (A@BaseDecoderTools & ChecksumAddress) | (A@BaseDecoderTools & SubstrateAddress) | (A@BaseDecoderTools & SolanaAddress)]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- Found 2101 diagnostics
+ Found 2099 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/config_entries.py:3930:33: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str]) | (Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str])` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
+ homeassistant/config_entries.py:3930:33: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str] | Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str]` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
- homeassistant/config_entries.py:3931:12: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str]) | (Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str])` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
+ homeassistant/config_entries.py:3931:12: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str] | Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str]` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
- homeassistant/helpers/device_registry.py:1867:36: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str]) | (Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str])` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`
+ homeassistant/helpers/device_registry.py:1867:36: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: Literal["action"], /) -> Literal["create", "remove"], (key: Literal["entity_id"], /) -> str] | Overload[(key: Literal["action"], /) -> Literal["update"], (key: Literal["entity_id"], /) -> str, (key: Literal["changes"], /) -> dict[str, Any], (key: Literal["old_entity_id"], /) -> str]` cannot be called with key of type `Literal["changes"]` on object of type `EventEntityRegistryUpdatedData`


```

</details>


No memory usage changes detected ✅



---

_@carljm reviewed on 2025-12-26 21:26_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1625 on 2025-12-26 21:26_

I intentionally decided not to preserve this comment in the new location for this check. It's redundant with the existing comment on `BindingError::CalledTopCallable` itself (not to mention the diagnostic we emit for it), and those are both places you would very quickly find if you were curious about this check.

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-26 21:37_

---

_Comment by @astral-sh-bot[bot] on 2025-12-26 21:42_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-assignment` | 1 | 0 | 16 |
| `invalid-argument-type` | 2 | 3 | 10 |
| `invalid-return-type` | 1 | 1 | 9 |
| `unresolved-attribute` | 0 | 0 | 8 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **5** | **4** | **43** |


**[Full report with detailed diff](https://8bc1847f.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://8bc1847f.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @AlexWaygood on 2025-12-26 21:45_

> One user-visible change from this is that I now display the "top callable" as `(Top[...]) -> object` instead of `Top[(...) -> object]`.

But the top materialization of a `Callable` uses the bottom materialization of a `Parameters`. So shouldn't `Top[(...) -> object]` be equivalent to `Bottom[...] -> object`?

I think this is an argument in favour of our current display, actually. It means you don't have to think about contravariance.

---

_Comment by @carljm on 2025-12-26 21:54_

> But the top materialization of a `Callable` uses the bottom materialization of a `Parameters`. So shouldn't `Top[(...) -> object]` be equivalent to `Bottom[...] -> object`?

I think it's arguable and a bit arbitrary whether you consider the entire `Parameters` to be in contravariant position, or the types of individual parameters. The semantics of how arity mismatches work with parameter subtyping is not arbitrary, of course, but the way we label it is. So I don't think `(Top[...]) -> object` is wrong, and I made that (arbitrary) choice so that you wouldn't have to think about contravariance.

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-12-26 23:17_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-26 23:17_

---

_Comment by @AlexWaygood on 2025-12-26 23:31_

The ecosystem-analyzer workflow reruns automatically on new pushes to a labelled PR following https://github.com/astral-sh/ruff/commit/19b10993e1bfcdcb0be8fad8b4259a0f504860c6; there's no need to remove and re-add the label anymore to trigger a rerun :-)

---

_Marked ready for review by @carljm on 2025-12-26 23:51_

---

_Review requested from @AlexWaygood by @carljm on 2025-12-26 23:51_

---

_Review requested from @sharkdp by @carljm on 2025-12-26 23:51_

---

_Review requested from @dcreager by @carljm on 2025-12-26 23:51_

---

_Comment by @carljm on 2025-12-26 23:53_

@AlexWaygood I don't feel strongly about the display -- I think it's a bit odd (and more visually confusing) to wrap extra stuff in `Top` that will always already be fully static, but it's not _wrong_. And I see the possibility that someone who knows too much about variance could be confused by what `Top` means inside a parameter list. So if you still think the old display is preferable, I don't think it'd be much work to restore it.

---

_Review requested from @charliermarsh by @carljm on 2025-12-26 23:53_

---

_@AlexWaygood reviewed on 2025-12-27 11:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:1921 on 2025-12-27 11:51_

It's very tempting, looking at this, to think "Ah, but surely the top-materialization of a `Callable` parameters can also be simplified, to `(*args: Never, **kwargs: Never)`, the inverse of the bottom-materialization of a `Callable` parameters directly below? But no, that's not the case, because those parameters would still permit calls that pass 0 arguments in -- it's actually "the same" as the empty parameters list `()`. It might be worth adding a comment here explicitly laying that out?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:1341 on 2025-12-27 12:04_

no tests fail if I remove this, and I think the logic would be taken care of by the generalised fall-through below, so I think it's just a pure optimisation? But codspeed isn't reporting any speedup on this PR, so I'd be inclined to remove it, to keep the code simple (this method is already very complex)

```suggestion
        // The top signature is supertype of (and assignable from) all other signatures.
        if other.parameters.is_top() {
            return ConstraintSet::from(true);
        }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/materialization.md`:73 on 2025-12-27 12:09_

I don't think this is true? `Bottom[list[Any]]` should simplify to `Never`, I think. But obviously this is a pre-existing issue.

---

_@charliermarsh approved on 2025-12-27 13:16_

---

_@AlexWaygood reviewed on 2025-12-27 14:17_

---

_Comment by @AlexWaygood on 2025-12-27 15:31_

> @AlexWaygood I don't feel strongly about the display -- I think it's a bit odd (and more visually confusing) to wrap extra stuff in `Top` that will always already be fully static, but it's not _wrong_. And I see the possibility that someone who knows too much about variance could be confused by what `Top` means inside a parameter list. So if you still think the old display is preferable, I don't think it'd be much work to restore it.

I do still prefer the current display, I think. I totally see your point that the top materialisation of the return type can always be (and is always!) fully simplified, so it doesn't "need" to be wrapped in `Top`. But (at somebody who knows too much about variance myself), I still find it a bit confusing personally.

I also find the situation with the parentheses in the current display to be a little confusing. `Top[(...)] -> object` would make more sense to me than `(Top[...]) -> object` — the parentheses in the latter look redundant to me, and also seem too similar to our display for a parameters set that takes only a single parameter. But here again, I feel like we can just sidestep that issue entirely by sticking with our current display ;)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:195 on 2025-12-27 15:38_

I worry that this PR makes the `Parameters::new()` method a bit of a footgun. You have to remember never to use it when you're constructing a new `Parameters` from an existing `Parameters`, because the existing `Parameters` might be a top materialisation. Iterating over the existing parameters and passing them to `Parameters::new()` will discard that information — so maybe `Parameters::new()` should take an additional `parameters_kind` parameter, to force us to consider that explicitly every time we construct a new `Parameters`?

It looks like this issue might already exist for gradual `Parameters`, though.

---

_@AlexWaygood approved on 2025-12-27 15:38_

Great catch with the overloads issue!

---

_@carljm reviewed on 2025-12-27 17:04_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_properties/materialization.md`:73 on 2025-12-27 17:04_

It's more complex than that -- we don't actually want to simplify `Bottom[list[Any]]` to `Never` because that has other undesirable consequences. But I'll add some nuance to the wording here.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:1341 on 2025-12-27 17:35_

This condition is necessary for correctness, though only in an edge case (and apparently one we're lacking test coverage of). Without it, we will wrongly consider a top callable to be a subtype of `(*object, **object) -> object` -- this is a side effect of how we give the top callable the parameters `(*object, **object)` in order to avoid redundant arity-checking errors when calling it.

I'll add a test case that fails without this.

---

_@carljm reviewed on 2025-12-27 17:35_

---

_Comment by @carljm on 2025-12-27 18:15_

I restored the old display of top callables. I also noticed that we were needlessly parenthesizing overloaded callables (they are wrapped in `Overload[]`, so by the same reasoning that we don't parenthesize `Top[]`, we don't need to parenthesize overloads either), so I fixed that as well in passing.

---

_@carljm reviewed on 2025-12-27 18:29_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:195 on 2025-12-27 18:29_

The problem doesn't exist for `Gradual`, because gradual-parameters can be (and is) automatically determined from the parameters (if they are `(*Any, **Any)`, that's the gradual parameters).
 
This also makes it awkward to add a `kind` parameter here, since it would be redundant information in every case except `Top`.

We could add an `is_top` parameter, but that is also awkward, because if it's `true` the provided parameters iterator would be ignored.

I think the most natural API is to provide the existing `new()`, `bottom()`, and `top()` constructors, with their current signatures. Though we could rename `new()` to `from_parameters()` to make it seem a bit less general?

I don't see a great solution to avoid the potential for mistakes here -- it's just the case that a `Parameters` is no longer fully determined by its list of `parameters`, there's no way around us having to understand that. I don't think that "constructing a parameters from an existing parameters" is going to be a super common task.

Open to suggestions here.

---

_@carljm reviewed on 2025-12-27 18:45_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:195 on 2025-12-27 18:45_

Will go ahead and merge this as-is, open to follow-up.

---

_Merged by @carljm on 2025-12-27 18:45_

---

_Closed by @carljm on 2025-12-27 18:45_

---

_Branch deleted on 2025-12-27 18:45_

---
