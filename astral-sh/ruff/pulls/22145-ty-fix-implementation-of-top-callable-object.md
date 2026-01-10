```yaml
number: 22145
title: "[ty] Fix implementation of `Top[Callable[..., object]]`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: charlie/x
created_at: 2025-12-22T18:05:14Z
updated_at: 2025-12-24T17:49:11Z
url: https://github.com/astral-sh/ruff/pull/22145
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Fix implementation of `Top[Callable[..., object]]`

---

_Pull request opened by @charliermarsh on 2025-12-22 18:05_

## Summary

Add a proper representation for the `Callable` top type, and use it to get `callable()` narrowing right.

Closes https://github.com/astral-sh/ty/issues/1426.

---

_Comment by @astral-sh-bot[bot] on 2025-12-22 18:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-22 18:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- mypy_primer/git_utils.py:66:56: error[invalid-argument-type] Argument to function `get_revision_for_revision_or_date` is incorrect: Expected `str`, found `@Todo | (str & ~(() -> object)) | (((Path, /) -> Awaitable[str]) & ~(() -> object))`
- Found 4 diagnostics
+ Found 3 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/llnl/util/filesystem.py:330:21: warning[possibly-missing-attribute] Attribute `replace` may be missing on object of type `(str & ~(() -> object)) | (((Match[Unknown], /) -> str) & ~(() -> object))`
- lib/spack/spack/vendor/jinja2/runtime.py:734:38: error[invalid-assignment] Object of type `(Unknown & ~(() -> object)) | bool | (((str | None, /) -> bool) & ~(() -> object))` is not assignable to `bool | None`
- lib/spack/spack/vendor/jinja2/runtime.py:814:40: error[invalid-argument-type] Argument to bound method `_invoke` is incorrect: Expected `bool`, found `@Todo | bool | None`
- Found 4295 diagnostics
+ Found 4292 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/resolvelib/structs.py:203:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `() -> Iterable[Unknown]`, found `(Iterable[CT@build_iter_view] & (() -> object)) | (() -> Iterable[CT@build_iter_view])`
+ src/pip/_vendor/resolvelib/structs.py:203:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `() -> Iterable[Unknown]`, found `(Iterable[CT@build_iter_view] & Top[(...) -> object]) | (() -> Iterable[CT@build_iter_view])`
- src/pip/_vendor/rich/_log_render.py:62:59: error[invalid-argument-type] Argument to bound method `strftime` is incorrect: Expected `str`, found `(str & ~(() -> object)) | (((datetime, /) -> Text) & ~(() -> object)) | (Unknown & ~(() -> object))`
- src/pip/_vendor/rich/text.py:624:51: error[invalid-argument-type] Argument is incorrect: Expected `str | Style`, found `(@Todo & ~None) | (((str, /) -> str | Style | None) & ~AlwaysFalsy & ~(() -> object)) | (str & ~AlwaysFalsy & ~(() -> object)) | (Style & ~AlwaysFalsy & ~(() -> object))`
- Found 612 diagnostics
+ Found 610 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/legacy/server.py:634:37: error[invalid-argument-type] Argument to bound method `update` is incorrect: Expected `Mapping[str, str] | Iterable[tuple[str, str]] | SupportsKeysAndGetItem`, found `(Mapping[str, str] & ~(() -> object)) | (Iterable[tuple[str, str]] & ~(() -> object)) | (SupportsKeysAndGetItem & ~(() -> object)) | (((str, Headers, /) -> Mapping[str, str] | Iterable[tuple[str, str]] | SupportsKeysAndGetItem) & ~(() -> object)) | (@Todo & ~None)`
- Found 44 diagnostics
+ Found 43 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/utils.py:506:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/werkzeug/utils.py:501:12: error[unsupported-operator] Operator `>` is not supported between objects of type `(int & ~(() -> object)) | (((str | None, /) -> int | None) & ~(() -> object)) | (@Todo & ~None)` and `Literal[0]`
- src/werkzeug/utils.py:505:9: error[invalid-assignment] Object of type `(int & ~(() -> object)) | (((str | None, /) -> int | None) & ~(() -> object)) | (@Todo & ~None)` is not assignable to attribute `max_age` of type `int | None`
- src/werkzeug/wsgi.py:244:25: error[invalid-assignment] Object of type `list[Unknown | (() -> None) | (Iterable[() -> None] & (() -> object))]` is not assignable to `None | (() -> None) | Iterable[() -> None]`
+ src/werkzeug/wsgi.py:244:25: error[invalid-assignment] Object of type `list[Unknown | (() -> None) | (Iterable[() -> None] & Top[(...) -> object])]` is not assignable to `None | (() -> None) | Iterable[() -> None]`
- Found 387 diagnostics
+ Found 386 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_check/metadata/metadecor.py:555:34: error[unresolved-attribute] Object of type `((...) -> Unknown) & (() -> object)` has no attribute `__name__`
+ beartype/_check/metadata/metadecor.py:555:34: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__name__`
- beartype/_util/func/utilfuncscope.py:261:29: error[unresolved-attribute] Object of type `((...) -> Unknown) & (() -> object)` has no attribute `__name__`
+ beartype/_util/func/utilfuncscope.py:261:29: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__name__`
- beartype/_util/func/utilfuncscope.py:320:16: error[unresolved-attribute] Object of type `((...) -> Unknown) & (() -> object)` has no attribute `__qualname__`
+ beartype/_util/func/utilfuncscope.py:320:16: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__qualname__`
- beartype/_util/func/utilfuncscope.py:334:16: error[unresolved-attribute] Object of type `((...) -> Unknown) & (() -> object)` has no attribute `__qualname__`
+ beartype/_util/func/utilfuncscope.py:334:16: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__qualname__`
- beartype/_util/func/utilfunctest.py:914:16: error[unresolved-attribute] Object of type `((...) -> Unknown) & ~(() -> object)` has no attribute `__qualname__`
- beartype/typing/_typingcache.py:87:34: error[unresolved-attribute] Object of type `((...) -> Unknown) & (() -> object)` has no attribute `__name__`
+ beartype/typing/_typingcache.py:87:34: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__name__`
- Found 495 diagnostics
+ Found 494 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/runtime.py:690:38: error[invalid-assignment] Object of type `(Unknown & ~(() -> object)) | bool | (((str | None, /) -> bool) & ~(() -> object))` is not assignable to `bool | None`
- src/jinja2/runtime.py:770:40: error[invalid-argument-type] Argument to bound method `_invoke` is incorrect: Expected `bool`, found `@Todo | bool | None`
- Found 182 diagnostics
+ Found 180 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/core/spidermw.py:138:50: error[invalid-argument-type] Argument to bound method `_check_mw_method_spider_arg` is incorrect: Expected `(...) -> Unknown`, found `object`
- tests/test_utils_console.py:27:12: error[unresolved-attribute] Object of type `((...) -> None) & (() -> object)` has no attribute `__name__`
+ tests/test_utils_console.py:27:12: error[unresolved-attribute] Object of type `(...) -> None` has no attribute `__name__`
- tests/test_utils_console.py:34:12: error[unresolved-attribute] Object of type `((...) -> None) & (() -> object)` has no attribute `__name__`
+ tests/test_utils_console.py:34:12: error[unresolved-attribute] Object of type `(...) -> None` has no attribute `__name__`
- Found 1792 diagnostics
+ Found 1791 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_core/intents/registries.py:471:16: error[missing-argument] No arguments provided for required parameters 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17
+ kopf/_core/intents/registries.py:509:42: error[missing-argument] No arguments provided for required parameters 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17
+ kopf/_core/intents/registries.py:539:36: error[missing-argument] No arguments provided for required parameters 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17
+ kopf/_core/intents/registries.py:545:36: error[missing-argument] No arguments provided for required parameters 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17
- Found 263 diagnostics
+ Found 267 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:557:43: error[invalid-argument-type] Argument to bound method `_check_scope` is incorrect: Expected `Scope`, found `Scope | (((str, Config, /) -> Literal["session", "package", "module", "class", "function"]) & ~(() -> object) & ~str)`
- src/_pytest/fixtures.py:1003:17: error[invalid-argument-type] Argument to bound method `from_user` is incorrect: Expected `Literal["session", "package", "module", "class", "function"]`, found `Literal["session", "package", "module", "class", "function"] | (((str, Config, /) -> Literal["session", "package", "module", "class", "function"]) & str & ~(() -> object))`
- src/_pytest/fixtures.py:1025:16: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Scope | (((str, Config, /) -> Literal["session", "package", "module", "class", "function"]) & ~(() -> object) & ~str)`
- src/_pytest/fixtures.py:1381:9: error[invalid-argument-type] Argument is incorrect: Expected `tuple[object, ...] | ((Any, /) -> object) | None`, found `None | (Sequence[object] & (() -> object)) | (((Any, /) -> object) & (() -> object)) | Unknown`
- src/_pytest/fixtures.py:1381:70: error[invalid-argument-type] Argument to class `tuple` is incorrect: Expected `Iterable[object]`, found `(Sequence[object] & ~(() -> object)) | (((Any, /) -> object) & ~(() -> object))`
- src/_pytest/fixtures.py:1724:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (((str, Config, /) -> Literal["session", "package", "module", "class", "function"]) & ~(() -> object) & ~str)`
- src/_pytest/python.py:1435:39: error[invalid-argument-type] Argument to bound method `_validate_ids` is incorrect: Expected `Iterable[object]`, found `(Iterable[object] & ~(() -> object)) | (((Any, /) -> object) & ~(() -> object))`
- src/_pytest/python.py:1439:13: error[invalid-argument-type] Argument is incorrect: Expected `((Any, /) -> object) | None`, found `None | (Iterable[object] & (() -> object)) | (((Any, /) -> object) & (() -> object))`
- src/_pytest/python.py:1534:20: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `Scope | (((str, Config, /) -> str) & ~(() -> object) & ~str) | Unknown` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 436 diagnostics
+ Found 427 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/_exception_handler.py:59:42: error[invalid-argument-type] Argument is incorrect: Expected `WebSocket`, found `Request | WebSocket`
+ starlette/middleware/errors.py:176:51: error[invalid-argument-type] Argument is incorrect: Expected `WebSocket`, found `Request`
+ starlette/middleware/errors.py:181:23: error[call-non-callable] Object of type `None` is not callable
- starlette/routing.py:67:51: error[invalid-assignment] Object of type `(((Request, /) -> Awaitable[Response] | Response) & (() -> Awaitable[object])) | partial[Unknown]` is not assignable to `(Request, /) -> Awaitable[Response]`
- Found 215 diagnostics
+ Found 217 diagnostics

