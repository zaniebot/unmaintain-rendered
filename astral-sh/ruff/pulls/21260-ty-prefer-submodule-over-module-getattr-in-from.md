```yaml
number: 21260
title: "[ty] prefer submodule over module __getattr__ in from-imports"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/module_getattr
created_at: 2025-11-03T16:40:20Z
updated_at: 2025-11-03T20:24:03Z
url: https://github.com/astral-sh/ruff/pull/21260
synced_at: 2026-01-12T15:57:20Z
```

# [ty] prefer submodule over module __getattr__ in from-imports

---

_@carljm_

Fixes https://github.com/astral-sh/ty/issues/1053

## Summary

Other type checkers prioritize a submodule over a package `__getattr__` in `from mod import sub`, even though the runtime precedence is the other direction. In effect, this is making an implicit assumption that a module `__getattr__` will not handle (that is, will raise `AttributeError`) for names that are also actual submodules, rather than shadowing them. In practice this seems like a realistic assumption in the ecosystem? Or at least the ecosystem has adapted to it, and we need to adapt this precedence also, for ecosystem compatibility.

The implementation is a bit ugly, precisely because it departs from the runtime semantics, and our implementation is oriented toward modeling runtime semantics accurately. That is, `__getattr__` is modeled within the member-lookup code, so it's hard to split "member lookup result from module `__getattr__`" apart from other member lookup results. I did this via a synthetic `TypeQualifier::FROM_MODULE_GETATTR` that we attach to a type resulting from a member lookup, which isn't beautiful but it works well and doesn't introduce inefficiency (e.g. redundant member lookups).

## Test Plan

Updated mdtests.

Also added a related mdtest formalizing our support for a module `__getattr__` that is explicitly annotated to accept a limited set of names. In principle this could be an alternative (more explicit) way to handle the precedence problem without departing from runtime semantics, if the ecosystem would adopt it.

### Ecosystem analysis

Lots of removed diagnostics which are an improvement because we now infer the expected submodule.

Added diagnostics are mostly unrelated issues surfaced now because we previously had an earlier attribute error resulting in `Unknown`; now we correctly resolve the module so that earlier attribute error goes away, we get an actual type instead of `Unknown`, and that triggers a new error.

In scipy and sklearn, the module `__getattr__` which we were respecting previously is un-annotated so returned a forgiving `Unknown`; now we correctly see the actual module, which reveals some cases of https://github.com/astral-sh/ty/issues/133 that were previously hidden (`scipy/optimize/__init__.py` [imports `from ._tnc`](https://github.com/scipy/scipy/blob/eff82ca575668d2d7a4bc12b6afba98daaf6d5d0/scipy/optimize/__init__.py#L429).)



---

_Label `ty` added by @carljm on 2025-11-03 16:40_

---

