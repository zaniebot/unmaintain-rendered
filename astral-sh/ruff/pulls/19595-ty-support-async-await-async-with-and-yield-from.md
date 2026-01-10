```yaml
number: 19595
title: "[ty] Support `async`/`await`, `async with` and `yield from`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/async-await
created_at: 2025-07-28T11:58:05Z
updated_at: 2025-07-30T09:51:22Z
url: https://github.com/astral-sh/ruff/pull/19595
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Support `async`/`await`, `async with` and `yield from`

---

_Pull request opened by @sharkdp on 2025-07-28 11:58_

## Summary

- Add support for the return types of `async` functions
- Add type inference for `await` expressions
- Add support for `async with` / async context managers
- Add support for `yield from` expressions

This PR is generally lacking proper error handling in some cases (e.g. illegal `__await__` attributes). I'm planning to work on this in a follow-up.

part of https://github.com/astral-sh/ty/issues/151

closes https://github.com/astral-sh/ty/issues/736

## Ecosystem

There are a lot of true positives on `prefect` which look similar to:
```diff
prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:406:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
```

This is due to a wrong return type annotation [here](https://github.com/PrefectHQ/prefect/blob/e926b8c4c114e74533e7bd75d83e7f205c774645/src/integrations/prefect-aws/tests/workers/test_ecs_worker.py#L355-L391).

```diff
mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ test/mitmproxy/addons/test_clientplayback.py:18:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `def tcp_server(handle_conn, **server_args) -> Unknown | tuple[str, int]`
```

[This](https://github.com/mitmproxy/mitmproxy/blob/a4d794c59a27472d193a592d8037505a1cf6ae93/test/mitmproxy/addons/test_clientplayback.py#L18-L19) is a true positive. That function should return `AsyncIterator[Address]`, not `Address`.

I looked through almost all of the other new diagnostics and they all look like known problems or true positives.

## Typing conformance

The typing conformance diff looks good.

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-07-28 11:58_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-28 11:58_

---

_@sharkdp reviewed on 2025-07-28 11:59_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:458 on 2025-07-28 11:59_

??

---

_Comment by @github-actions[bot] on 2025-07-28 12:00_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-07-30 08:56:13.250732152 +0000
+++ new-output.txt	2025-07-30 08:56:13.314732684 +0000
@@ -87,7 +87,6 @@
 aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[T_co]'>` with no `__class_getitem__` method
 aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[T_co]'>` with no `__class_getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[T_co, T_contra]'>` with no `__class_getitem__` method
-annotations_coroutines.py:27:5: error[type-assertion-failure] Argument does not have asserted type `str`
 annotations_forward_refs.py:22:7: error[unresolved-reference] Name `ClassA` used when not defined
 annotations_forward_refs.py:23:12: error[unresolved-reference] Name `ClassA` used when not defined
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
@@ -102,8 +101,6 @@
 annotations_forward_refs.py:96:1: error[type-assertion-failure] Argument does not have asserted type `int`
 annotations_generators.py:86:21: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.GeneratorType`
 annotations_generators.py:91:27: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.AsyncGeneratorType`
-annotations_generators.py:167:5: error[type-assertion-failure] Argument does not have asserted type `AsyncGenerator[str, None]`
-annotations_generators.py:174:5: error[type-assertion-failure] Argument does not have asserted type `AsyncGenerator[str, None]`
 annotations_generators.py:193:1: error[type-assertion-failure] Argument does not have asserted type `() -> AsyncIterator[int]`
 annotations_methods.py:31:1: error[type-assertion-failure] Argument does not have asserted type `A`
 annotations_methods.py:36:1: error[type-assertion-failure] Argument does not have asserted type `B`
@@ -892,4 +889,4 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 893 diagnostics
+Found 890 diagnostics
```
</details>


---