ignite (https://github.com/pytorch/ignite)
- ignite/handlers/base_logger.py:52:29: error[not-iterable] Object of type `(list[str] & ~(() -> object)) | (((str, Unknown, /) -> bool) & ~(() -> object))` may not be iterable
- ignite/handlers/clearml_logger.py:502:46: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((Unknown, /) -> int | float | Unknown) & (() -> object))`
+ ignite/handlers/clearml_logger.py:502:46: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((Unknown, /) -> int | float | Unknown)`
- ignite/handlers/clearml_logger.py:691:44: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((Unknown, /) -> int | float | Unknown) & (() -> object))`
+ ignite/handlers/clearml_logger.py:691:44: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((Unknown, /) -> int | float | Unknown)`
- ignite/handlers/neptune_logger.py:505:42: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((Unknown, /) -> int | float | Unknown) & (() -> object))`
+ ignite/handlers/neptune_logger.py:505:42: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((Unknown, /) -> int | float | Unknown)`
- ignite/handlers/neptune_logger.py:609:40: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((Unknown, /) -> int | float | Unknown) & (() -> object))`
+ ignite/handlers/neptune_logger.py:609:40: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((Unknown, /) -> int | float | Unknown)`
- ignite/handlers/tensorboard_logger.py:477:40: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((Unknown, /) -> int | float | Unknown) & (() -> object))`
+ ignite/handlers/tensorboard_logger.py:477:40: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((Unknown, /) -> int | float | Unknown)`
- ignite/handlers/tensorboard_logger.py:641:38: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((Unknown, /) -> int | float | Unknown) & (() -> object))`
+ ignite/handlers/tensorboard_logger.py:641:38: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((Unknown, /) -> int | float | Unknown)`
- ignite/handlers/visdom_logger.py:491:40: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((Unknown, /) -> int | float | Unknown) & (() -> object))`
+ ignite/handlers/visdom_logger.py:491:40: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((Unknown, /) -> int | float | Unknown)`
- ignite/handlers/visdom_logger.py:542:38: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((Unknown, /) -> int | float | Unknown) & (() -> object))`
+ ignite/handlers/visdom_logger.py:542:38: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((Unknown, /) -> int | float | Unknown)`
- Found 2054 diagnostics
+ Found 2053 diagnostics

