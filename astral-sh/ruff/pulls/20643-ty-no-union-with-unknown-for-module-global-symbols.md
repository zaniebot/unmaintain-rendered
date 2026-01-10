```yaml
number: 20643
title: "[ty] No union with `Unknown` for module-global symbols"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: david/no-unknown-union
created_at: 2025-09-30T07:57:29Z
updated_at: 2025-10-01T09:44:05Z
url: https://github.com/astral-sh/ruff/pull/20643
synced_at: 2026-01-10T17:40:28Z
```

# [ty] No union with `Unknown` for module-global symbols

---

_Pull request opened by @sharkdp on 2025-09-30 07:57_

# not ready for review

## Summary

https://github.com/astral-sh/ty/issues/1069

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-09-30 07:57_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-30 07:57_

---

_Comment by @github-actions[bot] on 2025-09-30 07:59_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-01 08:03:41.657168848 +0000
+++ new-output.txt	2025-10-01 08:03:44.982161744 +0000
@@ -373,7 +373,7 @@
 generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
 generics_defaults_specialization.py:45:1: error[type-assertion-failure] Argument does not have asserted type `@Todo(unsupported nested subscript in type[X])`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_components.py:49:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `tuple[Unknown, ...]`
+generics_paramspec_components.py:49:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `tuple[@Todo(Support for `typing.ParamSpec`), ...]`
 generics_paramspec_components.py:83:18: error[parameter-already-assigned] Multiple values provided for parameter 1 (`x`) of function `foo`
 generics_paramspec_semantics.py:13:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> str`
 generics_paramspec_semantics.py:17:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-30 08:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pegen (https://github.com/we-like-parsers/pegen)
+ src/pegen/grammar_parser.py:160:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str] | None`, found `tuple[Unknown, None]`
- Found 46 diagnostics
+ Found 47 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:3430:13: error[invalid-assignment] Object of type `KeysView[object]` is not assignable to `Sequence[Unknown] | AbstractSet[AnyKeyT@_zaggregate]`
+ aioredis/client.py:3430:13: error[invalid-assignment] Object of type `KeysView[object]` is not assignable to `Sequence[@Todo] | AbstractSet[AnyKeyT@_zaggregate]`
- aioredis/connection.py:289:41: error[unresolved-attribute] Type `BaseException` has no attribute `errno`
- aioredis/connection.py:517:41: error[unresolved-attribute] Type `BaseException` has no attribute `errno`
- Found 18 diagnostics
+ Found 16 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/llnl/util/tty/pty.py:75:15: warning[possibly-missing-attribute] Attribute `tcgetattr` on type `Unknown | None | <module 'termios'>` may be missing
+ lib/spack/spack/llnl/util/tty/pty.py:75:15: warning[possibly-missing-attribute] Attribute `tcgetattr` on type `None | <module 'termios'>` may be missing
- lib/spack/spack/llnl/util/tty/pty.py:76:31: warning[possibly-missing-attribute] Attribute `ICANON` on type `Unknown | None | <module 'termios'>` may be missing
+ lib/spack/spack/llnl/util/tty/pty.py:76:31: warning[possibly-missing-attribute] Attribute `ICANON` on type `None | <module 'termios'>` may be missing
- lib/spack/spack/llnl/util/tty/pty.py:76:62: warning[possibly-missing-attribute] Attribute `ECHO` on type `Unknown | None | <module 'termios'>` may be missing
+ lib/spack/spack/llnl/util/tty/pty.py:76:62: warning[possibly-missing-attribute] Attribute `ECHO` on type `None | <module 'termios'>` may be missing
- lib/spack/spack/test/llnl/util/lock.py:176:19: warning[possibly-missing-attribute] Attribute `rank` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:176:19: warning[possibly-missing-attribute] Attribute `rank` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:179:19: warning[possibly-missing-attribute] Attribute `bcast` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:179:19: warning[possibly-missing-attribute] Attribute `bcast` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:188:9: warning[possibly-missing-attribute] Attribute `barrier` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:188:9: warning[possibly-missing-attribute] Attribute `barrier` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:190:19: warning[possibly-missing-attribute] Attribute `rank` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:190:19: warning[possibly-missing-attribute] Attribute `rank` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:203:30: warning[possibly-missing-attribute] Attribute `rank` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:203:30: warning[possibly-missing-attribute] Attribute `rank` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:258:16: warning[possibly-missing-attribute] Attribute `size` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:258:16: warning[possibly-missing-attribute] Attribute `size` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:261:5: warning[possibly-missing-attribute] Attribute `Barrier` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:261:5: warning[possibly-missing-attribute] Attribute `Barrier` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:263:15: warning[possibly-missing-attribute] Attribute `rank` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:263:15: warning[possibly-missing-attribute] Attribute `rank` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:264:15: warning[possibly-missing-attribute] Attribute `Split` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:264:15: warning[possibly-missing-attribute] Attribute `Split` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:281:13: warning[possibly-missing-attribute] Attribute `Abort` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:281:13: warning[possibly-missing-attribute] Attribute `Abort` on type `None | Unknown` may be missing
- lib/spack/spack/test/llnl/util/lock.py:284:5: warning[possibly-missing-attribute] Attribute `Barrier` on type `Unknown | None` may be missing
+ lib/spack/spack/test/llnl/util/lock.py:284:5: warning[possibly-missing-attribute] Attribute `Barrier` on type `None | Unknown` may be missing