_Comment by @github-actions[bot] on 2025-07-28 12:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- mypy_primer/utils.py:114:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[CompletedProcess[str], int | float]`, found `tuple[CompletedProcess[@Todo(generic `typing.Awaitable` type) | None], int | float]`
- Found 9 diagnostics
+ Found 8 diagnostics

anyio (https://github.com/agronholm/anyio)
+ src/anyio/_core/_contextmanagers.py:165:16: error[invalid-return-type] Return type does not match returned value: expected `_T_co`, found `object`
+ src/anyio/_core/_sockets.py:596:64: error[invalid-argument-type] Argument to function `convert_ipv6_sockaddr` is incorrect: Expected `tuple[str, int, int, int] | tuple[str, int]`, found `tuple[str, int] | tuple[str, int, int, int] | tuple[int, bytes]`
- Found 83 diagnostics
+ Found 85 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/pyutils/gather_with_cancel.py:28:16: error[invalid-return-type] Return type does not match returned value: expected `list[Any]`, found `tuple[Unknown]`
+ src/graphql/pyutils/gather_with_cancel.py:30:16: error[invalid-return-type] Return type does not match returned value: expected `list[Any]`, found `tuple[Unknown]`
- Found 327 diagnostics
+ Found 329 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_run.py:1736:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_core/_tests/test_run.py:1746:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/test_highlevel_open_tcp_listeners.py:408:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/test_subprocess.py:370:32: error[invalid-argument-type] Argument to function `run_process` is incorrect: Expected `bytes | bytearray | memoryview[Unknown] | int | HasFileno | None`, found `Literal["oh no, it's text"]`
+ src/trio/_tests/test_testing.py:423:20: error[invalid-return-type] Return type does not match returned value: expected `bytes`, found `bytearray`
- src/trio/_tests/test_util.py:109:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/trio/_tests/test_util.py:120:28: error[not-iterable] Object of type `CoroutineType[Any, Any, None]` is not iterable
- src/trio/_tests/test_util.py:154:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/testing/_fake_net.py:330:56: error[invalid-argument-type] Argument to bound method `from_python_sockaddr` is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `None | @Todo(generic `typing.Awaitable` type)`
+ src/trio/testing/_fake_net.py:330:56: error[invalid-argument-type] Argument to bound method `from_python_sockaddr` is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `None | Unknown`
- Found 738 diagnostics
+ Found 734 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/server.py:273:94: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pymongo/synchronous/helpers.py:86:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 412 diagnostics
+ Found 410 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/webhook/async_.py:212:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 519 diagnostics
+ Found 518 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/http/async_base_view.py:213:20: error[invalid-return-type] Return type does not match returned value: expected `ExecutionResult | list[ExecutionResult] | SubscriptionExecutionResult`, found `tuple[Unknown]`
- Found 368 diagnostics
+ Found 369 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/tests/test_package.py:31:12: error[unresolved-attribute] Type `bound method Master.get_package_metadata(package_name: str, serial: int = Literal[0]) -> @Todo(generic types.CoroutineType)` has no attribute `await_count`
+ src/bandersnatch/tests/test_package.py:31:12: error[unresolved-attribute] Type `bound method Master.get_package_metadata(package_name: str, serial: int = Literal[0]) -> CoroutineType[Any, Any, Any]` has no attribute `await_count`
- src/bandersnatch/tests/test_package.py:58:12: error[unresolved-attribute] Type `bound method Master.get_package_metadata(package_name: str, serial: int = Literal[0]) -> @Todo(generic types.CoroutineType)` has no attribute `await_count`
+ src/bandersnatch/tests/test_package.py:58:12: error[unresolved-attribute] Type `bound method Master.get_package_metadata(package_name: str, serial: int = Literal[0]) -> CoroutineType[Any, Any, Any]` has no attribute `await_count`

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/lib/database/redis_utest.py:46:9: error[unresolved-attribute] Type `def _initialize_async_redis(host: str, port: int, wait_time: int = Literal[30]) -> @Todo(generic types.CoroutineType)` has no attribute `assert_called_once`
+ dragonchain/lib/database/redis_utest.py:46:9: error[unresolved-attribute] Type `def _initialize_async_redis(host: str, port: int, wait_time: int = Literal[30]) -> CoroutineType[Any, Any, Unknown]` has no attribute `assert_called_once`

websockets (https://github.com/aaugustin/websockets)
- src/websockets/legacy/server.py:607:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `@Todo(generic `typing.Awaitable` type) | Literal[HTTPStatus.SERVICE_UNAVAILABLE, b"Server is shutting down.\n"] | list[Unknown]`
+ src/websockets/legacy/server.py:607:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `Unknown | Literal[HTTPStatus.SERVICE_UNAVAILABLE, b"Server is shutting down.\n"] | list[Unknown]`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/exchange/binance_public_data.py:96:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `@Todo(generic `typing.Awaitable` type) | Series[Any]`
+ freqtrade/exchange/binance_public_data.py:96:16: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any] | Unknown`