rich (https://github.com/Textualize/rich)
- rich/_log_render.py:62:59: error[invalid-argument-type] Argument to bound method `strftime` is incorrect: Expected `str`, found `(str & ~(() -> object)) | (((datetime, /) -> Text) & ~(() -> object)) | (Unknown & ~(() -> object))`
- rich/text.py:624:51: error[invalid-argument-type] Argument is incorrect: Expected `str | Style`, found `(@Todo & ~None) | (((str, /) -> str | Style | None) & ~AlwaysFalsy & ~(() -> object)) | (str & ~AlwaysFalsy & ~(() -> object)) | (Style & ~AlwaysFalsy & ~(() -> object))`
- Found 348 diagnostics
+ Found 346 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/pack.py:3512:13: warning[possibly-missing-attribute] Attribute `write` may be missing on object of type `(((bytes, /) -> None) & ~(() -> object)) | (((bytes | bytearray | memoryview[int], /) -> int) & ~(() -> object)) | (IO[bytes] & ~(() -> object))`
- dulwich/worktree.py:598:33: warning[possibly-missing-attribute] Attribute `encode` may be missing on object of type `(@Todo & ~None & ~bytes) | (str & ~(() -> object)) | (((Any, Commit, /) -> bytes) & ~(() -> object) & ~bytes)`
- dulwich/worktree.py:643:29: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal[b"commit: "]` and `bytes | (@Todo & ~None & ~str) | (((Any, Commit, /) -> bytes) & ~(() -> object) & ~str)`
- dulwich/worktree.py:661:29: error[unsupported-operator] Operator `+` is not supported between objects of type `Literal[b"commit: "]` and `bytes | (@Todo & ~None & ~str) | (((Any, Commit, /) -> bytes) & ~(() -> object) & ~str)`
- Found 231 diagnostics
+ Found 227 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/hooks.py:302:16: error[unresolved-attribute] Object of type `(str & (() -> object)) | (((...) -> Unknown) & (() -> object))` has no attribute `__name__`
+ src/schemathesis/hooks.py:302:16: error[unresolved-attribute] Object of type `(str & Top[(...) -> object]) | ((...) -> Unknown)` has no attribute `__name__`

tornado (https://github.com/tornadoweb/tornado)
- tornado/escape.py:336:30: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `(str & ~AlwaysFalsy & ~(() -> object)) | (((str, /) -> str) & ~AlwaysFalsy & ~(() -> object))`
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
- Found 328 diagnostics
+ Found 327 diagnostics

optuna (https://github.com/optuna/optuna)
+ tests/test_deprecated.py:56:12: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__name__`
+ tests/test_deprecated.py:72:12: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__name__`
+ tests/test_deprecated.py:192:12: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__name__`
+ tests/test_experimental.py:55:12: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__name__`
+ tests/test_experimental.py:68:12: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__name__`
+ tests/test_experimental.py:81:12: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__name__`
- Found 554 diagnostics
+ Found 560 diagnostics

vision (https://github.com/pytorch/vision)
- test/datasets_utils.py:835:52: error[invalid-argument-type] Argument to function `create_image_file` is incorrect: Expected `Sequence[int] | int`, found `@Todo | (Sequence[int] & ~(() -> object)) | (int & ~(() -> object)) | (((int, /) -> Sequence[int] | int) & ~(() -> object))`
- torchvision/transforms/v2/_utils.py:149:16: error[invalid-return-type] Return type does not match returned value: expected `(Any, /) -> Any`, found `(str & (() -> object)) | (((Any, /) -> Any) & (() -> object))`
- Found 1414 diagnostics
+ Found 1412 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_injection.py:298:9: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(), object] & ~Top[classmethod[Unknown, (), object]]` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:298:9: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]]` has no attribute `__qualname__`
- src/antidote/core/_injection.py:298:30: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(), object] & ~Top[classmethod[Unknown, (), object]]` has no attribute `__name__`
+ src/antidote/core/_injection.py:298:30: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]]` has no attribute `__name__`
- src/antidote/core/_injection.py:302:17: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(), object] & ~Top[classmethod[Unknown, (), object]] & ~MethodType` has no attribute `__qualname__`
+ src/antidote/core/_injection.py:302:17: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]] & ~MethodType` has no attribute `__qualname__`
- src/antidote/core/_injection.py:302:42: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[(), object] & ~Top[classmethod[Unknown, (), object]] & ~MethodType` has no attribute `__name__`
+ src/antidote/core/_injection.py:302:42: error[unresolved-attribute] Object of type `((...) -> object) & ~staticmethod[Top[(...)], object] & ~Top[classmethod[Unknown, Top[(...)], object]] & ~MethodType` has no attribute `__name__`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/strategy/informative_decorator.py:150:21: warning[possibly-missing-attribute] Attribute `format` may be missing on object of type `(str & ~AlwaysFalsy & ~(() -> object)) | (((Any, /) -> str) & ~AlwaysFalsy & ~(() -> object))`
- Found 688 diagnostics
+ Found 687 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/app_commands/commands.py:2477:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__discord_app_commands_checks__` on type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, (), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>)`
+ discord/app_commands/commands.py:2477:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__discord_app_commands_checks__` on type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu & ~<Protocol with members '__discord_app_commands_checks__'>)`
- discord/app_commands/commands.py:2479:13: error[unresolved-attribute] Object of type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, (), Unknown]] & ~ContextMenu) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]] & ~ContextMenu) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]] & ~ContextMenu) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]] & ~ContextMenu)` has no attribute `__discord_app_commands_checks__`
+ discord/app_commands/commands.py:2479:13: error[unresolved-attribute] Object of type `(((...) -> Coroutine[Any, Any, Unknown]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu) | (((Interaction[Any], Member, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu) | (((Interaction[Any], User, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu) | (((Interaction[Any], Message, /) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~ContextMenu)` has no attribute `__discord_app_commands_checks__`
- discord/app_commands/errors.py:453:95: error[unresolved-attribute] Object of type `(() -> Coroutine[object, Never, object]) | ((...) -> Coroutine[Any, Any, Unknown])` has no attribute `__qualname__`
+ discord/app_commands/errors.py:453:95: error[unresolved-attribute] Object of type `Top[(...) -> Coroutine[object, Never, object]]` has no attribute `__qualname__`
- discord/ext/commands/core.py:1942:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:1942:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:1944:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:1944:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2365:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:2365:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:2367:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:2367:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2368:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_guild_only__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]`
+ discord/ext/commands/core.py:2368:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_guild_only__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2440:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]] & ~<Protocol with members '__commands_checks__'>`
+ discord/ext/commands/core.py:2440:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__commands_checks__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]] & ~<Protocol with members '__commands_checks__'>`
- discord/ext/commands/core.py:2442:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]` has no attribute `__commands_checks__`
+ discord/ext/commands/core.py:2442:13: error[unresolved-attribute] Object of type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]` has no attribute `__commands_checks__`
- discord/ext/commands/core.py:2443:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_is_nsfw__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]`
+ discord/ext/commands/core.py:2443:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__discord_app_commands_is_nsfw__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2499:13: error[invalid-assignment] Object of type `CooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]`
+ discord/ext/commands/core.py:2499:13: error[invalid-assignment] Object of type `CooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2547:13: error[invalid-assignment] Object of type `DynamicCooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]`
+ discord/ext/commands/core.py:2547:13: error[invalid-assignment] Object of type `DynamicCooldownMapping[Context[Any]]` is not assignable to attribute `__commands_cooldown__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2582:13: error[invalid-assignment] Object of type `MaxConcurrency` is not assignable to attribute `__commands_max_concurrency__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]`
+ discord/ext/commands/core.py:2582:13: error[invalid-assignment] Object of type `MaxConcurrency` is not assignable to attribute `__commands_max_concurrency__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2634:13: error[invalid-assignment] Object of type `((CogT@before_invoke, ContextT@before_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@before_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__before_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]`
+ discord/ext/commands/core.py:2634:13: error[invalid-assignment] Object of type `((CogT@before_invoke, ContextT@before_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@before_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__before_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`
- discord/ext/commands/core.py:2657:13: error[invalid-assignment] Object of type `((CogT@after_invoke, ContextT@after_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@after_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__after_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, (), Unknown]]`
+ discord/ext/commands/core.py:2657:13: error[invalid-assignment] Object of type `((CogT@after_invoke, ContextT@after_invoke, /) -> Coroutine[Any, Any, Any]) | ((ContextT@after_invoke, /) -> Coroutine[Any, Any, Any])` is not assignable to attribute `__after_invoke__` on type `((...) -> Coroutine[Any, Any, Any]) & ~Top[Command[Unknown, Top[(...)], Unknown]]`