_Comment by @github-actions[bot] on 2025-11-03 16:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-11-03 16:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
+ src/anyio/_backends/_asyncio.py:836:28: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_effectively_cancelled`
- src/anyio/_backends/_asyncio.py:696:26: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `TaskStatus`
+ src/anyio/_backends/_asyncio.py:854:40: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_host_task`
- src/anyio/_backends/_asyncio.py:720:17: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `TaskGroup`
+ src/anyio/_backends/_asyncio.py:857:28: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_host_task`
+ src/anyio/_backends/_asyncio.py:881:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `CancelScope | None`, found `CancelScope`
- src/anyio/_backends/_asyncio.py:1008:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `BlockingPortal`
- src/anyio/_backends/_asyncio.py:1037:27: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
- src/anyio/_backends/_asyncio.py:1053:27: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
- src/anyio/_backends/_asyncio.py:1084:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Process`
+ src/anyio/_backends/_asyncio.py:883:9: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_tasks`
- src/anyio/_backends/_asyncio.py:1129:24: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
- src/anyio/_backends/_asyncio.py:1133:25: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
- src/anyio/_backends/_asyncio.py:1137:25: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
- src/anyio/_backends/_asyncio.py:1166:55: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Process`
- src/anyio/_backends/_asyncio.py:1174:14: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Process`
- src/anyio/_backends/_asyncio.py:1261:20: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
- src/anyio/_backends/_asyncio.py:1392:41: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UNIXSocketStream`
- src/anyio/_backends/_asyncio.py:1508:25: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
- src/anyio/_backends/_asyncio.py:1521:31: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
- src/anyio/_backends/_asyncio.py:1568:26: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
- src/anyio/_backends/_asyncio.py:1575:31: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
- src/anyio/_backends/_asyncio.py:1605:17: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UDPSocket`
- src/anyio/_backends/_asyncio.py:1653:26: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUDPSocket`
- src/anyio/_backends/_asyncio.py:1703:43: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
- src/anyio/_backends/_asyncio.py:1739:52: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
- src/anyio/_backends/_asyncio.py:2169:18: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `TestRunner`
- src/anyio/_backends/_asyncio.py:2430:35: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `TaskGroup`
- src/anyio/_backends/_asyncio.py:2434:30: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Event`
- src/anyio/_backends/_asyncio.py:2438:52: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Lock`
- src/anyio/_backends/_asyncio.py:2448:10: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Semaphore`
- src/anyio/_backends/_asyncio.py:2452:62: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `CapacityLimiter`
- src/anyio/_backends/_asyncio.py:2461:18: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `CapacityLimiter`
- src/anyio/_backends/_asyncio.py:2591:40: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `BlockingPortal`
- src/anyio/_backends/_asyncio.py:2631:63: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Process`
- src/anyio/_backends/_asyncio.py:2643:10: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
- src/anyio/_backends/_asyncio.py:2654:55: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UNIXSocketStream`
- src/anyio/_backends/_asyncio.py:2708:10: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
- src/anyio/_backends/_asyncio.py:2708:35: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
- src/anyio/_backends/_trio.py:173:17: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `TaskGroup`
- src/anyio/_backends/_trio.py:177:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_trio.py:232:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `BlockingPortal`
- src/anyio/_backends/_trio.py:265:28: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
- src/anyio/_backends/_trio.py:286:25: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
- src/anyio/_backends/_trio.py:302:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Process`
- src/anyio/_backends/_trio.py:304:13: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
- src/anyio/_backends/_trio.py:305:14: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
- src/anyio/_backends/_trio.py:306:14: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
- src/anyio/_backends/_trio.py:346:24: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
- src/anyio/_backends/_trio.py:350:25: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
- src/anyio/_backends/_trio.py:354:25: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
- src/anyio/_backends/_trio.py:368:47: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Process`
+ src/anyio/_backends/_trio.py:938:9: warning[possibly-missing-attribute] Attribute `send_nowait` may be missing on object of type `MemoryObjectSendStream[Unknown] | None`
- src/anyio/_backends/_trio.py:417:38: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
- src/anyio/_backends/_trio.py:430:16: warning[possibly-unresolved-reference] Name `data` used when possibly not defined
- src/anyio/_backends/_trio.py:431:24: warning[possibly-unresolved-reference] Name `data` used when possibly not defined
- src/anyio/_backends/_trio.py:444:29: warning[possibly-unresolved-reference] Name `bytes_sent` used when possibly not defined
- src/anyio/_backends/_trio.py:450:38: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UNIXSocketStream`
- src/anyio/_backends/_trio.py:517:43: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
- src/anyio/_backends/_trio.py:529:9: warning[possibly-unresolved-reference] Name `trio_socket` used when possibly not defined
- src/anyio/_backends/_trio.py:530:29: warning[possibly-unresolved-reference] Name `trio_socket` used when possibly not defined
- src/anyio/_backends/_trio.py:533:44: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
- src/anyio/_backends/_trio.py:545:33: warning[possibly-unresolved-reference] Name `trio_socket` used when possibly not defined
- src/anyio/_backends/_trio.py:548:51: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UDPSocket`
- src/anyio/_backends/_trio.py:554:32: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `tuple[bytes, @Todo]`
- src/anyio/_backends/_trio.py:570:60: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUDPSocket`
- src/anyio/_backends/_trio.py:576:32: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `bytes`
- src/anyio/_backends/_trio.py:591:49: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
- src/anyio/_backends/_trio.py:614:28: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
- src/anyio/_backends/_trio.py:621:32: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `bytes`
- src/anyio/_backends/_trio.py:888:18: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `TestRunner`
- src/anyio/_backends/_trio.py:1035:10: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `CancelScope`
- src/anyio/_backends/_trio.py:1043:35: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `TaskGroup`
- src/anyio/_backends/_trio.py:1047:30: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Event`
- src/anyio/_backends/_trio.py:1061:10: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Semaphore`
- src/anyio/_backends/_trio.py:1074:18: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `CapacityLimiter`
- src/anyio/_backends/_trio.py:1118:40: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `BlockingPortal`
- src/anyio/_backends/_trio.py:1163:63: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `Process`
- src/anyio/_backends/_trio.py:1185:55: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UNIXSocketStream`
- src/anyio/_backends/_trio.py:1196:58: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
- src/anyio/_backends/_trio.py:1200:59: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
- src/anyio/_backends/_trio.py:1229:10: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
- src/anyio/_backends/_trio.py:1235:10: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
- src/anyio/_backends/_trio.py:1240:10: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
- src/anyio/_backends/_trio.py:1240:35: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
- src/anyio/_backends/_trio.py:1299:65: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
- src/anyio/_core/_fileio.py:90:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:93:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:96:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:99:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:102:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:105:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:108:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:117:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:128:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:131:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:134:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:137:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:140:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:187:16: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:209:25: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:383:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:467:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:471:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:475:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:479:19: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:487:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:495:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:499:27: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:506:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:511:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:516:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:519:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:522:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:530:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:538:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:541:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:556:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:559:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:564:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:594:20: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:600:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:603:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:608:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:625:24: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:632:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:639:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:644:27: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:651:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:657:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:663:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:673:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:676:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:680:19: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:721:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_fileio.py:737:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_sockets.py:880:19: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_sockets.py:882:23: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:96:20: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:205:20: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:327:26: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:499:31: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:502:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:511:19: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:517:19: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:557:18: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:592:18: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:604:18: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/_core/_tempfile.py:616:18: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/streams/file.py:40:15: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/streams/file.py:78:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/streams/file.py:83:26: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/streams/file.py:108:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/streams/file.py:120:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/streams/file.py:145:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/streams/file.py:150:19: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/streams/tls.py:156:32: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- src/anyio/to_interpreter.py:210:22: error[unresolved-attribute] Object of type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- Found 231 diagnostics
+ Found 85 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/meta.py:53:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/jinja2/optimizer.py:24:12: warning[redundant-cast] Value is already of type `Any`
+ src/jinja2/parser.py:429:9: error[invalid-assignment] Object of type `Expr` is not assignable to attribute `call` of type `Call`
+ src/jinja2/parser.py:1024:37: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Node]`, found `(Node & Top[list[Unknown]]) | list[Node]`
- src/jinja2/parser.py:435:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/jinja2/parser.py:506:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/jinja2/parser.py:794:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_loader.py:94:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[Any], Literal["one"]]` and `None`, in comparing `tuple[ReferenceType[Any], Literal["one"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
+ tests/test_loader.py:94:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[DictLoader], Literal["one"]]` and `None`, in comparing `tuple[ReferenceType[DictLoader], Literal["one"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
- tests/test_loader.py:95:16: error[unsupported-operator] Operator `not in` is not supported for types `tuple[ReferenceType[Any], Literal["two"]]` and `None`, in comparing `tuple[ReferenceType[Any], Literal["two"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
+ tests/test_loader.py:95:16: error[unsupported-operator] Operator `not in` is not supported for types `tuple[ReferenceType[DictLoader], Literal["two"]]` and `None`, in comparing `tuple[ReferenceType[DictLoader], Literal["two"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
- tests/test_loader.py:96:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[Any], Literal["three"]]` and `None`, in comparing `tuple[ReferenceType[Any], Literal["three"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
+ tests/test_loader.py:96:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[DictLoader], Literal["three"]]` and `None`, in comparing `tuple[ReferenceType[DictLoader], Literal["three"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
- tests/test_loader.py:282:9: error[invalid-assignment] Object of type `Any` is not assignable to attribute `loader` on type `Unknown | None | Environment`
+ tests/test_loader.py:282:9: error[invalid-assignment] Object of type `ChoiceLoader` is not assignable to attribute `loader` on type `Unknown | None | Environment`
+ tests/test_loader.py:283:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[BaseLoader]`, found `list[Unknown | BaseLoader | None]`
- tests/test_loader.py:292:9: error[invalid-assignment] Object of type `Any` is not assignable to attribute `loader` on type `Unknown | None | Environment`
+ tests/test_loader.py:292:9: error[invalid-assignment] Object of type `PrefixLoader` is not assignable to attribute `loader` on type `Unknown | None | Environment`
+ tests/test_loader.py:293:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, BaseLoader]`, found `dict[Unknown | str, Unknown | BaseLoader | None]`
- Found 184 diagnostics
+ Found 183 diagnostics

black (https://github.com/psf/black)
- src/blackd/__init__.py:86:5: error[unresolved-attribute] Object of type `object` has no attribute `run_app`
- src/blackd/__init__.py:94:19: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- src/blackd/__init__.py:95:11: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- src/blackd/__init__.py:98:21: error[unresolved-attribute] Object of type `object` has no attribute `post`
- src/blackd/__init__.py:102:27: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- src/blackd/__init__.py:102:63: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- src/blackd/__init__.py:106:20: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- src/blackd/__init__.py:116:20: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- src/blackd/__init__.py:158:16: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- src/blackd/__init__.py:165:16: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- src/blackd/__init__.py:167:16: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- src/blackd/__init__.py:170:16: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- Found 67 diagnostics
+ Found 55 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:79:26: error[unresolved-attribute] Object of type `object` has no attribute `Discriminator`
- pydantic/fields.py:165:26: error[unresolved-attribute] Object of type `object` has no attribute `Discriminator`
- pydantic/fields.py:211:19: error[unresolved-attribute] Object of type `object` has no attribute `Strict`
- pydantic/fields.py:225:22: error[unresolved-attribute] Object of type `object` has no attribute `FailFast`
- pydantic/fields.py:937:26: error[unresolved-attribute] Object of type `object` has no attribute `Discriminator`
+ pydantic/fields.py:937:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- pydantic/fields.py:977:26: error[unresolved-attribute] Object of type `object` has no attribute `Discriminator`
+ pydantic/fields.py:977:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- pydantic/fields.py:1020:26: error[unresolved-attribute] Object of type `object` has no attribute `Discriminator`
+ pydantic/fields.py:1020:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- pydantic/fields.py:1060:26: error[unresolved-attribute] Object of type `object` has no attribute `Discriminator`
+ pydantic/fields.py:1060:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- pydantic/fields.py:1103:26: error[unresolved-attribute] Object of type `object` has no attribute `Discriminator`
+ pydantic/fields.py:1103:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- pydantic/fields.py:1142:26: error[unresolved-attribute] Object of type `object` has no attribute `Discriminator`
+ pydantic/fields.py:1142:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- pydantic/fields.py:1182:26: error[unresolved-attribute] Object of type `object` has no attribute `Discriminator`
+ pydantic/fields.py:1182:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
- Found 775 diagnostics
+ Found 771 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- aiohttp_devtools/runserver/config.py:16:20: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/config.py:16:50: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/config.py:16:91: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/config.py:205:33: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/config.py:233:58: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/config.py:234:36: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/config.py:242:32: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/log_handlers.py:16:32: error[unresolved-attribute] Object of type `object` has no attribute `BaseRequest`
- aiohttp_devtools/runserver/log_handlers.py:16:59: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/log_handlers.py:19:30: error[unresolved-attribute] Object of type `object` has no attribute `BaseRequest`
- aiohttp_devtools/runserver/log_handlers.py:19:57: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/log_handlers.py:22:28: error[unresolved-attribute] Object of type `object` has no attribute `BaseRequest`
- aiohttp_devtools/runserver/log_handlers.py:22:55: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/log_handlers.py:43:32: error[unresolved-attribute] Object of type `object` has no attribute `BaseRequest`
- aiohttp_devtools/runserver/log_handlers.py:43:59: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/log_handlers.py:52:30: error[unresolved-attribute] Object of type `object` has no attribute `BaseRequest`
- aiohttp_devtools/runserver/log_handlers.py:52:57: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/log_handlers.py:57:59: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- aiohttp_devtools/runserver/log_handlers.py:72:32: error[unresolved-attribute] Object of type `object` has no attribute `BaseRequest`
- aiohttp_devtools/runserver/log_handlers.py:72:59: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/serve.py:38:15: error[unresolved-attribute] Object of type `object` has no attribute `AppKey`
- aiohttp_devtools/runserver/serve.py:39:21: error[unresolved-attribute] Object of type `object` has no attribute `AppKey`
- aiohttp_devtools/runserver/serve.py:40:15: error[unresolved-attribute] Object of type `object` has no attribute `AppKey`
- aiohttp_devtools/runserver/serve.py:41:14: error[unresolved-attribute] Object of type `object` has no attribute `AppKey`
- aiohttp_devtools/runserver/serve.py:42:6: error[unresolved-attribute] Object of type `object` has no attribute `AppKey`
- aiohttp_devtools/runserver/serve.py:42:33: error[unresolved-attribute] Object of type `object` has no attribute `WebSocketResponse`
- aiohttp_devtools/runserver/serve.py:45:26: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/serve.py:55:29: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/serve.py:65:26: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/serve.py:74:27: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- aiohttp_devtools/runserver/serve.py:81:39: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- aiohttp_devtools/runserver/serve.py:81:62: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/serve.py:82:42: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- aiohttp_devtools/runserver/serve.py:94:10: error[unresolved-attribute] Object of type `object` has no attribute `middleware`
- aiohttp_devtools/runserver/serve.py:95:48: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- aiohttp_devtools/runserver/serve.py:95:82: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/serve.py:106:10: error[unresolved-attribute] Object of type `object` has no attribute `middleware`
- aiohttp_devtools/runserver/serve.py:107:46: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- aiohttp_devtools/runserver/serve.py:107:80: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/serve.py:117:40: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- aiohttp_devtools/runserver/serve.py:117:56: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- aiohttp_devtools/runserver/serve.py:121:20: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- aiohttp_devtools/runserver/serve.py:197:71: error[unresolved-attribute] Object of type `object` has no attribute `AppRunner`
- aiohttp_devtools/runserver/serve.py:202:12: error[unresolved-attribute] Object of type `object` has no attribute `AppRunner`
- aiohttp_devtools/runserver/serve.py:205:34: error[unresolved-attribute] Object of type `object` has no attribute `AppRunner`
- aiohttp_devtools/runserver/serve.py:207:12: error[unresolved-attribute] Object of type `object` has no attribute `TCPSite`
- aiohttp_devtools/runserver/serve.py:211:27: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/serve.py:257:32: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/serve.py:264:41: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/serve.py:265:11: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/serve.py:266:19: error[unresolved-attribute] Object of type `object` has no attribute `WebSocketResponse`
- aiohttp_devtools/runserver/serve.py:294:34: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- aiohttp_devtools/runserver/serve.py:294:50: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- aiohttp_devtools/runserver/serve.py:299:12: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- aiohttp_devtools/runserver/serve.py:305:38: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- aiohttp_devtools/runserver/serve.py:305:54: error[unresolved-attribute] Object of type `object` has no attribute `WebSocketResponse`
- aiohttp_devtools/runserver/serve.py:306:10: error[unresolved-attribute] Object of type `object` has no attribute `WebSocketResponse`
- aiohttp_devtools/runserver/serve.py:361:39: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- aiohttp_devtools/runserver/serve.py:387:40: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/serve.py:387:63: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/serve.py:388:37: error[unresolved-attribute] Object of type `object` has no attribute `FileResponse`
- aiohttp_devtools/runserver/serve.py:399:16: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- aiohttp_devtools/runserver/serve.py:405:40: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/serve.py:406:10: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/serve.py:413:59: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/serve.py:424:16: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- aiohttp_devtools/runserver/serve.py:426:38: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- aiohttp_devtools/runserver/serve.py:426:54: error[unresolved-attribute] Object of type `object` has no attribute `StreamResponse`
- aiohttp_devtools/runserver/watch.py:23:11: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/watch.py:29:32: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- aiohttp_devtools/runserver/watch.py:45:38: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- tests/cleanup_app.py:24:12: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- tests/cleanup_app.py:41:7: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- tests/test_runserver_config.py:62:28: error[unresolved-attribute] Object of type `object` has no attribute `Application`
- tests/test_runserver_logs.py:100:30: error[unresolved-attribute] Object of type `object` has no attribute `Request`
- tests/test_runserver_logs.py:105:31: error[unresolved-attribute] Object of type `object` has no attribute `Response`
- Found 120 diagnostics
+ Found 44 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
+ mkdocs/tests/utils/templates_tests.py:36:85: error[invalid-argument-type] Argument to function `script_tag_filter` is incorrect: Expected `ExtraScriptValue`, found `Unknown | ExtraScriptValue | str`
- Found 220 diagnostics
+ Found 221 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/application.py:1171:38: error[invalid-argument-type] Argument to function `register_role` is incorrect: Expected `RoleFunction`, found `GenericRole`
- sphinx/ext/autosummary/generate.py:962:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/linear_model/tests/test_sgd.py:73:16: error[unresolved-attribute] Class `SGDRegressor` has no attribute `decision_function`
+ sklearn/tree/tests/test_tree.py:616:44: error[unresolved-attribute] Module `sklearn.tree` has no member `_tree`
+ sklearn/tree/tests/test_tree.py:647:44: error[unresolved-attribute] Module `sklearn.tree` has no member `_tree`
+ sklearn/tree/tests/test_tree.py:1144:43: error[unresolved-attribute] Module `sklearn.tree` has no member `_tree`
+ sklearn/tree/tests/test_tree.py:1640:32: error[unresolved-attribute] Module `sklearn.tree` has no member `_tree`
+ sklearn/tree/tests/test_tree.py:1650:46: error[unresolved-attribute] Module `sklearn.tree` has no member `_tree`
+ sklearn/tree/tests/test_tree.py:1970:35: error[unresolved-attribute] Module `sklearn.tree` has no member `_tree`
+ sklearn/tree/tests/test_tree.py:1976:59: error[unresolved-attribute] Module `sklearn.tree` has no member `_tree`
+ sklearn/tree/tests/test_tree.py:1985:68: error[unresolved-attribute] Module `sklearn.tree` has no member `_tree`
- Found 2538 diagnostics
+ Found 2547 diagnostics

AutoSplit (https://github.com/Toufool/AutoSplit)
- src/AutoControlledThread.py:14:28: error[unresolved-attribute] Object of type `list[str]` has no attribute `QThread`
- src/AutoControlledThread.py:19:6: error[unresolved-attribute] Object of type `list[str]` has no attribute `Slot`
- src/AutoSplit.py:97:34: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:98:20: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:99:25: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:100:25: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:101:20: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:102:25: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:103:35: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:104:36: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:105:33: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:107:25: error[unresolved-attribute] Object of type `list[str]` has no attribute `Signal`
- src/AutoSplit.py:110:24: error[unresolved-attribute] Object of type `list[str]` has no attribute `QTimer`
- src/AutoSplit.py:111:35: error[unresolved-attribute] Object of type `list[str]` has no attribute `Qt`
- src/AutoSplit.py:112:25: error[unresolved-attribute] Object of type `list[str]` has no attribute `QTimer`
- src/AutoSplit.py:113:36: error[unresolved-attribute] Object of type `list[str]` has no attribute `Qt`
- src/AutoSplit.py:118:28: error[unresolved-attribute] Object of type `list[str]` has no attribute `QThread`
- src/AutoSplit.py:980:33: error[unresolved-attribute] Object of type `list[str]` has no attribute `QCloseEvent`
- src/AutoSplit.py:1027:9: warning[possibly-missing-attribute] Attribute `ignore` may be missing on object of type `Unknown | None`
+ src/AutoSplit.py:1027:9: warning[possibly-missing-attribute] Attribute `ignore` may be missing on object of type `QCloseEvent | None`
- src/AutoSplit.py:1039:28: error[unresolved-attribute] Object of type `list[str]` has no attribute `QImage`
- src/AutoSplit.py:1042:28: error[unresolved-attribute] Object of type `list[str]` has no attribute `QImage`
- src/AutoSplit.py:1045:18: error[unresolved-attribute] Object of type `list[str]` has no attribute `QImage`
- src/AutoSplit.py:1047:13: error[unresolved-attribute] Object of type `list[str]` has no attribute `QPixmap`
- src/AutoSplit.py:1049:17: error[unresolved-attribute] Object of type `list[str]` has no attribute `Qt`
- src/AutoSplit.py:1050:17: error[unresolved-attribute] Object of type `list[str]` has no attribute `Qt`
- src/AutoSplit.py:1078:27: error[unresolved-attribute] Object of type `list[str]` has no attribute `QIcon`
- src/AutoSplit.py:1093:21: error[unresolved-attribute] Object of type `list[str]` has no attribute `QTimer`
- src/error_messages.py:32:19: error[unresolved-attribute] Object of type `list[str]` has no attribute `QMessageBox`
- src/error_messages.py:34:31: error[unresolved-attribute] Object of type `list[str]` has no attribute `Qt`
- src/error_messages.py:38:46: error[unresolved-attribute] Object of type `list[str]` has no attribute `QMessageBox`
- src/error_messages.py:42:13: error[unresolved-attribute] Object of type `list[str]` has no attribute `QMessageBox`
- src/error_messages.py:49:50: error[unresolved-attribute] Object of type `list[str]` has no attribute `QMessageBox`
- src/hotkeys.py:305:14: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/menu_bar.py:52:21: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/menu_bar.py:65:42: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/menu_bar.py:69:29: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/menu_bar.py:110:17: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/menu_bar.py:123:31: error[unresolved-attribute] Object of type `list[str]` has no attribute `QThread`
- src/menu_bar.py:163:24: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/menu_bar.py:322:17: error[unresolved-attribute] Object of type `list[str]` has no attribute `QRect`
- src/menu_bar.py:341:27: error[unresolved-attribute] Object of type `list[str]` has no attribute `QLineEdit`
- src/menu_bar.py:342:39: error[unresolved-attribute] Object of type `list[str]` has no attribute `QPushButton`
- src/menu_bar.py:360:23: error[unresolved-attribute] Object of type `list[str]` has no attribute `QCheckBox`
- src/menu_bar.py:467:45: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/menu_bar.py:472:19: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/region_selection.py:215:25: error[unresolved-attribute] Object of type `list[str]` has no attribute `QFileDialog`
- src/region_selection.py:317:24: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/region_selection.py:340:29: error[unresolved-attribute] Object of type `list[str]` has no attribute `Qt`
- src/region_selection.py:344:36: error[unresolved-attribute] Object of type `list[str]` has no attribute `QKeyEvent`
- src/region_selection.py:345:27: error[unresolved-attribute] Object of type `list[str]` has no attribute `Qt`
- src/region_selection.py:353:40: error[unresolved-attribute] Object of type `list[str]` has no attribute `QMouseEvent`
- src/region_selection.py:366:15: error[unresolved-attribute] Object of type `list[str]` has no attribute `QPoint`
- src/region_selection.py:367:13: error[unresolved-attribute] Object of type `list[str]` has no attribute `QPoint`
- src/region_selection.py:370:9: error[unresolved-attribute] Object of type `list[str]` has no attribute `QApplication`
- src/region_selection.py:370:50: error[unresolved-attribute] Object of type `list[str]` has no attribute `QCursor`
- src/region_selection.py:370:64: error[unresolved-attribute] Object of type `list[str]` has no attribute `Qt`
- src/region_selection.py:374:33: error[unresolved-attribute] Object of type `list[str]` has no attribute `QPaintEvent`
- src/region_selection.py:376:24: error[unresolved-attribute] Object of type `list[str]` has no attribute `QPainter`
- src/region_selection.py:377:29: error[unresolved-attribute] Object of type `list[str]` has no attribute `QPen`
- src/region_selection.py:377:40: error[unresolved-attribute] Object of type `list[str]` has no attribute `QColor`
- src/region_selection.py:378:31: error[unresolved-attribute] Object of type `list[str]` has no attribute `QColor`
- src/region_selection.py:379:31: error[unresolved-attribute] Object of type `list[str]` has no attribute `QRect`
- src/region_selection.py:382:38: error[unresolved-attribute] Object of type `list[str]` has no attribute `QMouseEvent`
- src/region_selection.py:388:37: error[unresolved-attribute] Object of type `list[str]` has no attribute `QMouseEvent`
- src/region_selection.py:393:40: error[unresolved-attribute] Object of type `list[str]` has no attribute `QMouseEvent`
- src/region_selection.py:408:9: error[unresolved-attribute] Object of type `list[str]` has no attribute `QApplication`
- src/region_selection.py:408:50: error[unresolved-attribute] Object of type `list[str]` has no attribute `QCursor`
- src/region_selection.py:408:64: error[unresolved-attribute] Object of type `list[str]` has no attribute `Qt`
- src/user_profile.py:101:31: error[unresolved-attribute] Object of type `list[str]` has no attribute `QFileDialog`
- src/user_profile.py:132:28: error[unresolved-attribute] Object of type `list[str]` has no attribute `QWidget`
- src/user_profile.py:188:12: error[unresolved-attribute] Object of type `list[str]` has no attribute `QFileDialog`
- src/user_profile.py:236:9: error[unresolved-attribute] Object of type `list[str]` has no attribute `QSettings`
- src/user_profile.py:251:5: error[unresolved-attribute] Object of type `list[str]` has no attribute `QSettings`
- Found 141 diagnostics
+ Found 69 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/client.py:273:32: error[unresolved-attribute] Object of type `object` has no attribute `HttpVersion11`
- aiohttp/client.py:473:20: error[unresolved-attribute] Object of type `object` has no attribute `JsonPayload`
- aiohttp/client.py:1464:32: error[unresolved-attribute] Object of type `object` has no attribute `HttpVersion11`
- aiohttp/client_reqrep.py:346:40: error[unresolved-attribute] Object of type `object` has no attribute `parse_content_disposition`
- aiohttp/client_reqrep.py:348:20: error[unresolved-attribute] Object of type `object` has no attribute `content_disposition_filename`
- aiohttp/client_reqrep.py:955:19: error[unresolved-attribute] Object of type `object` has no attribute `PAYLOAD_REGISTRY`
- aiohttp/client_reqrep.py:1029:23: error[unresolved-attribute] Object of type `object` has no attribute `Payload`
+ aiohttp/client_reqrep.py:1162:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Any]`, found `(Any & ~None & ~FormData) | Payload`
- aiohttp/client_reqrep.py:1155:24: error[unresolved-attribute] Object of type `object` has no attribute `PAYLOAD_REGISTRY`
- aiohttp/client_reqrep.py:1156:20: error[unresolved-attribute] Object of type `object` has no attribute `LookupError`
- aiohttp/connector.py:411:36: error[unresolved-attribute] Object of type `object` has no attribute `weakref_handle`
- aiohttp/connector.py:434:43: error[unresolved-attribute] Object of type `object` has no attribute `weakref_handle`
- aiohttp/connector.py:754:36: error[unresolved-attribute] Object of type `object` has no attribute `weakref_handle`
- aiohttp/formdata.py:31:24: error[unresolved-attribute] Object of type `object` has no attribute `MultipartWriter`
- aiohttp/formdata.py:102:39: error[unresolved-attribute] Object of type `object` has no attribute `BytesPayload`
- aiohttp/formdata.py:117:16: error[unresolved-attribute] Object of type `object` has no attribute `BytesPayload`
- aiohttp/formdata.py:122:33: error[unresolved-attribute] Object of type `object` has no attribute `MultipartWriter`
- aiohttp/formdata.py:127:28: error[unresolved-attribute] Object of type `object` has no attribute `get_payload`
- aiohttp/formdata.py:134:28: error[unresolved-attribute] Object of type `object` has no attribute `get_payload`
- aiohttp/web_response.py:593:37: error[unresolved-attribute] Object of type `object` has no attribute `PAYLOAD_REGISTRY`
- aiohttp/web_response.py:593:37: error[unresolved-attribute] Object of type `object` has no attribute `PAYLOAD_REGISTRY`
- aiohttp/web_response.py:594:20: error[unresolved-attribute] Object of type `object` has no attribute `LookupError`
+ aiohttp/worker.py:105:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SSLContext | None`, found `object`
- aiohttp/worker.py:71:33: error[unresolved-attribute] Object of type `object` has no attribute `AppRunner`
- aiohttp/worker.py:84:22: error[unresolved-attribute] Object of type `object` has no attribute `AppRunner`
- aiohttp/worker.py:102:20: error[unresolved-attribute] Object of type `object` has no attribute `SockSite`
- Found 181 diagnostics
+ Found 159 diagnostics

altair (https://github.com/vega/altair)
- altair/vegalite/v6/api.py:262:30: error[invalid-argument-type] Argument to function `_dataset_name` is incorrect: Expected `dict[str, Any] | list[Unknown] | InlineDataset`, found `UndefinedType | @Todo`
+ altair/vegalite/v6/api.py:2156:49: error[invalid-argument-type] Argument to function `compile_with_vegafusion` is incorrect: Expected `dict[str, Any]`, found `Unknown | MutableMapping[Any, Any]`
+ altair/vegalite/v6/api.py:2164:20: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `Unknown | MutableMapping[Any, Any]`
- Found 1037 diagnostics
+ Found 1038 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ tests/internal/crashtracker/test_crashtracker.py:64:29: error[invalid-argument-type] Argument to function `start` is incorrect: Expected `dict[str, str] | None`, found `dict[Unknown | str, Unknown | bytes]`
- Found 8155 diagnostics
+ Found 8156 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- test/scalability/lib.py:26:11: error[unresolved-attribute] Object of type `__getattr__` has no attribute `figure`
- test/scalability/mzcompose.py:450:15: error[unresolved-attribute] Object of type `__getattr__` has no attribute `figure`
- test/scalability/mzcompose.py:462:9: error[unresolved-attribute] Object of type `__getattr__` has no attribute `savefig`
- test/scalability/mzcompose.py:463:9: error[unresolved-attribute] Object of type `__getattr__` has no attribute `close`
- test/scalability/mzcompose.py:469:15: error[unresolved-attribute] Object of type `__getattr__` has no attribute `figure`
- test/scalability/mzcompose.py:478:9: error[unresolved-attribute] Object of type `__getattr__` has no attribute `savefig`
- test/scalability/mzcompose.py:483:9: error[unresolved-attribute] Object of type `__getattr__` has no attribute `close`
- test/scalability/mzcompose.py:485:15: error[unresolved-attribute] Object of type `__getattr__` has no attribute `figure`
- test/scalability/mzcompose.py:494:9: error[unresolved-attribute] Object of type `__getattr__` has no attribute `savefig`
- test/scalability/mzcompose.py:499:9: error[unresolved-attribute] Object of type `__getattr__` has no attribute `close`
- Found 3380 diagnostics
+ Found 3370 diagnostics

scipy (https://github.com/scipy/scipy)
+ scipy/differentiate/tests/test_differentiate.py:35:38: error[unresolved-attribute] Module `scipy.stats` has no member `_distr_params`
+ scipy/integrate/_quadrature.py:1170:11: warning[possibly-missing-attribute] Attribute `_qmc` may be missing on object of type `<module 'scipy.stats'> | Unknown`
+ scipy/integrate/tests/test_tanhsinh.py:258:38: error[unresolved-attribute] Module `scipy.stats` has no member `_distr_params`
+ scipy/linalg/tests/test_batch.py:224:15: error[unresolved-attribute] Module `scipy.linalg` has no member `qr_update`
+ scipy/linalg/tests/test_batch.py:228:21: error[unresolved-attribute] Module `scipy.linalg` has no member `qr_update`
+ scipy/linalg/tests/test_batch.py:241:15: error[unresolved-attribute] Module `scipy.linalg` has no member `qr_insert`
+ scipy/linalg/tests/test_batch.py:246:21: error[unresolved-attribute] Module `scipy.linalg` has no member `qr_insert`
+ scipy/linalg/tests/test_batch.py:257:15: error[unresolved-attribute] Module `scipy.linalg` has no member `qr_delete`
+ scipy/linalg/tests/test_batch.py:261:21: error[unresolved-attribute] Module `scipy.linalg` has no member `qr_delete`
+ scipy/ndimage/tests/test_filters.py:2208:11: error[unresolved-attribute] Module `scipy.ndimage` has no member `_ni_support`
+ scipy/ndimage/tests/test_filters.py:2211:11: error[unresolved-attribute] Module `scipy.ndimage` has no member `_ni_support`
+ scipy/ndimage/tests/test_filters.py:2215:11: error[unresolved-attribute] Module `scipy.ndimage` has no member `_ni_support`
+ scipy/ndimage/tests/test_filters.py:2219:11: error[unresolved-attribute] Module `scipy.ndimage` has no member `_ni_support`
+ scipy/ndimage/tests/test_filters.py:2228:5: error[unresolved-attribute] Module `scipy.ndimage` has no member `_ni_support`
+ scipy/optimize/tests/test_bracket.py:123:16: error[unresolved-attribute] Module `scipy.stats` has no member `_stats_py`
+ scipy/optimize/tests/test_optimize.py:713:20: error[unresolved-attribute] Module `scipy.optimize` has no member `_optimize`
+ scipy/optimize/tests/test_optimize.py:721:24: error[unresolved-attribute] Module `scipy.optimize` has no member `_optimize`
+ scipy/optimize/tests/test_optimize.py:724:20: error[unresolved-attribute] Module `scipy.optimize` has no member `_optimize`
+ scipy/optimize/tests/test_optimize.py:742:11: error[unresolved-attribute] Module `scipy.optimize` has no member `_minimize`
+ scipy/optimize/tests/test_optimize.py:763:5: error[unresolved-attribute] Module `scipy.optimize` has no member `_minimize`
+ scipy/optimize/tests/test_optimize.py:2235:23: error[unresolved-attribute] Module `scipy.optimize` has no member `_optimize`
+ scipy/optimize/tests/test_optimize.py:2295:25: error[unresolved-attribute] Module `scipy.optimize` has no member `_optimize`
+ scipy/optimize/tests/test_optimize.py:2333:25: error[unresolved-attribute] Module `scipy.optimize` has no member `_optimize`
+ scipy/optimize/tests/test_tnc.py:198:48: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:203:33: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:211:48: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:216:33: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:224:48: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:229:33: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:236:48: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:241:33: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:248:48: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:253:33: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:260:48: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:265:33: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:272:48: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:277:33: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:284:48: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:289:33: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:297:48: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/optimize/tests/test_tnc.py:302:33: error[unresolved-attribute] Module `scipy.optimize` has no member `_tnc`
+ scipy/signal/tests/test_signaltools.py:1000:29: error[unresolved-attribute] Module `scipy.signal` has no member `_signaltools`
+ scipy/signal/tests/test_signaltools.py:1031:29: error[unresolved-attribute] Module `scipy.signal` has no member `_signaltools`
+ scipy/signal/tests/test_signaltools.py:1054:29: error[unresolved-attribute] Module `scipy.signal` has no member `_signaltools`
+ scipy/signal/tests/test_signaltools.py:1089:29: error[unresolved-attribute] Module `scipy.signal` has no member `_signaltools`
+ scipy/signal/tests/test_signaltools.py:3169:43: error[unresolved-attribute] Object of type `dlti` has no attribute `num`
+ scipy/signal/tests/test_signaltools.py:3169:55: error[unresolved-attribute] Object of type `dlti` has no attribute `den`
+ scipy/signal/tests/test_signaltools.py:3233:34: error[too-many-positional-arguments] Too many positional arguments to function `lfilter`: expected 5, got 6
+ scipy/signal/tests/test_signaltools.py:3239:34: error[too-many-positional-arguments] Too many positional arguments to function `filtfilt`: expected 8, got 9
+ scipy/signal/tests/test_spectral.py:346:62: error[invalid-argument-type] Argument to function `detrend` is incorrect: Expected `Literal["linear", "constant"]`, found `Literal["l"]`
+ scipy/signal/tests/test_spectral.py:353:62: error[invalid-argument-type] Argument to function `detrend` is incorrect: Expected `Literal["linear", "constant"]`, found `Literal["l"]`
+ scipy/signal/tests/test_spectral.py:361:70: error[invalid-argument-type] Argument to function `detrend` is incorrect: Expected `Literal["linear", "constant"]`, found `Literal["l"]`
+ scipy/signal/tests/test_spectral.py:726:60: error[invalid-argument-type] Argument to function `detrend` is incorrect: Expected `Literal["linear", "constant"]`, found `Literal["l"]`
+ scipy/signal/tests/test_spectral.py:733:60: error[invalid-argument-type] Argument to function `detrend` is incorrect: Expected `Literal["linear", "constant"]`, found `Literal["l"]`
+ scipy/signal/tests/test_spectral.py:741:68: error[invalid-argument-type] Argument to function `detrend` is incorrect: Expected `Literal["linear", "constant"]`, found `Literal["l"]`
+ scipy/stats/_sampling.py:625:29: error[unresolved-attribute] Module `scipy.stats` has no member `distributions`
+ scipy/stats/_survival.py:683:12: error[unresolved-attribute] Module `scipy.stats` has no member `_stats_py`
+ scipy/stats/_survival.py:684:14: error[unresolved-attribute] Module `scipy.stats` has no member `_stats_py`
+ scipy/stats/tests/test_axis_nan_policy.py:45:12: error[unresolved-attribute] Module `scipy.stats` has no member `_stats_py`
+ scipy/stats/tests/test_axis_nan_policy.py:51:12: error[unresolved-attribute] Module `scipy.stats` has no member `_stats_py`
+ scipy/stats/tests/test_axis_nan_policy.py:56:12: error[unresolved-attribute] Module `scipy.stats` has no member `_stats_py`
+ scipy/stats/tests/test_axis_nan_policy.py:78:16: error[unresolved-attribute] Module `scipy.stats` has no member `_morestats`
+ scipy/stats/tests/test_axis_nan_policy.py:84:16: error[unresolved-attribute] Module `scipy.stats` has no member `_morestats`
+ scipy/stats/tests/test_axis_nan_policy.py:829:26: error[unresolved-attribute] Module `scipy.stats` has no member `_axis_nan_policy`
+ scipy/stats/tests/test_axis_nan_policy.py:918:26: error[unresolved-attribute] Module `scipy.stats` has no member `_axis_nan_policy`
+ scipy/stats/tests/test_axis_nan_policy.py:960:21: error[unresolved-attribute] Module `scipy.stats` has no member `_axis_nan_policy`
+ scipy/stats/tests/test_continuous.py:1087:16: error[unresolved-attribute] Object of type `Normal` has no attribute `mu`
+ scipy/stats/tests/test_continuous.py:1088:16: error[unresolved-attribute] Object of type `Normal` has no attribute `sigma`
+ scipy/stats/tests/test_continuous.py:1089:16: error[unresolved-attribute] Object of type `Normal` has no attribute `mu`
+ scipy/stats/tests/test_continuous.py:1090:16: error[unr...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-03 16:44_

---

_Comment by @github-actions[bot] on 2025-11-03 16:51_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 85 | 347 | 0 |
| `invalid-argument-type` | 16 | 1 | 0 |
| `invalid-parameter-default` | 7 | 0 | 0 |
| `possibly-unresolved-reference` | 0 | 6 | 0 |
| `unused-ignore-comment` | 0 | 6 | 0 |
| `invalid-return-type` | 1 | 3 | 0 |
| `invalid-assignment` | 1 | 0 | 2 |
| `possibly-missing-attribute` | 2 | 0 | 1 |
| `unsupported-operator` | 0 | 0 | 3 |
| `too-many-positional-arguments` | 2 | 0 | 0 |
| `redundant-cast` | 0 | 1 | 0 |
| **Total** | **114** | **364** | **6** |

**[Full report with detailed diff](https://cjm-module-getattr.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-module-getattr.ecosystem-663.pages.dev/timing))


---

_Marked ready for review by @carljm on 2025-11-03 18:12_

---

_Review requested from @AlexWaygood by @carljm on 2025-11-03 18:12_

---

_Review requested from @sharkdp by @carljm on 2025-11-03 18:12_

---

_Review requested from @dcreager by @carljm on 2025-11-03 18:12_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:98 on 2025-11-03 18:41_

```suggestion
If you `from mod import sub`, at runtime `sub` will be the value returned by the module
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5442 on 2025-11-03 18:57_

why are we using `DeclaredAndInferredType::MightBeDifferent` here? It looks like the type always comes from the return annotation of `__getattr__` (i.e., a declaration) and that they're always the same? Intuitively, `DeclaredAndInferredType::AreTheSame` feels like it would be a better fit here

---

_@AlexWaygood approved on 2025-11-03 18:57_

---

_@carljm reviewed on 2025-11-03 20:07_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:5442 on 2025-11-03 20:07_

`AreTheSame` implies "there are no type qualifiers on the declared type", because you provide only a type and internally it builds a no-qualifiers declared type from it. If there are potential qualifiers to pass through, we can't use it.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:5442 on 2025-11-03 20:19_

Ah I missed that, thanks

---

_@AlexWaygood reviewed on 2025-11-03 20:19_

---

_Merged by @carljm on 2025-11-03 20:24_

---

_Closed by @carljm on 2025-11-03 20:24_

---

_Branch deleted on 2025-11-03 20:24_

---