paasta (https://github.com/yelp/paasta)
+ paasta_tools/contrib/get_running_task_allocation.py:102:17: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `str | None`
+ paasta_tools/instance/kubernetes.py:816:17: error[invalid-argument-type] Argument to function `get_replicaset_status` is incorrect: Expected `Sequence[Future[dict[str, Any]]]`, found `Sequence[Future[dict[str, Any]]] | None`
+ paasta_tools/instance/kubernetes.py:821:12: error[invalid-return-type] Return type does not match returned value: expected `list[KubernetesVersionDict]`, found `tuple[Unknown]`
+ paasta_tools/instance/kubernetes.py:1099:12: error[invalid-return-type] Return type does not match returned value: expected `list[KubernetesVersionDict]`, found `tuple[Unknown]`
- Found 881 diagnostics
+ Found 885 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
+ aiohttp/connector.py:1545:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `RequestInfo`, found `Unknown | under_cached_property[Unknown]`
+ aiohttp/connector.py:1546:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `tuple[ClientResponse, ...]`, found `Unknown | under_cached_property[Unknown]`
+ aiohttp/connector.py:1549:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MultiMapping[str] | None`, found `Unknown | under_cached_property[Unknown]`
+ aiohttp/web_middlewares.py:109:42: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | under_cached_property[Unknown]` and `Unknown | Literal[""]`
- Found 168 diagnostics
+ Found 172 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ test/mitmproxy/addons/test_clientplayback.py:18:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `def tcp_server(handle_conn, **server_args) -> Unknown | tuple[str, int]`
+ test/mitmproxy/addons/test_proxyserver.py:337:1: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `def udp_server(handle_datagram: (DatagramTransport, bytes, tuple[str, int], /) -> None) -> Unknown | tuple[str, int]`
- Found 1814 diagnostics
+ Found 1816 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/scripts/run_tool.py:49:12: error[invalid-return-type] Return type does not match returned value: expected `int`, found `int | None`
- Found 773 diagnostics
+ Found 774 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:406:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:407:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:500:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:501:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:542:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:543:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:592:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:593:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:638:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:639:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:669:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:670:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:703:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:704:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:757:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:758:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:795:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:796:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:835:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:836:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:892:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:893:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:936:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:937:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:963:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:964:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:988:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:989:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1018:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1064:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1113:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1165:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1306:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1342:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1544:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1545:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1587:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1588:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1618:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1619:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1656:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1657:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1659:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1660:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1703:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1704:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1706:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1707:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1731:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1733:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1735:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1761:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1762:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1764:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1792:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1794:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1796:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1829:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1830:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1832:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1834:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1836:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1868:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1870:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1872:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1902:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1904:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1906:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1967:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1969:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:1971:38: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2001:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2002:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2035:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2036:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2072:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2073:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2129:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2130:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2174:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2175:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2229:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2230:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2275:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2276:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2318:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2319:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2365:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2366:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2458:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2459:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2482:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2483:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2676:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2677:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2704:12: error[unresolved-attribute] Type `str` has no attribute `status_code`
+ src/integrations/prefect-aws/tests/workers/test_ecs_worker.py:2705:36: error[unresolved-attribute] Type `str` has no attribute `identifier`
- src/prefect/cli/deploy.py:688:13: error[invalid-argument-type] Argument to function `_generate_default_pull_action` is incorrect: Expected `list[dict[str, Any]]`, found `(dict[str, Any] & ~AlwaysFalsy) | dict[Unknown, Unknown] | @Todo(generic `typing.Awaitable` type)`
+ src/prefect/cli/deploy.py:688:13: error[invalid-argument-type] Argument to function `_generate_default_pull_action` is incorrect: Expected `list[dict[str, Any]]`, found `(dict[str, Any] & ~AlwaysFalsy) | dict[Unknown, Unknown] | dict[str, list[dict[str, Any]]]`
+ src/prefect/cli/work_pool.py:208:42: error[invalid-argument-type] Argument to function `provision` is incorrect: Expected `dict[str, Any]`, found `dict[str, Any] | None | Any`
+ src/prefect/cli/work_pool.py:226:17: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Any]`, found `dict[str, Any] | None | Any`
+ src/prefect/cli/work_pool.py:869:33: warning[possibly-unbound-attribute] Attribute `id` on type `BlockSchema | None` is possibly unbound
+ src/prefect/cli/worker.py:159:14: error[call-non-callable] Object of type `None` is not callable
- src/prefect/events/related.py:100:17: error[invalid-argument-type] Argument to function `_get_and_cache_related_object` is incorrect: Expected `(UUID | str, /) -> Awaitable[ObjectBaseModel | None]`, found `bound method PrefectClient.read_flow_run(flow_run_id: UUID) -> @Todo(generic types.CoroutineType)`
+ src/prefect/events/related.py:100:17: error[invalid-argument-type] Argument to function `_get_and_cache_related_object` is incorrect: Expected `(UUID | str, /) -> Awaitable[ObjectBaseModel | None]`, found `bound method PrefectClient.read_flow_run(flow_run_id: UUID) -> CoroutineType[Any, Any, FlowRun]`
- src/prefect/events/related.py:123:21: error[invalid-argument-type] Argument to function `_get_and_cache_related_object` is incorrect: Expected `(UUID | str, /) -> Awaitable[ObjectBaseModel | None]`, found `bound method PrefectClient.read_flow(flow_id: UUID) -> @Todo(generic types.CoroutineType)`
+ src/prefect/events/related.py:123:21: error[invalid-argument-type] Argument to function `_get_and_cache_related_object` is incorrect: Expected `(UUID | str, /) -> Awaitable[ObjectBaseModel | None]`, found `bound method PrefectClient.read_flow(flow_id: UUID) -> CoroutineType[Any, Any, Flow]`
- src/prefect/events/related.py:142:25: error[invalid-argument-type] Argument to function `_get_and_cache_related_object` is incorrect: Expected `(UUID | str, /) -> Awaitable[ObjectBaseModel | None]`, found `bound method PrefectClient.read_work_queue(id: UUID) -> @Todo(generic types.CoroutineType)`
+ src/prefect/events/related.py:142:25: error[invalid-argument-type] Argument to function `_get_and_cache_related_object` is incorrect: Expected `(UUID | str, /) -> Awaitable[ObjectBaseModel | None]`, found `bound method PrefectClient.read_work_queue(id: UUID) -> CoroutineType[Any, Any, WorkQueue]`
- src/prefect/events/related.py:153:25: error[invalid-argument-type] Argument to function `_get_and_cache_related_object` is incorrect: Expected `(UUID | str, /) -> Awaitable[ObjectBaseModel | None]`, found `bound method PrefectClient.read_work_pool(work_pool_name: str) -> @Todo(generic types.CoroutineType)`
+ src/prefect/events/related.py:153:25: error[invalid-argument-type] Argument to function `_get_and_cache_related_object` is incorrect: Expected `(UUID | str, /) -> Awaitable[ObjectBaseModel | None]`, found `bound method PrefectClient.read_work_pool(work_pool_name: str) -> CoroutineType[Any, Any, WorkPool]`
+ src/prefect/flow_runs.py:316:16: warning[possibly-unbound-attribute] Attribute `is_running` on type `State[Any] | None` is possibly unbound
+ src/prefect/flow_runs.py:478:16: warning[possibly-unbound-attribute] Attribute `is_paused` on type `State[Any] | None` is possibly unbound
+ src/prefect/flow_runs.py:484:12: warning[possibly-unbound-attribute] Attribute `type` on type `State[Any] | None` is possibly unbound
+ src/prefect/infrastructure/provisioners/cloud_run.py:238:37: warning[possibly-unbound-attribute] Attribute `id` on type `BlockSchema | None` is possibly unbound
+ src/prefect/infrastructure/provisioners/container_instance.py:731:37: warning[possibly-unbound-attribute] Attribute `id` on type `BlockSchema | None` is possibly unbound
+ src/prefect/server/api/artifacts.py:46:12: error[invalid-return-type] Return type does not match returned value: expected `Artifact`, found `Artifact`
+ src/prefect/server/events/actions.py:626:16: error[invalid-return-type] Return type does not match returned value: expected `list[str]`, found `tuple[Unknown]`
+ src/prefect/task_engine.py:1708:16: error[invalid-return-type] Return type does not match returned value: expected `R | State[Any] | None | Coroutine[Any, Any, R | State[Any] | None]`, found `AsyncGenerator[Unknown, None]`
- Found 3712 diagnostics
+ Found 3821 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~73MB
+     memo metadata = ~76MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-07-28 12:07_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 97 | 0 | 3 |
| `invalid-argument-type` | 10 | 1 | 7 |
| `invalid-return-type` | 11 | 1 | 1 |
| `unused-ignore-comment` | 0 | 8 | 0 |
| `possibly-unbound-attribute` | 6 | 0 | 0 |
| `call-non-callable` | 1 | 0 | 0 |
| `not-iterable` | 1 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **127** | **10** | **11** |

**[Full report with detailed diff](https://david-async-await.ecosystem-663.pages.dev/diff)**


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/signatures.rs`:332 on 2025-07-28 12:08_