manticore (https://github.com/trailofbits/manticore)
- manticore/platforms/wasm.py:262:68: error[invalid-argument-type] Argument is incorrect: Expected `FunctionType`, found `(ProtoFuncInst & (() -> object) & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy) | (TableInst & (() -> object) & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy) | (MemInst & (() -> object) & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy) | (GlobalInst & (() -> object) & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy) | (((...) -> Unknown) & (() -> object) & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy)`
+ manticore/platforms/wasm.py:262:68: error[invalid-argument-type] Argument is incorrect: Expected `FunctionType`, found `(ProtoFuncInst & Top[(...) -> object] & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy) | (TableInst & Top[(...) -> object] & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy) | (MemInst & Top[(...) -> object] & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy) | (GlobalInst & Top[(...) -> object] & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy) | (((...) -> Unknown) & ~FuncAddr & ~TableAddr & ~MemAddr & ~GlobalAddr & ~AlwaysFalsy)`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/compilers/compilers.py:1336:9: error[unsupported-operator] Operator `+=` is not supported between objects of type `CompilerArgs` and `(CompilerArgs & ~(() -> object)) | (list[str] & ~(() -> object)) | (((CompileCheckMode, /) -> list[str]) & ~(() -> object)) | (@Todo & ~None) | list[Unknown]`
- mesonbuild/compilers/d.py:565:29: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[str]`, found `(list[str] & ~AlwaysFalsy & ~(() -> object)) | (((CompileCheckMode, /) -> list[str]) & Top[list[Unknown]] & ~AlwaysFalsy & ~(() -> object)) | (@Todo & Top[list[Unknown]])`
- mesonbuild/compilers/vala.py:195:9: error[unsupported-operator] Operator `+=` is not supported between objects of type `CompilerArgs` and `(CompilerArgs & ~(() -> object)) | (list[str] & ~(() -> object)) | (((CompileCheckMode, /) -> list[str]) & ~(() -> object)) | (@Todo & ~None) | list[Unknown]`
- Found 1949 diagnostics
+ Found 1946 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_vendor/jaraco/collections/__init__.py:42:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- setuptools/_vendor/typing_extensions.py:2855:17: error[invalid-assignment] Object of type `Unknown | str` is not assignable to attribute `__deprecated__` on type `_T@__call__ & (() -> object) & ~type`
+ setuptools/_vendor/typing_extensions.py:2855:17: error[invalid-assignment] Object of type `Unknown | str` is not assignable to attribute `__deprecated__` on type `_T@__call__ & Top[(...) -> object] & ~type`
- setuptools/config/_apply_pyprojecttoml.py:84:31: error[invalid-argument-type] Argument to function `_set_config` is incorrect: Expected `str`, found `(((Distribution, Any, str | PathLike[str] | None, /) -> None) & ~(() -> object)) | (str & ~(() -> object))`
- setuptools/config/_apply_pyprojecttoml.py:124:31: error[invalid-argument-type] Argument to function `_set_config` is incorrect: Expected `str`, found `(Unknown & ~(() -> object)) | (partial[None] & ~(() -> object))`
- setuptools/config/expand.py:334:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `(@Todo & <Protocol with members '__iter__'> & ~str) | (((...) -> Unknown) & <Protocol with members '__iter__'> & ~(() -> object) & ~str) | (Iterable[str | int] & ~(() -> object) & ~str)`
+ setuptools/config/expand.py:334:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `(Unknown & <Protocol with members '__iter__'> & ~str) | (Iterable[str | int] & ~Top[(...) -> object] & ~str)`
- Found 1272 diagnostics
+ Found 1271 diagnostics

archinstall (https://github.com/archlinux/archinstall)
- archinstall/lib/output.py:36:20: error[invalid-argument-type] Argument to function `hasattr` is incorrect: Expected `str`, found `(str & ~AlwaysFalsy & ~(() -> object)) | (((...) -> Unknown) & ~AlwaysFalsy & ~(() -> object))`
- archinstall/lib/output.py:36:61: error[invalid-argument-type] Argument to function `getattr` is incorrect: Expected `str`, found `(str & ~AlwaysFalsy & ~(() -> object)) | (((...) -> Unknown) & ~AlwaysFalsy & ~(() -> object))`
- archinstall/lib/output.py:37:23: error[invalid-argument-type] Argument to function `getattr` is incorrect: Expected `str`, found `(str & ~AlwaysFalsy & ~(() -> object)) | (((...) -> Unknown) & ~AlwaysFalsy & ~(() -> object))`
- Found 46 diagnostics
+ Found 43 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/funcs.py:55:20: error[invalid-assignment] Object of type `() -> object` is not assignable to `(...) -> T@partial_with_wrapper`
+ src/hydra_zen/funcs.py:55:20: error[invalid-assignment] Object of type `Top[(...) -> object]` is not assignable to `(...) -> T@partial_with_wrapper`
- src/hydra_zen/structured_configs/_implementations.py:2253:47: error[invalid-argument-type] Argument to function `get_target_path` is incorrect: Expected `HasTarget`, found `(() -> object) & type`
+ src/hydra_zen/structured_configs/_implementations.py:2253:47: error[invalid-argument-type] Argument to function `get_target_path` is incorrect: Expected `HasTarget`, found `type`

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/cli/utils/__init__.py:23:29: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ strawberry/types/field.py:143:38: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- strawberry/types/fields/resolver.py:392:20: error[invalid-return-type] Return type does not match returned value: expected `(...) -> T@StrawberryResolver`, found `(() -> object) | ((...) -> Unknown) | ((...) -> Unknown)`
+ strawberry/types/fields/resolver.py:392:20: error[invalid-return-type] Return type does not match returned value: expected `(...) -> T@StrawberryResolver`, found `Top[(...) -> object]`
- Found 384 diagnostics
+ Found 386 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/common.py:518:16: error[invalid-return-type] Return type does not match returned value: expected `dict[Hashable, T@_calc_assign_results]`, found `dict[Any, @Todo | (T@_calc_assign_results & ~(() -> object)) | (((C@_calc_assign_results, /) -> T@_calc_assign_results) & ~(() -> object))]`
- xarray/core/dataarray.py:3251:48: error[invalid-argument-type] Argument to bound method `drop_vars` is incorrect: Expected `Iterable[Hashable] | ((Dataset, /) -> Iterable[Hashable])`, found `@Todo | (Iterable[Hashable] & ~(() -> object)) | (((Self@drop_vars, /) -> Unknown | Iterable[Hashable]) & ~(() -> object))`
- xarray/core/dataarray.py:5277:35: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ xarray/core/dataarray.py:5277:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- xarray/core/dataset.py:1527:35: error[invalid-argument-type] Argument to function `getattr` is incorrect: Expected `str`, found `(str & ~(() -> object)) | (((Variable, Variable, /) -> bool) & ~(() -> object))`
- xarray/core/dataset.py:1530:48: error[invalid-argument-type] Argument to function `dict_equiv` is incorrect: Expected `(Variable, Variable, /) -> bool`, found `(str & (() -> object)) | (((Variable, Variable, /) -> bool) & (() -> object)) | (def compat(x: Variable, y: Variable) -> bool)`
+ xarray/core/dataset.py:1530:48: error[invalid-argument-type] Argument to function `dict_equiv` is incorrect: Expected `(Variable, Variable, /) -> bool`, found `(str & Top[(...) -> object]) | ((Variable, Variable, /) -> bool)`
- xarray/core/dataset.py:8110:35: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+ xarray/core/dataset.py:8110:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 1786 diagnostics
+ Found 1783 diagnostics

altair (https://github.com/vega/altair)
- altair/utils/plugin_registry.py:110:44: error[invalid-parameter-default] Default value of type `def callable(obj: object, /) -> TypeIs[() -> object]` is not assignable to annotated parameter type `(object, /) -> TypeIs[object]`
+ altair/utils/plugin_registry.py:110:44: error[invalid-parameter-default] Default value of type `def callable(obj: object, /) -> TypeIs[Top[(...) -> object]]` is not assignable to annotated parameter type `(object, /) -> TypeIs[object]`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/property/bases.py:187:24: error[invalid-return-type] Return type does not match returned value: expected `T@Property`, found `(() -> T@Property) | (T@Property & (() -> object))`
+ src/bokeh/core/property/bases.py:187:24: error[invalid-return-type] Return type does not match returned value: expected `T@Property`, found `(() -> T@Property) | (T@Property & Top[(...) -> object])`
- src/bokeh/io/notebook.py:568:30: error[invalid-argument-type] Argument to function `_origin_url` is incorrect: Expected `str`, found `(str & ~(() -> object)) | (((int | None, /) -> str) & ~(() -> object))`
- src/bokeh/io/notebook.py:580:27: error[invalid-argument-type] Argument to function `_server_url` is incorrect: Expected `str`, found `(str & ~(() -> object)) | (((int | None, /) -> str) & ~(() -> object))`
- src/bokeh/server/tornado.py:286:56: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Document, /) -> None`, found `(Mapping[str, Application | ((Document, /) -> None)] & (() -> object)) | (Application & (() -> object)) | (((Document, /) -> None) & (() -> object))`
+ src/bokeh/server/tornado.py:286:56: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Document, /) -> None`, found `(Mapping[str, Application | ((Document, /) -> None)] & Top[(...) -> object]) | (Application & Top[(...) -> object]) | ((Document, /) -> None)`
- src/bokeh/server/tornado.py:291:28: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/bokeh/server/tornado.py:295:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Document, /) -> None`, found `(Unknown & (() -> object)) | (Application & (() -> object))`
+ src/bokeh/server/tornado.py:295:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Document, /) -> None`, found `(Unknown & Top[(...) -> object]) | (Application & Top[(...) -> object])`
- src/bokeh/server/tornado.py:295:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Document, /) -> None`, found `(Unknown & (() -> object)) | (Application & (() -> object))`
+ src/bokeh/server/tornado.py:295:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(Document, /) -> None`, found `(Unknown & Top[(...) -> object]) | (Application & Top[(...) -> object])`
- Found 896 diagnostics
+ Found 893 diagnostics

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
- src/prefect/task_engine.py:438:13: error[invalid-assignment] Object of type `Unknown | None | (((Task[(...), Any], TaskRun, State[Any], /) -> Awaitable[bool] | bool) & (() -> object))` is not assignable to `((Task[P@SyncTaskRunEngine, Coroutine[Any, Any, R@SyncTaskRunEngine]], TaskRun, State[R@SyncTaskRunEngine], /) -> bool) | None`
+ src/prefect/task_engine.py:438:13: error[invalid-assignment] Object of type `Unknown | None | ((Task[(...), Any], TaskRun, State[Any], /) -> Awaitable[bool] | bool)` is not assignable to `((Task[P@SyncTaskRunEngine, Coroutine[Any, Any, R@SyncTaskRunEngine]], TaskRun, State[R@SyncTaskRunEngine], /) -> bool) | None`
- src/prefect/task_engine.py:1026:13: error[invalid-assignment] Object of type `Unknown | None | (((Task[(...), Any], TaskRun, State[Any], /) -> Awaitable[bool] | bool) & (() -> object))` is not assignable to `((Task[P@AsyncTaskRunEngine, Coroutine[Any, Any, R@AsyncTaskRunEngine]], TaskRun, State[R@AsyncTaskRunEngine], /) -> bool) | None`
+ src/prefect/task_engine.py:1026:13: error[invalid-assignment] Object of type `Unknown | None | ((Task[(...), Any], TaskRun, State[Any], /) -> Awaitable[bool] | bool)` is not assignable to `((Task[P@AsyncTaskRunEngine, Coroutine[Any, Any, R@AsyncTaskRunEngine]], TaskRun, State[R@AsyncTaskRunEngine], /) -> bool) | None`
- src/prefect/utilities/_engine.py:64:13: error[invalid-argument-type] Argument to bound method `is_callback_with_parameters` is incorrect: Expected `(...) -> str`, found `(Unknown & (() -> object)) | (() -> str) | (TaskRunNameCallbackWithParameters & (() -> object)) | (str & (() -> object))`
+ src/prefect/utilities/_engine.py:64:13: error[invalid-argument-type] Argument to bound method `is_callback_with_parameters` is incorrect: Expected `(...) -> str`, found `(Unknown & Top[(...) -> object]) | (() -> str) | TaskRunNameCallbackWithParameters | (str & Top[(...) -> object])`
+ src/prefect/utilities/_engine.py:69:29: error[missing-argument] No argument provided for required parameter `parameters` of bound method `__call__`
- src/prefect/utilities/_engine.py:93:13: error[unresolved-attribute] Object of type `() -> object` has no attribute `__name__`
+ src/prefect/utilities/_engine.py:93:13: error[unresolved-attribute] Object of type `Top[(...) -> object]` has no attribute `__name__`
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
- Found 5542 diagnostics
+ Found 5543 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/tests/conftest.py:21:35: error[invalid-type-form] Variable of type `def callable(obj: object, /) -> TypeIs[() -> object]