yarl (https://github.com/aio-libs/yarl)
- yarl/_url.py:565:44: error[index-out-of-bounds] Index 1 is out of bounds for tuple `tuple[Unknown | tuple[str, str, str, str, str]]` with length 1
+ yarl/_url.py:565:44: error[index-out-of-bounds] Index 1 is out of bounds for tuple `tuple[tuple[str, str, str, str, str]]` with length 1
- yarl/_url.py:567:19: error[index-out-of-bounds] Index 1 is out of bounds for tuple `tuple[Unknown | tuple[str, str, str, str, str]]` with length 1
+ yarl/_url.py:567:19: error[index-out-of-bounds] Index 1 is out of bounds for tuple `tuple[tuple[str, str, str, str, str]]` with length 1

websockets (https://github.com/aaugustin/websockets)
+ src/websockets/legacy/auth.py:163:37: warning[redundant-cast] Value is already of type `Iterable[tuple[str, str]]`
- src/websockets/server.py:110:17: error[unresolved-attribute] Type `(ServerProtocol, Sequence[Unknown], /) -> Unknown | None` has no attribute `__get__`
+ src/websockets/server.py:110:17: error[unresolved-attribute] Type `(ServerProtocol, Sequence[@Todo], /) -> @Todo | None` has no attribute `__get__`
- Found 41 diagnostics
+ Found 42 diagnostics

asynq (https://github.com/quora/asynq)
- asynq/async_task.py:213:20: warning[possibly-missing-attribute] Attribute `KEEP_DEPENDENCIES` on type `Unknown | DebugOptions` may be missing
+ asynq/async_task.py:213:20: error[unresolved-attribute] Type `DebugOptions` has no attribute `KEEP_DEPENDENCIES`
- asynq/debug.py:324:5: error[invalid-assignment] Object of type `Unknown | None` is not assignable to attribute `excepthook` of type `(type[BaseException], BaseException, TracebackType | None, /) -> Any`
+ asynq/debug.py:324:5: error[invalid-assignment] Object of type `None` is not assignable to attribute `excepthook` of type `(type[BaseException], BaseException, TracebackType | None, /) -> Any`
- asynq/scheduler.py:93:35: warning[possibly-missing-attribute] Attribute `MAX_TASK_STACK_SIZE` on type `Unknown | DebugOptions` may be missing
+ asynq/scheduler.py:93:35: error[unresolved-attribute] Type `DebugOptions` has no attribute `MAX_TASK_STACK_SIZE`

werkzeug (https://github.com/pallets/werkzeug)
- tests/live_apps/run.py:20:30: warning[possibly-missing-attribute] Attribute `app` on type `Unknown | ModuleType` may be missing
+ tests/live_apps/run.py:20:30: error[unresolved-attribute] Type `ModuleType` has no attribute `app`

paasta (https://github.com/yelp/paasta)
- paasta_tools/utils.py:1480:12: warning[possibly-missing-attribute] Attribute `log` on type `Unknown | None` may be missing
+ paasta_tools/utils.py:1480:12: error[unresolved-attribute] Type `None` has no attribute `log`
- paasta_tools/utils.py:1503:12: warning[possibly-missing-attribute] Attribute `log_audit` on type `Unknown | None` may be missing
+ paasta_tools/utils.py:1503:12: error[unresolved-attribute] Type `None` has no attribute `log_audit`

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/doctest.py:247:9: error[unknown-argument] Argument `continue_on_failure` does not match any known parameter of bound method `__init__`
- src/_pytest/fixtures.py:1658:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (Unknown & ~None & ~(() -> object) & ~str) | (((str, Config, /) -> Unknown) & ~(() -> object) & ~str) | (Unknown & ~str)`
+ src/_pytest/fixtures.py:1658:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (@Todo & ~None & ~(() -> object) & ~str) | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str) | (@Todo & ~str)`
+ src/_pytest/junitxml.py:372:9: error[invalid-assignment] Implicit shadowing of function `record_func`
- src/_pytest/pastebin.py:43:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method Stash.__setitem__[T](key: StashKey[T@__setitem__], value: T@__setitem__) -> None)` cannot be called with a key of type `Unknown | StashKey[IO[bytes]]` and a value of type `TextIOWrapper[_WrappedBuffer]` on object of type `Unknown | Stash`
+ src/_pytest/pastebin.py:43:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method Stash.__setitem__[T](key: StashKey[T@__setitem__], value: T@__setitem__) -> None)` cannot be called with a key of type `StashKey[IO[bytes]]` and a value of type `TextIOWrapper[_WrappedBuffer]` on object of type `Unknown | Stash`
- src/_pytest/pytester.py:1129:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method Stash.__setitem__[T](key: StashKey[T@__setitem__], value: T@__setitem__) -> None)` cannot be called with a key of type `Unknown | StashKey[int]` and a value of type `Literal[0]` on object of type `Unknown | Stash`
- src/_pytest/python.py:1096:13: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[str]`, found `Unknown | list[Unknown | str | _HiddenParam]`

alerta (https://github.com/alerta/alerta)
- alerta/database/backends/mongodb/base.py:1609:59: warning[possibly-missing-attribute] Attribute `DEFAULT_INFORM_SEVERITY` on type `Unknown | AlarmModel` may be missing
+ alerta/database/backends/mongodb/base.py:1609:59: error[unresolved-attribute] Type `AlarmModel` has no attribute `DEFAULT_INFORM_SEVERITY`
- alerta/database/backends/postgres/base.py:1540:57: warning[possibly-missing-attribute] Attribute `DEFAULT_INFORM_SEVERITY` on type `Unknown | AlarmModel` may be missing
+ alerta/database/backends/postgres/base.py:1540:57: error[unresolved-attribute] Type `AlarmModel` has no attribute `DEFAULT_INFORM_SEVERITY`
- alerta/views/alerts.py:310:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/alerts.py:358:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/alerts.py:385:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/alerts.py:406:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/alerts.py:433:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/alerts.py:460:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/alerts.py:486:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/alerts.py:511:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/alerts.py:536:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/alerts.py:561:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/alerts.py:310:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/alerts.py:358:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/alerts.py:385:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/alerts.py:406:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/alerts.py:433:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/alerts.py:460:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/alerts.py:486:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/alerts.py:511:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/alerts.py:536:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/alerts.py:561:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
- alerta/views/blackouts.py:66:13: warning[possibly-missing-attribute] Attribute `blackouts` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/blackouts.py:66:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `blackouts`
- alerta/views/bulk.py:43:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/bulk.py:85:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/bulk.py:105:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/bulk.py:120:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/bulk.py:135:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- alerta/views/bulk.py:147:13: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/bulk.py:43:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/bulk.py:85:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/bulk.py:105:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/bulk.py:120:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/bulk.py:135:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ alerta/views/bulk.py:147:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
- alerta/views/customers.py:58:13: warning[possibly-missing-attribute] Attribute `customers` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/customers.py:58:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `customers`
- alerta/views/groups.py:83:13: warning[possibly-missing-attribute] Attribute `groups` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/groups.py:83:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `groups`
- alerta/views/heartbeats.py:62:13: warning[possibly-missing-attribute] Attribute `heartbeats` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/heartbeats.py:62:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `heartbeats`
- alerta/views/keys.py:81:13: warning[possibly-missing-attribute] Attribute `keys` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/keys.py:81:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `keys`
- alerta/views/oembed.py:35:21: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/oembed.py:35:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
- alerta/views/permissions.py:71:13: warning[possibly-missing-attribute] Attribute `perms` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/permissions.py:71:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `perms`
- alerta/views/permissions.py:77:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Scope]`, found `Unknown | list[Unknown | str]`
+ alerta/views/permissions.py:77:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
- alerta/views/users.py:133:13: warning[possibly-missing-attribute] Attribute `users` on type `Unknown | QueryBuilder` may be missing
+ alerta/views/users.py:133:13: error[unresolved-attribute] Type `QueryBuilder` has no attribute `users`
- alerta/webhooks/grafana.py:116:25: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
+ alerta/webhooks/grafana.py:116:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
- tests/test_search.py:59:25: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:98:25: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:116:21: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:131:21: warning[possibly-missing-attribute] Attribute `alerts` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:165:25: warning[possibly-missing-attribute] Attribute `blackouts` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:192:25: warning[possibly-missing-attribute] Attribute `blackouts` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:212:21: warning[possibly-missing-attribute] Attribute `blackouts` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:245:25: warning[possibly-missing-attribute] Attribute `heartbeats` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:266:25: warning[possibly-missing-attribute] Attribute `heartbeats` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:283:21: warning[possibly-missing-attribute] Attribute `heartbeats` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:313:25: warning[possibly-missing-attribute] Attribute `keys` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:334:25: warning[possibly-missing-attribute] Attribute `keys` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:356:21: warning[possibly-missing-attribute] Attribute `keys` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:388:25: warning[possibly-missing-attribute] Attribute `users` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:411:25: warning[possibly-missing-attribute] Attribute `users` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:428:21: warning[possibly-missing-attribute] Attribute `users` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:451:25: warning[possibly-missing-attribute] Attribute `groups` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:465:25: warning[possibly-missing-attribute] Attribute `groups` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:481:21: warning[possibly-missing-attribute] Attribute `groups` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:504:25: warning[possibly-missing-attribute] Attribute `perms` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:517:25: warning[possibly-missing-attribute] Attribute `perms` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:534:21: warning[possibly-missing-attribute] Attribute `perms` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:557:25: warning[possibly-missing-attribute] Attribute `customers` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:570:25: warning[possibly-missing-attribute] Attribute `customers` on type `Unknown | QueryBuilder` may be missing
- tests/test_search.py:588:21: warning[possibly-missing-attribute] Attribute `customers` on type `Unknown | QueryBuilder` may be missing
+ tests/test_search.py:59:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ tests/test_search.py:98:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ tests/test_search.py:116:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ tests/test_search.py:131:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `alerts`
+ tests/test_search.py:165:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `blackouts`
+ tests/test_search.py:192:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `blackouts`
+ tests/test_search.py:212:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `blackouts`
+ tests/test_search.py:245:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `heartbeats`
+ tests/test_search.py:266:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `heartbeats`
+ tests/test_search.py:283:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `heartbeats`
+ tests/test_search.py:313:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `keys`
+ tests/test_search.py:334:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `keys`
+ tests/test_search.py:356:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `keys`
+ tests/test_search.py:388:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `users`
+ tests/test_search.py:411:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `users`
+ tests/test_search.py:428:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `users`
+ tests/test_search.py:451:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `groups`
+ tests/test_search.py:465:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `groups`
+ tests/test_search.py:481:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `groups`
+ tests/test_search.py:504:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `perms`
+ tests/test_search.py:517:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `perms`
+ tests/test_search.py:534:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `perms`
+ tests/test_search.py:557:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `customers`
+ tests/test_search.py:570:25: error[unresolved-attribute] Type `QueryBuilder` has no attribute `customers`
+ tests/test_search.py:588:21: error[unresolved-attribute] Type `QueryBuilder` has no attribute `customers`

starlette (https://github.com/encode/starlette)
+ tests/test_formparsers.py:297:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="field0"\r\n\r\nvalue0\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="file"; filename="file.txt"\r\nContent-Type: text/plain\r\n\r\n<file content>\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="field1"\r\n\r\nvalue1\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c--\r\n']`
+ tests/test_formparsers.py:372:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="file"; filename="\xe6\x96\x87\xe6\x9b\xb8.txt"\r\nContent-Type: text/plain\r\n\r\n<file content>\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c--\r\n']`
+ tests/test_formparsers.py:396:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; name="file"; filename="\xe7\x94\xbb\xe5\x83\x8f.jpg"\r\nContent-Type: image/jpeg\r\n\r\n<file content>\r\n--a7f7ac8d4e2e437c877bb7b8d7cc549c--\r\n']`
+ tests/test_formparsers.py:420:9: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--20b303e711c4ab8c443184ac833ab00f\r\nContent-Disposition: form-data; name="value"\r\n\r\nTransf\xc3\xa9rer\r\n--20b303e711c4ab8c443184ac833ab00f--\r\n']`
+ tests/test_formparsers.py:494:13: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'Content-Disposition: form-data; name="file"; filename="\xe6\x96\x87\xe6\x9b\xb8.txt"\r\nContent-Type: text/plain\r\n\r\n<file content>\r\n']`
+ tests/test_formparsers.py:522:13: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `Mapping[str, Iterable[str]] | None`, found `Literal[b'--a7f7ac8d4e2e437c877bb7b8d7cc549c\r\nContent-Disposition: form-data; ="field0"\r\n\r\nvalue0\r\n']`
- tests/test_formparsers.py:554:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_formparsers.py:581:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_formparsers.py:610:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_formparsers.py:638:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_formparsers.py:668:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_formparsers.py:698:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_formparsers.py:715:21: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_formparsers.py:761:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_requests.py:97:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_requests.py:118:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_requests.py:160:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_requests.py:179:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_requests.py:479:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_responses.py:258:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_responses.py:366:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_websockets.py:477:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 147 diagnostics
+ Found 137 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/_windows_renderer.py:7:60: error[invalid-type-form] Variable of type `Never` is not allowed in a type expression
- rich/table.py:359:5: error[invalid-argument-type] Argument to bound method `setter` is incorrect: Expected `(Any, Any, /) -> None`, found `def padding(self, padding: Unknown) -> Table`
+ rich/table.py:359:5: error[invalid-argument-type] Argument to bound method `setter` is incorrect: Expected `(Any, Any, /) -> None`, found `def padding(self, padding: @Todo) -> Table`
+ tests/test_syntax.py:169:38: error[invalid-argument-type] Argument to bound method `get_style_for_token` is incorrect: Expected `tuple[str, ...]`, found `Literal["abc"]`
- Found 309 diagnostics
+ Found 311 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | int`
- boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | int`
- boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:664:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | int`
- boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | int`
- boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | int`
- boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:676:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | int`
- boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | int`
- boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | int`
- boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:689:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | int`
- boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | int`
- boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | int`
- boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:710:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | int`
- boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | int`
- boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | int`
- boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:722:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | int`
- boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | type[Action]`, found `<class 'int'> | str | int`
- boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `int | str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `Iterable[Unknown] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `bool`, found `<class 'int'> | str | int`
- boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | tuple[str, ...] | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str | None`, found `<class 'int'> | str | int`
- boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | Unknown | int`
+ boostedblob/cli.py:754:45: error[invalid-argument-type] Argument to bound method `add_argument` is incorrect: Expected `str`, found `<class 'int'> | str | int`

dulwich (https://github.com/dulwich/dulwich)
- dulwich/diff_tree.py:177:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/diff_tree.py:178:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/notes.py:626:16: error[invalid-return-type] Return type does not match returned value: expected `bytes`, found `bytes | None | Unknown`
+ dulwich/notes.py:626:16: error[invalid-return-type] Return type does not match returned value: expected `bytes`, found `bytes | None`
+ dulwich/walk.py:146:21: error[invalid-argument-type] Argument to function `tree_changes` is incorrect: Expected `bytes | None`, found `None | Unknown | list[Unknown]`
- dulwich/worktree.py:918:28: error[invalid-argument-type] Method `__getitem__` of type `bound method DiskRefsContainer.__getitem__(name: bytes) -> Unknown | bytes` cannot be called with key of type `str | bytes` on object of type `DiskRefsContainer`
+ dulwich/worktree.py:918:28: error[invalid-argument-type] Method `__getitem__` of type `bound method DiskRefsContainer.__getitem__(name: bytes) -> bytes` cannot be called with key of type `str | bytes` on object of type `DiskRefsContainer`
- dulwich/worktree.py:918:28: error[invalid-argument-type] Method `__getitem__` of type `bound method ReftableRefsContainer.__getitem__(name: bytes) -> Unknown | bytes` cannot be called with key of type `str | bytes` on object of type `ReftableRefsContainer`
+ dulwich/worktree.py:918:28: error[invalid-argument-type] Method `__getitem__` of type `bound method ReftableRefsContainer.__getitem__(name: bytes) -> bytes` cannot be called with key of type `str | bytes` on object of type `ReftableRefsContainer`
- Found 197 diagnostics
+ Found 196 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/MainClass.py:548:39: warning[possibly-missing-attribute] Attribute `strftime` on type `@Todo | _NotSetType` may be missing
+ github/MainClass.py:548:39: warning[possibly-missing-attribute] Attribute `strftime` on type `_NotSetType | (@Todo & datetime)` may be missing
- github/MainClass.py:910:42: warning[possibly-missing-attribute] Attribute `_identity` on type `@Todo | _NotSetType` may be missing
+ github/MainClass.py:910:42: warning[possibly-missing-attribute] Attribute `_identity` on type `_NotSetType | @Todo` may be missing
- github/Milestone.py:202:41: warning[possibly-missing-attribute] Attribute `strftime` on type `@Todo | _NotSetType` may be missing
+ github/Milestone.py:202:41: warning[possibly-missing-attribute] Attribute `strftime` on type `_NotSetType | (@Todo & date)` may be missing
- github/NamedUser.py:444:39: warning[possibly-missing-attribute] Attribute `strftime` on type `@Todo | _NotSetType` may be missing
+ github/NamedUser.py:444:39: warning[possibly-missing-attribute] Attribute `strftime` on type `_NotSetType | (@Todo & datetime)` may be missing
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | str | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | str | None`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | str`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | str`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Retry | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Retry | None`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AppAuth | None`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AppAuth | None`, found `AppAuth | str | int`
- tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `AppAuth | str | Unknown | int`
+ tests/GithubIntegration.py:150:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | str | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | str | None`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | str`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | str`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Retry | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | Retry | None`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float | None`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AppAuth | None`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AppAuth | None`, found `AppAuth | str | int`
- tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `AppAuth | str | Unknown | int`
+ tests/Installation.py:119:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `AppAuth | str | int`

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[@Todo] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
- tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Unknown] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[@Todo] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/type/test_validation.py:1071:9: error[invalid-assignment] Object of type `tuple[None]` is not assignable to attribute `interfaces` of type `CachedProperty`
- Found 423 diagnostics
+ Found 424 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/model.py:183:20: warning[deprecated] The function `warn` is deprecated: Deprecated since Python 3.3. Use `Logger.warning()` instead.
+ sockeye/model.py:187:24: warning[deprecated] The function `warn` is deprecated: Deprecated since Python 3.3. Use `Logger.warning()` instead.
- sockeye/train.py:445:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[BaseParallelSampleIter, BaseParallelSampleIter, DataConfig, list[Unknown], list[Unknown]]`, found `tuple[BaseParallelSampleIter, BaseParallelSampleIter | None, DataConfig, list[Any], list[Any]]`
+ sockeye/train.py:445:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[BaseParallelSampleIter, BaseParallelSampleIter, DataConfig, list[Any], list[Any]]`, found `tuple[BaseParallelSampleIter, BaseParallelSampleIter | None, DataConfig, list[Any], list[Any]]`
+ sockeye/utils.py:843:20: warning[deprecated] The function `warn` is deprecated: Deprecated since Python 3.3. Use `Logger.warning()` instead.
- test/unit/test_inference.py:53:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Unknown]`, found `None`
+ test/unit/test_inference.py:53:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `None`
- test/unit/test_inference.py:54:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Unknown]`, found `None`
+ test/unit/test_inference.py:54:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `None`
- test/unit/test_loss.py:93:17: warning[possibly-missing-attribute] Attribute `sum` on type `Unknown | bool` may be missing
+ test/unit/test_loss.py:93:17: error[unresolved-attribute] Type `bool` has no attribute `sum`
- Found 316 diagnostics
+ Found 319 diagnostics

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
- test/test_generated_mypy.py:486:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[Unknown, Email]`
+ test/test_generated_mypy.py:486:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[@Todo, Email]`
- test/test_generated_mypy.py:497:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[Unknown, Email]`
+ test/test_generated_mypy.py:497:5: error[type-assertion-failure] Argument does not have asserted type `ScalarMap[@Todo, Email]`

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/_internal/_decorators_v1.py:174:12: error[invalid-return-type] Return type does not match returned value: expected `V2CoreBeforeRootValidator`, found `def _wrapper2(fields_tuple: tuple[Any, ...], _: ValidationInfo[Any | None]) -> tuple[Any, ...]`
- pydantic/_internal/_generate_schema.py:2442:1: error[invalid-assignment] Object of type `dict[Unknown | tuple[str, str], Unknown | ((f, schema) -> Unknown) | ((f, _) -> Unknown)]` is not assignable to `Mapping[tuple[@Todo, Literal["no-info", "with-info"]], ((...) -> Any, Unknown, /) -> Unknown]`
+ pydantic/_internal/_generate_schema.py:2442:1: error[invalid-assignment] Object of type `dict[Unknown | tuple[str, str], Unknown | ((f, schema) -> Unknown) | ((f, _) -> Unknown)]` is not assignable to `Mapping[tuple[@Todo, Literal["no-info", "with-info"]], ((...) -> Any, @Todo, /) -> @Todo]`
- pydantic/v1/main.py:935:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, Unknown]`
+ pydantic/v1/main.py:935:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, @Todo]`
- pydantic/v1/main.py:949:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, Unknown]`
+ pydantic/v1/main.py:949:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, @Todo]`
- pydantic/v1/main.py:962:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, Unknown]`
+ pydantic/v1/main.py:962:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `dict[str, @Todo]`
- pydantic/v1/utils.py:610:17: error[invalid-argument-type] Argument to function `assert_never` is incorrect: Expected `Never`, found `Unknown & ~Top[Mapping[Unknown, object]] & ~AbstractSet[object]`
+ pydantic/v1/utils.py:613:16: error[invalid-return-type] Return type does not match returned value: expected `Mapping[@Todo, Any]`, found `AbstractSet[@Todo] | Mapping[@Todo, Any] | dict[Unknown, ellipsis]`
- Found 765 diagnostics
+ Found 766 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_internal/utils/misc.py:491:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, tuple[str, tuple[str | None, str | None]]]`, found `tuple[Literal[b""], Unknown | tuple[str, tuple[str | None, str | None]]]`
+ src/pip/_internal/utils/misc.py:491:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, tupl...*[Comment body truncated]*

---

_Comment by @github-actions[bot] on 2025-09-30 08:03_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-operator` | 15 | 12 | 2,605 |
| `possibly-missing-attribute` | 3 | 1,343 | 148 |
| `unresolved-attribute` | 1,453 | 11 | 24 |
| `no-matching-overload` | 849 | 4 | 0 |
| `invalid-argument-type` | 169 | 55 | 483 |
| `type-assertion-failure` | 12 | 666 | 9 |
| `unsupported-base` | 0 | 0 | 191 |
| `invalid-assignment` | 35 | 63 | 73 |
| `unused-ignore-comment` | 17 | 84 | 0 |
| `invalid-return-type` | 30 | 5 | 31 |
| `invalid-context-manager` | 0 | 0 | 65 |
| `conflicting-argument-forms` | 0 | 36 | 0 |
| `non-subscriptable` | 0 | 0 | 20 |
| `possibly-unresolved-reference` | 0 | 17 | 0 |
| `invalid-type-form` | 7 | 0 | 8 |
| `invalid-exception-caught` | 0 | 0 | 11 |
| `deprecated` | 7 | 0 | 0 |
| `possibly-missing-implicit-call` | 0 | 1 | 4 |
| `call-non-callable` | 0 | 4 | 0 |
| `not-iterable` | 1 | 1 | 2 |
| `invalid-parameter-default` | 0 | 0 | 3 |
| `redundant-cast` | 3 | 0 | 0 |
| `unresolved-reference` | 0 | 3 | 0 |
| `index-out-of-bounds` | 0 | 0 | 2 |
| `invalid-key` | 2 | 0 | 0 |
| `invalid-super-argument` | 0 | 0 | 2 |
| `invalid-syntax-in-forward-annotation` | 0 | 2 | 0 |
| `invalid-attribute-access` | 1 | 0 | 0 |
| `invalid-raise` | 1 | 0 | 0 |
| `missing-argument` | 1 | 0 | 0 |
| `too-many-positional-arguments` | 1 | 0 | 0 |
| `unknown-argument` | 1 | 0 | 0 |
| `unresolved-import` | 0 | 1 | 0 |
| `unsupported-bool-conversion` | 0 | 1 | 0 |
| **Total** | **2,608** | **2,309** | **3,681** |

**[Full report with detailed diff](https://david-no-unknown-union.ecosystem-663.pages.dev/diff)** ([timing results](https://david-no-unknown-union.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-30 09:28_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-30 09:28_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-09-30 12:43_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-30 12:43_

---

_Comment by @sharkdp on 2025-10-01 09:44_

Let's do this without literal promotion first, and then potentially add literal promotion in a second step to evaluate it separately.

---

_Closed by @sharkdp on 2025-10-01 09:44_

---