a lot of the ecosystem report is because this isn't quite right if it's an async function that has a `yield` statement in it (which would make it an async generator). I can't remember the exact details of what the rules are for async generators here, but I remember that they're different to regular async functions...

---

_@AlexWaygood reviewed on 2025-07-28 12:08_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/function/return_type.md`:458 on 2025-07-28 12:16_

Ah, this happens because `i` is redefined below. This test should probably use subsections.

---

_@sharkdp reviewed on 2025-07-28 12:16_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-28 12:32_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-28 12:32_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-28 14:12_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-28 14:12_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-28 14:41_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-28 14:41_

---

_Renamed from "[ty] Support async/await" to "[ty] Support `async`/`await` and `yield from`" by @sharkdp on 2025-07-29 13:45_

---

_Renamed from "[ty] Support `async`/`await` and `yield from`" to "[ty] Support `async`/`await`, `async with` and `yield from`" by @sharkdp on 2025-07-29 13:45_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-29 13:50_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-29 13:50_

---

_Marked ready for review by @sharkdp on 2025-07-29 14:12_

---

_Review requested from @carljm by @sharkdp on 2025-07-29 14:12_

---

_Review requested from @dcreager by @sharkdp on 2025-07-29 14:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4835 on 2025-07-29 15:58_

We could probably implement a dumb version of `try_upcast()` (that would only work with explicit subclasses of `Generator`) that iterates over the MRO of a nominal instance (or a protocol instance) to see if it can find `typing.Generator` in there. That would at least return `Some()` for `types.GeneratorType`, since `types.GeneratorType` explicitly subclasses `typing.Generator` in typeshed (as of 29/07/2025, anyway)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:4800 on 2025-07-29 16:04_

or we could add an `is_async: bool` parameter to `try_enter`, and have it call `__aenter__` and `__aexit__` internally instead of `__enter__` if the parameter is `true`? We'd also need to propagate the asyncness into the `ContextManager` enum, of course, so that diagnostic messages don't mention theh wrong dunders

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2781 on 2025-07-29 16:06_

`CoroutineType` is marked as `@final`, so we'll anyway emit an error if anybody tries to subclass it. I think marking it as a solid base as well might just result in a second diagnostic, which seems unnecessary -- this is why most `@final` classes are listed in the branch below this one, even though some of them are strictly-speaking solid bases

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:2744 on 2025-07-29 16:08_

I think we can safely say that instances of `CoroutineType` are always truthy, since `CoroutineType` is an `@final` concrete class and `types.CoroutineType` doesn't define `__bool__` or `__len__` -- this should go in the first branch

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3678 on 2025-07-29 16:10_

huh? I don't think either of these are ever available from the `abc` module?

---

_@AlexWaygood reviewed on 2025-07-29 16:12_

Nice

---

_@AlexWaygood reviewed on 2025-07-29 16:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:355 on 2025-07-29 16:26_

Rather than threading the `is_generator` variable through, I think you could compute it inside `Signature::from_function`, e.g.

```diff
diff --git a/crates/ty_python_semantic/src/semantic_index/definition.rs b/crates/ty_python_semantic/src/semantic_index/definition.rs
index 6d1ce38299..63e107b777 100644
--- a/crates/ty_python_semantic/src/semantic_index/definition.rs
+++ b/crates/ty_python_semantic/src/semantic_index/definition.rs
@@ -8,6 +8,7 @@ use ruff_text_size::{Ranged, TextRange};
 use crate::Db;
 use crate::ast_node_ref::AstNodeRef;
 use crate::node_key::NodeKey;