... (truncated 78 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-12-22 18:25_

`Top[Callable[..., object]]` is a different type to `Callable[..., object]`, unfortunately! The former is an (unsimplifiable) fully static type, the latter is a gradual type

---

_Comment by @AlexWaygood on 2025-12-22 18:34_

It's the same as the difference between `Top[list[Any]]` (also unsimplifiable) and `list[Any]`

---

_Comment by @AlexWaygood on 2025-12-22 19:05_

We implement `Top[list[Any]]` using this field on the `Specialization` struct: https://github.com/astral-sh/ruff/blob/4a937543b932a18e419c87b1f2717107b4dd2164/crates/ty_python_semantic/src/types/generics.rs#L734-L741. I think fixing the linked issue requires adding a similar field to `CallableType` and/or `Parameters`.

---

_Comment by @charliermarsh on 2025-12-22 19:48_

🫡 

---

_Renamed from "Use gradual form (`...`) for parameters in Callable bottom type" to "[ty] Use gradual form (`...`) for parameters in Callable bottom type" by @AlexWaygood on 2025-12-23 09:55_

---

_Label `ty` added by @AlexWaygood on 2025-12-23 09:56_

---

_Closed by @AlexWaygood on 2025-12-23 09:56_

---

_Reopened by @AlexWaygood on 2025-12-23 09:56_

---

_Renamed from "[ty] Use gradual form (`...`) for parameters in Callable bottom type" to "[ty] Use gradual form (`...`) for parameters in `Callable` top materialization" by @AlexWaygood on 2025-12-23 10:24_

---

_Comment by @AlexWaygood on 2025-12-23 10:29_

Nice, this looks like it's on the right track now!

> ```diff
> + beartype/_check/metadata/metadecor.py:555:34: error[unresolved-attribute] Object of type `((...) -> Unknown) & (Top[(...) -> object])` has no attribute `__name__`
> ```

Diagnostics like this indicate that some of the subtyping rules aren't quite right yet. All `Callable` types should be considered subtypes of `Top[(...) -> object]`; it is a fully static supertype of all `Callable` types. If we understood this subtype relationship correctly, then the intersection `((...) -> Unknown) & (Top[(...) -> object])` would naturally simplify down to `(...) -> Unknown`.

Similarly, `(...) -> int`, `(str, /) -> int` and `(bytes, bytes) -> bool` should all be understood as subtypes of `Top[(...) -> int]`, but `(...) -> object` should not, etc.

---

_Comment by @AlexWaygood on 2025-12-23 17:54_

The primer report is starting to look a lot better!

There's another change that we need to make... which might make the ecosystem blow up, but I do think it's the "correct" thing to do:

```py
def f(x: object):
    if callable(x):
        x()  # this should be an error...
             # we know that `x` is callable, but we do not know
             # *how* it is callable (is it okay to call it with *zero arguments*?).
             # `Top[Callable[..., object]]` is much stricter than `Callable[..., object]`
             # for things like this
```

`Top[Callable[..., object]]` is really a very strange type. Objects that have this type *are* callable... but any attempt to actually call one of these objects should lead to a type-checker error. `Top[Callable[..., object]]` represents the "infinite union" of all possible `Callable` types, so for any attempt to call an object with this type should cause the type checker to go "no, it could be a `Callable` object with a different signature to the one you're assuming".

This is similar to the way we apply [much stricter rules](https://play.ty.dev/c7499b31-ed4f-499e-ac87-852ad5348981) to `Top[list[Any]]` than we do to `list[Any]`: `Top[list[Any]]` represents the fully static "infinite union" of all possible `list` types, whereas `list[Any]` is a very permissive gradual type.

```py
from ty_extensions import Top
from typing import Any

def f(x: list[Any], y: Top[list[Any]]):
    x.append("foo")  # fine
    y.append("foo")  # error: expected `Never`, found `Literal["foo"]
```

---

_Marked ready for review by @charliermarsh on 2025-12-23 18:15_

---

_Review requested from @carljm by @charliermarsh on 2025-12-23 18:15_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-23 18:15_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-23 18:15_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-23 18:15_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-23 18:51_

---

_Comment by @astral-sh-bot[bot] on 2025-12-23 18:57_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 2 | 36 | 14 |
| `unresolved-attribute` | 6 | 3 | 19 |
| `invalid-assignment` | 0 | 6 | 21 |
| `invalid-return-type` | 0 | 4 | 13 |
| `possibly-missing-attribute` | 0 | 7 | 8 |
| `unsupported-operator` | 0 | 7 | 0 |
| `call-top-callable` | 5 | 0 | 0 |
| `missing-argument` | 5 | 0 | 0 |
| `not-iterable` | 0 | 3 | 1 |
| `unused-ignore-comment` | 3 | 1 | 0 |
| `call-non-callable` | 1 | 1 | 0 |
| `too-many-positional-arguments` | 0 | 2 | 0 |
| `type-assertion-failure` | 2 | 0 | 0 |
| `invalid-parameter-default` | 0 | 0 | 1 |
| `invalid-type-form` | 0 | 0 | 1 |
| `no-matching-overload` | 0 | 1 | 0 |
| **Total** | **24** | **71** | **78** |


**[Full report with detailed diff](https://b3281093.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://b3281093.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @AlexWaygood on 2025-12-23 19:04_

I wouldn't worry about the spack panic in ecosystem-analyzer — we've seen it on some other PRs; it's one of our nondeterminism issues right now

---

_Comment by @carljm on 2025-12-24 00:41_

Lots of false positives going away in the ecosystem report! 🎉 

I see a few new diagnostics with message "Object of type `Top[(...) -> object]` is not callable". These are cases where someone tries to call the top callable type. If we are being strict, this call can never succeed (since you are trying to call the union of all possible callables -- no arguments could possibly satisfy them all). But we should probably emit a custom diagnostic for this, because it's odd to do `if callable(x): x()` and have ty tell you that x "is not callable".

Other type checkers unsoundly use `(...) -> object` instead of the top callable, so they allow any call. We could easily use a different rule code for "trying to call the top callable", so people who want that forgiving behavior can opt into it? 

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/callable.md`:69 on 2025-12-24 00:46_

Error is correct, but as mentioned I think we should use a separate rule code and message here, since it's confusing to say "is not callable" when the code just checked that it's callable! Something like
```suggestion
        # error: [call-top-callable] "Object of type `Top[(...) -> object]` is not safe to call; its signature is not known"
```

Maybe `call-top-callable` is too jargony, but it's also just the rule code? Not thinking of anything obviously better.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:4139 on 2025-12-24 00:47_

```suggestion
    /// represents the infinite union of all callables. While such types *are* callable (they pass
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:4141 on 2025-12-24 00:48_

Although the term "unknown" is correct here, it's confusing given our frequent usage of `Unknown` as a dynamic type. This isn't user-facing, it's in code: maybe `CalledTopCallable`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:4575 on 2025-12-24 00:49_

```suggestion
                        "Object of type `{callable_ty_display}` is not safe to call; its signature is not known"
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:4579 on 2025-12-24 00:50_

Oh, didn't realize until now that we have this -- that's great, but I think the wording here could also be improved to avoid saying its "not callable"

```suggestion
                        "This type includes all possible callables, so it cannot safely be called, \
                        because there is no valid set of arguments for it",
```

---

_Comment by @charliermarsh on 2025-12-24 00:50_

(I _think_ that diagnostic is what Alex asked for here: https://github.com/astral-sh/ruff/pull/22145#issuecomment-3687487019. No issue from me with using a custom diagnostic for it!)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:12653 on 2025-12-24 00:54_

I think this can be simpler?

The top callable parameters are always a super-type of any other parameters, so we don't need to compare parameters at all. But we do allow various "top callable" types with various return types, so we do still need to compare return types. But I think that's all we should check. (And I think we should pass through the relation we are checking, not use assignability.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:12667 on 2025-12-24 00:54_

Same here, bottom paramers are a subtype of all possible parameters, so the only thing we need to check here is return types. (I think this could even be collapsed into a single match arm with the above.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:12684 on 2025-12-24 00:56_

And then I'm not sure we need any of these cases at all?

---

_@carljm reviewed on 2025-12-24 01:04_

Awesome!

I think it'd be good to have some tests showing the behavior of the bottom callable, too? It's always callable with any arguments.

The ways it could be created in real code are a bit complex. You'd need a gradual-form callable type in contravariant position of some type -- e.g. as an argument to another Callable -- and then you'd need to do something that would top-materialize the outer type, like return it wrapped in `TypeIs`. I'm not sure we need to represent this in a test; I'd just use `ty_extensions.Bottom` to create the type "manually".

---

_@charliermarsh reviewed on 2025-12-24 03:56_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:12653 on 2025-12-24 03:56_

Hopefully resolved.

---

_Review requested from @carljm by @charliermarsh on 2025-12-24 03:56_

---

_@dhruvmanila reviewed on 2025-12-24 05:36_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/diagnostic.rs`:175 on 2025-12-24 05:36_

Any specific reason this is in preview? Otherwise this should be `LintStatus::stable("0.0.7")`

---

_Renamed from "[ty] Use gradual form (`...`) for parameters in `Callable` top materialization" to "[ty] Fix implementation of `Top[Callable[..., object]]`" by @AlexWaygood on 2025-12-24 11:05_

---

_Comment by @AlexWaygood on 2025-12-24 11:09_

> ```diff
> + src/werkzeug/wsgi.py:244:25: error[invalid-assignment] Object of type `list[Unknown | (() -> None) | (Iterable[() -> None] & (Top[(...) -> object]))]` is not assignable to `None | (() -> None) | Iterable[() -> None]`
> ```

I think we can improve a few details of the display here. Namely, we always parenthesize callable types when they appear in unions or intersections, because something like `(...) -> Unknown & None` is otherwise ambiguous. (Is that `(...) -> (Unknown & None)` or `((...) -> Unknown) & None`?) But there's no ambiguity for `Top[(...) -> object]`, so I don't think parenthesizing it is necessary when it appears in unions or intersections. `Iterable[() -> None] & Top[(...) -> object]` feels just as clear to me as `Iterable[() -> None] & (Top[(...) -> object])`, and the latter has a lot more visual noise because of the additional parentheses.

---

_Comment by @AlexWaygood on 2025-12-24 11:17_

I think it would be good to add this test:

```py
from ty_extensions import Bottom, static_assert, is_subtype_of
from typing import Callable, Protocol, NoReturn

class EquivalentToBottom(Protocol):
    def __call__(self, *args: object, **kwargs: object) -> Never: ...

static_assert(is_subtype_of(EquivalentToBottom, Bottom[Callable[..., NoReturn]]))
static_assert(is_subtype_of(Bottom[Callable[..., NoReturn]], EquivalentToBottom))
static_assert(is_equivalent_to(Bottom[Callable[..., NoReturn]], EquivalentToBottom))
```

`Bottom[Callable[..., Any]]` is the same type as `Bottom[Callable[..., NoReturn]]`, and the same type as the `EquivalentToBottom` protocol, which is why those assertions should pass. In fact, we could consider just having an `is_top_materialization` boolean flag on `CallableType`, rather than having the `materialization_kind` field: I think the bottom materialization of `(..., T)` can always just be immediately simplified to `(*args: object, **kwargs: object) -> <bottom materialization of T>`?

---

_Comment by @AlexWaygood on 2025-12-24 11:18_

(Neither of my comments immediately above are necessarily blocking; it would be fine to tackle them as followups IMO.)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:1638 on 2025-12-24 11:20_

nit

```suggestion
        if let Type::Callable(callable) = self.signature_type
            && callable.materialization_kind(db) == Some(MaterializationKind::Top)
        {
            for overload in &mut self.overloads {
                overload
                    .errors
                    .push(BindingError::CalledTopCallable(self.signature_type));
            }
            return None;
        }
```

---

_@AlexWaygood reviewed on 2025-12-24 11:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:614 on 2025-12-24 11:23_

This looks incorrect. If the function doesn't have an annotated return type, it returns `Unknown`, not `Never`. `Unknown` is:
- Only a subtype of `object`
- Redundant with other dynamic types, unions including dynamic types, and/or `object`
- Assignable to everything

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:619 on 2025-12-24 11:23_

same here, this doesn't look correct

---

_@AlexWaygood reviewed on 2025-12-24 11:23_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-24 13:55_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-24 13:55_

---

_Review requested from @MichaReiser by @charliermarsh on 2025-12-24 13:55_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-24 13:56_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-24 13:57_

---

_Comment by @AlexWaygood on 2025-12-24 14:10_

This new diagnostic on `antidote`:

```
[error] invalid-argument-type - :286:17 - Argument to function `wrap` is incorrect: Argument type `Top[(...) -> object]` does not satisfy upper bound `(...) -> Any` of type variable `F`
```

Implies that we don't see `Top[(...) -> object]` as assignable to `(...) -> Any`. I'm not sure that's correct? I think any subtype of `Top[(...) -> object]` should be assignable to `(...) -> Any`, and that includes `Top[(...) -> object]` itself. That's what we do for `Top[list[Unknown]]`, anyway: https://play.ty.dev/0baef771-d6a1-474c-b906-2e56ac9c05f7.

It would be good to add a test for this. Quite a few of the new diagnostics in the latest ecosystem report look related to this.

---

_Comment by @Wizzerinus on 2025-12-24 14:13_

How can `Top[(...) -> object]` be assignable to `(...) -> Any` when the latter is callable with arbitrary set of arguments and and the former is not callable with any set of arguments? Is it only because the latter is a gradual type and nothing else? Isn't the whole point of this top materialization to prevent this?

---

_Comment by @charliermarsh on 2025-12-24 14:23_

> static_assert(is_equivalent_to(Bottom[Callable[..., NoReturn]], EquivalentToBottom))

(Hmm, I can't get this one to pass because I think we don't support `is_equivalent_to` for protocols and callables at all right now.)


---

_Comment by @AlexWaygood on 2025-12-24 14:24_

@Wizzerinus, quoting from the [typing spec](https://typing.python.org/en/latest/spec/concepts.html#the-assignable-to-or-consistent-subtyping-relation):

> Given the materialization relation and the subtyping relation, we can define the consistent subtype relation over all types. A type `B` is a consistent subtype of a type `A` if there exists a materialization `A'` of `A` and a materialization `B'` of `B`, where `A'` and `B'` are both fully static types, and `B'` is a subtype of `A'`.

("Consistent subtyping" is a synonym for "assignability".)

In this case:
1. `B` is `Top[(...) -> object]`
2. `A` is `(...) -> object`
3. The only possible materialization of `B` is itself, because it is already a fully static type: `Top[(...) -> object]`.
4. `A` is a gradual type that can materialize to any fully static type that is a subtype of `Top[A]` (which is `Top[(...) -> object]`) and a supertype of `Bottom[A]` (`(*args: object, **kwargs: object) -> Never`).
5. `Top[(...) -> object]` is a subtype of `Top[A]` (it is a subtype of itself) and a supertype of `Bottom[A]`.
6. Ergo, there is a pair of materializations of `A` (`A'`) and `B` (`B'`) where `B'` is a subtype of `A'`.
7. Ergo, `B` (`Top[(...) -> object]`) is assignable to `A` (`(...) -> object`)

---

_Comment by @AlexWaygood on 2025-12-24 14:25_

> (Hmm, I can't get this one to pass because I think we don't support `is_equivalent_to` for protocols and callables at all right now.)

it's fine to add a failing assertion with a `# error [static-assert-error]` next to it and a TODO above it, in that case 👍

---

_Comment by @charliermarsh on 2025-12-24 14:37_

Less confident about this most recent commit but gave it a try.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-24 14:47_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-24 14:47_

---

_Closed by @AlexWaygood on 2025-12-24 16:36_

---

_Reopened by @AlexWaygood on 2025-12-24 16:36_

---

_Comment by @AlexWaygood on 2025-12-24 16:46_

The ecosystem report's looking good again!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:584 on 2025-12-24 16:57_

```suggestion
    /// This is used when wrapping gradual callables in `Top[...]` - we want
    /// to preserve the gradual parameters but materialize the return types (which are in
    /// covariant position).
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:171 on 2025-12-24 16:58_

```suggestion
    ///         x()  # error: We know x is callable, but not what arguments it accepts
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:12707 on 2025-12-24 16:59_

do we have a test for this branch? Could we add one, if not?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:12663 on 2025-12-24 17:00_

I think you've overcomplicated this a _little_ bit. All tests pass if I apply this diff to your branch (which I think is correct):

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index d2a9d25dea..65c860414e 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -12661,57 +12661,32 @@ impl<'db> CallableType<'db> {
         }
 
         // Handle top materialization:
-        // - `Top[Callable[..., R]]` is a supertype of all callables with return type subtype of R.
-        //
-        // For Top, we only need to compare return types because Top parameters are a supertype
-        // of all possible parameters. Bottom materializations are simplified to the bottom
-        // callable directly, so they use normal signature comparison.
-        match (
-            self.is_top_materialization(db),
-            other.is_top_materialization(db),
-        ) {
-            // Neither is a top materialization: use normal signature comparison.
-            (false, false) => {}
-
-            // Anything <: Top[...]: just compare return types.
-            (_, true) => {
-                return self.signatures(db).return_types_have_relation_to(
-                    db,
-                    other.signatures(db),
-                    inferable,
-                    relation,
-                    relation_visitor,
-                    disjointness_visitor,
-                );
-            }
-
-            // Top[...] <: non-Top: depends on whether target has gradual parameters.
-            // For assignability, Top[(...) -> R] can be assigned to (...) -> S if R <: S
-            // (where (...) represents gradual parameters).
-            // For subtyping, Top is never a subtype of any specific callable.
-            (true, false) => {
-                if !relation.is_subtyping() && other.signatures(db).has_gradual_parameters() {
-                    return self.signatures(db).return_types_have_relation_to(
-                        db,
-                        other.signatures(db),
-                        inferable,
-                        relation,
-                        relation_visitor,
-                        disjointness_visitor,
-                    );
-                }
-                return ConstraintSet::from(false);
-            }
+        // `Top[Callable[..., R]]` is a fully static supertype of all callables
+        // with return type subtype of `R`.
+        if other.is_top_materialization(db) {
+            debug_assert!(
+                other.signatures(db).has_gradual_parameters(),
+                "A `Callable` type without gradual parameters \
+                should never be marked as a top materialization"
+            );
+            self.signatures(db).return_types_have_relation_to(
+                db,
+                other.signatures(db),
+                inferable,
+                relation,
+                relation_visitor,
+                disjointness_visitor,
+            )
+        } else {
+            self.signatures(db).has_relation_to_impl(
+                db,
+                other.signatures(db),
+                inferable,
+                relation,
+                relation_visitor,
+                disjointness_visitor,
+            )
         }
-
-        self.signatures(db).has_relation_to_impl(
-            db,
-            other.signatures(db),
-            inferable,
-            relation,
-            relation_visitor,
-            disjointness_visitor,
-        )
     }
 
     /// Check whether this callable type is equivalent to another callable type.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_properties/materialization.md`:216 on 2025-12-24 17:01_

nit: this is usually how we do x-failing mdtests -- it's good, because CI starts to fail if the test starts unexpectedly passing!

```suggestion
#
# error: [static-assert-error]
static_assert(is_equivalent_to(Bottom[Callable[..., Never]], EquivalentToBottom))
```

---

_@AlexWaygood approved on 2025-12-24 17:01_

This looks great -- thank you!! 😃

---

_@charliermarsh reviewed on 2025-12-24 17:08_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:12663 on 2025-12-24 17:08_

Oh wow

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-24 17:24_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-24 17:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/narrow/callable.md`:81 on 2025-12-24 17:36_

I'd be inclined to add a more direct assertion here as well, since I think there might be some open questions about how gradual upper bounds of TypeVars should work:

```suggestion
from typing import Any, Callable, TypeVar
from ty_extensions import static_assert, Top, is_assignable_to

static_assert(is_assignable_to(Top[Callable[..., bool]], Callable[..., int]))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:171 on 2025-12-24 17:37_

```suggestion
    ///         x()  # error: We know `x` is callable, but not what arguments it accepts
```

---

_@AlexWaygood approved on 2025-12-24 17:38_

Let's go!

---

_Comment by @charliermarsh on 2025-12-24 17:43_

Thank _you_ @AlexWaygood!

---

_Merged by @charliermarsh on 2025-12-24 17:49_

---

_Closed by @charliermarsh on 2025-12-24 17:49_

---

_Branch deleted on 2025-12-24 17:49_

---