+use crate::semantic_index::SemanticIndex;
 use crate::semantic_index::place::ScopedPlaceId;
 use crate::semantic_index::scope::{FileScopeId, ScopeId};
 use crate::semantic_index::symbol::ScopedSymbolId;
@@ -82,6 +83,10 @@ impl<'db> Definition<'db> {
             _ => None,
         }
     }
+
+    pub(crate) fn is_generator_function(self, db: &'db dyn Db, index: &SemanticIndex) -> bool {
+        self.file_scope(db).is_generator_function(index)
+    }
 }
 
 /// Extract a docstring from a function or class body.
diff --git a/crates/ty_python_semantic/src/types/function.rs b/crates/ty_python_semantic/src/types/function.rs
index 2b1c71ef05..d1c7140fd7 100644
--- a/crates/ty_python_semantic/src/types/function.rs
+++ b/crates/ty_python_semantic/src/types/function.rs
@@ -341,16 +341,12 @@ impl<'db> OverloadLiteral<'db> {
             GenericContext::from_type_params(db, index, type_params)
         });
 
-        let index = semantic_index(db, scope.file(db));
-        let is_generator = scope.file_scope_id(db).is_generator_function(index);
-
         Signature::from_function(
             db,
             generic_context,
             inherited_generic_context,
             definition,
             function_stmt_node,
-            is_generator,
         )
     }
 
diff --git a/crates/ty_python_semantic/src/types/signatures.rs b/crates/ty_python_semantic/src/types/signatures.rs
index 856cc15fff..02436420eb 100644
--- a/crates/ty_python_semantic/src/types/signatures.rs
+++ b/crates/ty_python_semantic/src/types/signatures.rs
@@ -17,6 +17,7 @@ use smallvec::{SmallVec, smallvec_inline};
 
 use super::{DynamicType, Type, TypeTransformer, TypeVarVariance, definition_expression_type};
 use crate::semantic_index::definition::Definition;
+use crate::semantic_index::semantic_index;
 use crate::types::generics::{GenericContext, walk_generic_context};
 use crate::types::{KnownClass, TypeMapping, TypeRelation, TypeVarInstance, todo_type};
 use crate::{Db, FxOrderSet};
@@ -320,14 +321,13 @@ impl<'db> Signature<'db> {
         inherited_generic_context: Option<GenericContext<'db>>,
         definition: Definition<'db>,
         function_node: &ast::StmtFunctionDef,
-        is_generator: bool,
     ) -> Self {
         let parameters =
             Parameters::from_parameters(db, definition, function_node.parameters.as_ref());
         let return_ty = function_node.returns.as_ref().map(|returns| {
             let plain_return_ty = definition_expression_type(db, definition, returns.as_ref());
-
-            if function_node.is_async && !is_generator {
+            let semantic_index = semantic_index(db, definition.file(db));
+            if function_node.is_async && !definition.is_generator_function(db, semantic_index) {
                 KnownClass::CoroutineType
                     .to_specialized_instance(db, [Type::any(), Type::any(), plain_return_ty])
             } else {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/yield_and_yield_from.md`:63 on 2025-07-29 16:50_

I suggest explicitly specifying `value` in the `raise StopIteration` statement in `OnceIterator.__next__` (I've given it the value `42`), and making the comment slightly more expansive:

```suggestion
        if self.returned:
            raise StopIteration(42)

        self.returned = True
        return self.value

class Once(Generic[T]):
    def __init__(self, value: T):
        self.value = value

    def __iter__(self) -> OnceIterator[T]:
        return OnceIterator(self.value)

for x in Once("a"):
    reveal_type(x)  # revealed: str

def generator() -> Generator:
    result = yield from Once("a")

    # At runtime, the value of `result` will be the `.value` attribute of the `StopIteration`
    # error raised by `OnceIterator` to signal to the interpreter that the iterator has been
    # exhausted. Here that will always be 42, but this information cannot be captured in the
    # signature of `OnceIterator.__next__`, since exceptions lie outside the type signature.
    # We therefore just infer `Unknown` here.
    #
    # If the `StopIteration` error in `OnceIterator.__next__` had been simply `raise StopIteration`
    # (the more common case), then the `.value` attribute of the `StopIteration` instance
    # would default to `None`.
    reveal_type(result)  # revealed: Unknown
```

---

_@AlexWaygood reviewed on 2025-07-29 16:53_

---

_@sharkdp reviewed on 2025-07-29 18:14_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:3678 on 2025-07-29 18:14_

Oops, I confused `abc` with `collections.abc`

---

_@AlexWaygood reviewed on 2025-07-29 18:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:3678 on 2025-07-29 18:19_

According to typeshed, the `collections.abc` classes are just re-exported from `typing`, so I don't think we need to worry about `collections.abc` either -- just `typing` should be fine for our purposes :-)

There are a number of other classes where we'd need to make changes if typeshed ever decided to start differentiating between `collections.abc` classes and their typing aliases -- `Iterator` and `Iterable`, fo rexample

---

_@sharkdp reviewed on 2025-07-30 08:43_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:4835 on 2025-07-30 08:43_

Thanks. Done. Added a test that shows that it now also works for generator functions annotated with `types.GeneratorType[…]`.

---

_@sharkdp reviewed on 2025-07-30 08:50_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:4800 on 2025-07-30 08:50_

This is one of the routes I'll explore when I add proper error handling, thanks.

---

_@sharkdp reviewed on 2025-07-30 08:54_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:355 on 2025-07-30 08:54_

That doesn't work. I think it uses the wrong scope ID. We need the ID of the function body, not the one of the definition.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-30 08:56_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-30 08:56_

---

_@AlexWaygood reviewed on 2025-07-30 09:45_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:355 on 2025-07-30 09:45_

Gah, I checked that it compiled and assumed the tests would therefore pass 😆 sorry!

---

_@AlexWaygood approved on 2025-07-30 09:45_

Thank you!

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:355 on 2025-07-30 09:47_

No worries — it looked good!

---

_@sharkdp reviewed on 2025-07-30 09:47_

---

_Merged by @sharkdp on 2025-07-30 09:51_

---

_Closed by @sharkdp on 2025-07-30 09:51_

---

_Branch deleted on 2025-07-30 09:51_

---
