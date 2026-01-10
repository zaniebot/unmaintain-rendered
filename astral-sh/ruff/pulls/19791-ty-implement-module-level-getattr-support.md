```yaml
number: 19791
title: "[ty] Implement module-level `__getattr__` support"
type: pull_request
state: merged
author: PrettyWood
labels:
  - ty
assignees: []
merged: true
base: main
head: fix/943
created_at: 2025-08-06T19:54:42Z
updated_at: 2025-08-08T17:48:28Z
url: https://github.com/astral-sh/ruff/pull/19791
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Implement module-level `__getattr__` support

---

_Pull request opened by @PrettyWood on 2025-08-06 19:54_

fix https://github.com/astral-sh/ty/issues/943

## Summary

Add module-level `__getattr__` support for ty's type checker, fixing issue https://github.com/astral-sh/ty/issues/943. 
Module-level `__getattr__` functions ([PEP 562](https://peps.python.org/pep-0562/)) are now respected when resolving dynamic attributes, matching the behavior of mypy and pyright.

## Implementation

Thanks @sharkdp for the guidance in https://github.com/astral-sh/ty/issues/943#issuecomment-3157566579
  - Extracts existing `__getattr__` logic into a reusable helper function
  - Adds module-specific `__getattr__` resolution in `ModuleLiteral.static_member()`
  - Maintains proper attribute precedence: explicit attributes > submodules > `__getattr__`
  - Preserves restrictions on fake typeshed `__getattr__` methods

## Test Plan
  - New mdtest covering basic functionality, type annotations, attribute precedence, and edge cases
  (run ```cargo nextest run -p ty_python_semantic mdtest__import_module_getattr```)
  - All new tests pass, verifying `__getattr__` is called correctly and returns proper types
  - Existing test suite passes, ensuring no regressions introduced


---

_Review requested from @carljm by @PrettyWood on 2025-08-06 19:54_

---

_Review requested from @AlexWaygood by @PrettyWood on 2025-08-06 19:54_

---

_Review requested from @sharkdp by @PrettyWood on 2025-08-06 19:54_

---

_Review requested from @dcreager by @PrettyWood on 2025-08-06 19:54_

---

_Renamed from "[ty]Fix/943" to "[ty] Implement module-level `__getattr__` support" by @PrettyWood on 2025-08-06 19:55_

---

_Comment by @github-actions[bot] on 2025-08-06 20:44_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-06 20:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/door/_cls/doormeta.py:82:10: error[unresolved-attribute] Type `<module 'beartype'>` has no attribute `door`
+ beartype/door/_cls/doormeta.py:82:10: error[unresolved-attribute] Type `object` has no attribute `TypeHint`
- beartype/door/_cls/doormeta.py:163:19: error[unresolved-attribute] Type `<module 'beartype'>` has no attribute `door`
+ beartype/door/_cls/doormeta.py:163:19: error[unresolved-attribute] Type `object` has no attribute `TypeHint`
- beartype/door/_cls/doormeta.py:179:10: error[unresolved-attribute] Type `<module 'beartype'>` has no attribute `door`
+ beartype/door/_cls/doormeta.py:179:10: error[unresolved-attribute] Type `object` has no attribute `TypeHint`

black (https://github.com/psf/black)
- src/blackd/__init__.py:104:12: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- src/blackd/__init__.py:110:12: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- src/blackd/__init__.py:113:31: error[invalid-argument-type] Argument to function `parse_mode` is incorrect: Expected `MultiMapping[str]`, found `Unknown | under_cached_property[Unknown]`
- src/blackd/__init__.py:116:27: warning[possibly-unbound-attribute] Attribute `read` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- src/blackd/__init__.py:145:26: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ src/blackd/__init__.py:85:5: error[unresolved-attribute] Type `object` has no attribute `run_app`
+ src/blackd/__init__.py:93:19: error[unresolved-attribute] Type `object` has no attribute `Application`
+ src/blackd/__init__.py:94:11: error[unresolved-attribute] Type `object` has no attribute `Application`
+ src/blackd/__init__.py:97:21: error[unresolved-attribute] Type `object` has no attribute `post`
+ src/blackd/__init__.py:101:27: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/blackd/__init__.py:101:63: error[unresolved-attribute] Type `object` has no attribute `Response`
+ src/blackd/__init__.py:105:20: error[unresolved-attribute] Type `object` has no attribute `Response`
+ src/blackd/__init__.py:115:20: error[unresolved-attribute] Type `object` has no attribute `Response`
+ src/blackd/__init__.py:156:16: error[unresolved-attribute] Type `object` has no attribute `Response`
+ src/blackd/__init__.py:163:16: error[unresolved-attribute] Type `object` has no attribute `Response`
+ src/blackd/__init__.py:165:16: error[unresolved-attribute] Type `object` has no attribute `Response`
+ src/blackd/__init__.py:168:16: error[unresolved-attribute] Type `object` has no attribute `Response`
- Found 56 diagnostics
+ Found 63 diagnostics

anyio (https://github.com/agronholm/anyio)
+ src/anyio/_backends/_asyncio.py:687:26: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `TaskStatus`
+ src/anyio/_backends/_asyncio.py:711:17: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `TaskGroup`
+ src/anyio/_backends/_asyncio.py:999:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `BlockingPortal`
+ src/anyio/_backends/_asyncio.py:1028:27: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
+ src/anyio/_backends/_asyncio.py:1044:27: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
+ src/anyio/_backends/_asyncio.py:1057:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Process`
+ src/anyio/_backends/_asyncio.py:1102:24: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
+ src/anyio/_backends/_asyncio.py:1106:25: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
+ src/anyio/_backends/_asyncio.py:1110:25: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
+ src/anyio/_backends/_asyncio.py:1139:55: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Process`
+ src/anyio/_backends/_asyncio.py:1147:14: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Process`
+ src/anyio/_backends/_asyncio.py:1234:20: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
+ src/anyio/_backends/_asyncio.py:1365:41: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UNIXSocketStream`
+ src/anyio/_backends/_asyncio.py:1481:25: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
+ src/anyio/_backends/_asyncio.py:1494:31: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
+ src/anyio/_backends/_asyncio.py:1541:26: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
+ src/anyio/_backends/_asyncio.py:1548:31: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
+ src/anyio/_backends/_asyncio.py:1578:17: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UDPSocket`
+ src/anyio/_backends/_asyncio.py:1626:26: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUDPSocket`
+ src/anyio/_backends/_asyncio.py:1676:43: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
+ src/anyio/_backends/_asyncio.py:1712:52: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
+ src/anyio/_backends/_asyncio.py:2141:18: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `TestRunner`
+ src/anyio/_backends/_asyncio.py:2392:35: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `TaskGroup`
+ src/anyio/_backends/_asyncio.py:2396:30: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Event`
+ src/anyio/_backends/_asyncio.py:2400:52: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Lock`
+ src/anyio/_backends/_asyncio.py:2410:10: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Semaphore`
+ src/anyio/_backends/_asyncio.py:2414:62: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `CapacityLimiter`
+ src/anyio/_backends/_asyncio.py:2423:18: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `CapacityLimiter`
+ src/anyio/_backends/_asyncio.py:2541:40: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `BlockingPortal`
+ src/anyio/_backends/_asyncio.py:2581:63: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Process`
+ src/anyio/_backends/_asyncio.py:2593:10: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
+ src/anyio/_backends/_asyncio.py:2604:55: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UNIXSocketStream`
+ src/anyio/_backends/_asyncio.py:2658:10: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
+ src/anyio/_backends/_asyncio.py:2658:35: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
+ src/anyio/_backends/_trio.py:172:17: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `TaskGroup`
+ src/anyio/_backends/_trio.py:231:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `BlockingPortal`
+ src/anyio/_backends/_trio.py:264:28: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
+ src/anyio/_backends/_trio.py:285:25: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
+ src/anyio/_backends/_trio.py:301:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Process`
+ src/anyio/_backends/_trio.py:303:13: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
+ src/anyio/_backends/_trio.py:304:14: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
+ src/anyio/_backends/_trio.py:305:14: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
+ src/anyio/_backends/_trio.py:345:24: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteSendStream`
+ src/anyio/_backends/_trio.py:349:25: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
+ src/anyio/_backends/_trio.py:353:25: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ByteReceiveStream`
+ src/anyio/_backends/_trio.py:367:47: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Process`
+ src/anyio/_backends/_trio.py:416:38: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketStream`
+ src/anyio/_backends/_trio.py:449:38: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UNIXSocketStream`
+ src/anyio/_backends/_trio.py:516:43: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
+ src/anyio/_backends/_trio.py:532:44: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
+ src/anyio/_backends/_trio.py:547:51: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UDPSocket`
+ src/anyio/_backends/_trio.py:569:60: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUDPSocket`
+ src/anyio/_backends/_trio.py:590:49: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
+ src/anyio/_backends/_trio.py:613:28: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
+ src/anyio/_backends/_trio.py:887:18: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `TestRunner`
+ src/anyio/_backends/_trio.py:1034:10: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `CancelScope`
+ src/anyio/_backends/_trio.py:1042:35: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `TaskGroup`
+ src/anyio/_backends/_trio.py:1046:30: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Event`
+ src/anyio/_backends/_trio.py:1060:10: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Semaphore`
+ src/anyio/_backends/_trio.py:1073:18: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `CapacityLimiter`
+ src/anyio/_backends/_trio.py:1109:40: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `BlockingPortal`
+ src/anyio/_backends/_trio.py:1154:63: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `Process`
+ src/anyio/_backends/_trio.py:1176:55: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UNIXSocketStream`
+ src/anyio/_backends/_trio.py:1187:58: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
+ src/anyio/_backends/_trio.py:1191:59: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
+ src/anyio/_backends/_trio.py:1220:10: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
+ src/anyio/_backends/_trio.py:1226:10: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
+ src/anyio/_backends/_trio.py:1231:10: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `UNIXDatagramSocket`
+ src/anyio/_backends/_trio.py:1231:35: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `ConnectedUNIXDatagramSocket`
+ src/anyio/_backends/_trio.py:1290:65: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `SocketListener`
+ src/anyio/_core/_fileio.py:90:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:93:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:96:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:99:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:102:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:105:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:108:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:117:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:128:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:131:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:134:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:137:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:140:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:187:16: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:209:25: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:383:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:467:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:471:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:475:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:479:19: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:487:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:495:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:499:27: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:506:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:511:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:516:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:519:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:522:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:530:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:538:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:541:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:556:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:559:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:564:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:594:20: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:600:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:603:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:608:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:625:24: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:632:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:639:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:644:27: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:651:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:657:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:663:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:673:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:676:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:680:19: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:721:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_fileio.py:737:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_sockets.py:823:19: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_sockets.py:825:23: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:96:20: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:205:20: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:327:26: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:499:31: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:502:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:511:19: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:517:19: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:557:18: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:592:18: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:604:18: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/_core/_tempfile.py:616:18: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/streams/file.py:34:15: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/streams/file.py:72:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/streams/file.py:77:26: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/streams/file.py:102:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/streams/file.py:114:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/streams/file.py:139:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/streams/file.py:144:19: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/streams/tls.py:149:32: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
+ src/anyio/to_interpreter.py:205:22: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run_sync`
- Found 82 diagnostics
+ Found 224 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/http.py:637:39: error[unresolved-attribute] Type `<module 'src.werkzeug.datastructures'>` has no attribute `cache_control`
- src/werkzeug/http.py:643:28: error[unresolved-attribute] Type `<module 'src.werkzeug.datastructures'>` has no attribute `cache_control`
- src/werkzeug/http.py:650:28: error[unresolved-attribute] Type `<module 'src.werkzeug.datastructures'>` has no attribute `cache_control`
- src/werkzeug/http.py:657:28: error[unresolved-attribute] Type `<module 'src.werkzeug.datastructures'>` has no attribute `cache_control`
- tests/test_datastructures.py:555:16: error[unresolved-attribute] Type `<module 'werkzeug.datastructures'>` has no attribute `OrderedMultiDict`
- Found 374 diagnostics
+ Found 369 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/middleware/wsgi.py:133:13: error[unresolved-attribute] Type `<module 'anyio'>` has no attribute `from_thread`
+ starlette/middleware/wsgi.py:133:13: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run`
- starlette/middleware/wsgi.py:148:13: error[unresolved-attribute] Type `<module 'anyio'>` has no attribute `from_thread`
+ starlette/middleware/wsgi.py:148:13: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run`
- starlette/middleware/wsgi.py:153:9: error[unresolved-attribute] Type `<module 'anyio'>` has no attribute `from_thread`
+ starlette/middleware/wsgi.py:153:9: error[unresolved-attribute] Type `type[BrokenWorkerInterpreter]` has no attribute `run`

svcs (https://github.com/hynek/svcs)
+ src/svcs/aiohttp.py:32:29: error[unresolved-attribute] Type `object` has no attribute `AppKey`
+ src/svcs/aiohttp.py:42:24: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:49:23: error[unresolved-attribute] Type `object` has no attribute `Application`
+ src/svcs/aiohttp.py:57:10: error[unresolved-attribute] Type `object` has no attribute `Application`
+ src/svcs/aiohttp.py:61:6: error[unresolved-attribute] Type `object` has no attribute `Application`
+ src/svcs/aiohttp.py:75:2: error[unresolved-attribute] Type `object` has no attribute `middleware`
+ src/svcs/aiohttp.py:77:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:78:6: error[unresolved-attribute] Type `object` has no attribute `Response`
+ src/svcs/aiohttp.py:86:10: error[unresolved-attribute] Type `object` has no attribute `Application`
+ src/svcs/aiohttp.py:108:10: error[unresolved-attribute] Type `object` has no attribute `Application`
+ src/svcs/aiohttp.py:129:32: error[unresolved-attribute] Type `object` has no attribute `Application`
+ src/svcs/aiohttp.py:143:24: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:154:34: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:163:25: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:168:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:174:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:184:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:195:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:207:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:220:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:234:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:249:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:265:14: error[unresolved-attribute] Type `object` has no attribute `Request`
+ src/svcs/aiohttp.py:280:25: error[unresolved-attribute] Type `object` has no attribute `Request`
- Found 78 diagnostics
+ Found 102 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/fields.py:81:26: error[unresolved-attribute] Type `object` has no attribute `Discriminator`
+ pydantic/fields.py:150:26: error[unresolved-attribute] Type `object` has no attribute `Discriminator`
+ pydantic/fields.py:194:19: error[unresolved-attribute] Type `object` has no attribute `Strict`
+ pydantic/fields.py:208:22: error[unresolved-attribute] Type `object` has no attribute `FailFast`
- pydantic/fields.py:866:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
+ pydantic/fields.py:866:26: error[unresolved-attribute] Type `object` has no attribute `Discriminator`
- pydantic/fields.py:905:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
+ pydantic/fields.py:905:26: error[unresolved-attribute] Type `object` has no attribute `Discriminator`
- pydantic/fields.py:944:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
+ pydantic/fields.py:944:26: error[unresolved-attribute] Type `object` has no attribute `Discriminator`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
+ pydantic/fields.py:983:26: error[unresolved-attribute] Type `object` has no attribute `Discriminator`
- pydantic/fields.py:1022:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
+ pydantic/fields.py:1022:26: error[unresolved-attribute] Type `object` has no attribute `Discriminator`
- pydantic/fields.py:1060:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
+ pydantic/fields.py:1060:26: error[unresolved-attribute] Type `object` has no attribute `Discriminator`
- pydantic/fields.py:1099:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `str | Discriminator | None`
+ pydantic/fields.py:1099:26: error[unresolved-attribute] Type `object` has no attribute `Discriminator`
- pydantic/v1/_hypothesis_plugin.py:83:15: error[unresolved-attribute] Type `<module 'pydantic'>` has no attribute `PyObject`
+ pydantic/v1/_hypothesis_plugin.py:81:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pydantic/v1/_hypothesis_plugin.py:83:15: error[invalid-type-form] Variable of type `object` is not allowed in a type expression
- pydantic/v1/_hypothesis_plugin.py:190:22: error[unresolved-attribute] Type `<module 'pydantic.types'>` has no attribute `ConstrainedNumberMeta`
- pydantic/v1/_hypothesis_plugin.py:190:63: error[unresolved-attribute] Type `<module 'pydantic.types'>` has no attribute `ConstrainedNumberMeta`
- pydantic/v1/_hypothesis_plugin.py:195:40: error[unresolved-attribute] Type `<module 'pydantic.types'>` has no attribute `ConstrainedNumberMeta`
- pydantic/v1/_hypothesis_plugin.py:196:36: error[unresolved-attribute] Type `<module 'pydantic.types'>` has no attribute `ConstrainedNumberMeta`
- pydantic/v1/_hypothesis_plugin.py:200:5: error[unresolved-attribute] Type `<module 'pydantic.types'>` has no attribute `_DEFINED_TYPES`
- pydantic/v1/_hypothesis_plugin.py:209:22: error[unresolved-attribute] Type `<module 'pydantic.types'>` has no attribute `ConstrainedNumberMeta`
- pydantic/v1/_hypothesis_plugin.py:222:11: error[unresolved-attribute] Type `<module 'pydantic'>` has no attribute `JsonWrapper`
- pydantic/v1/_hypothesis_plugin.py:242:11: error[unresolved-attribute] Type `<module 'pydantic'>` has no attribute `ConstrainedBytes`
- pydantic/v1/_hypothesis_plugin.py:263:11: error[unresolved-attribute] Type `<module 'pydantic'>` has no attribute `ConstrainedDecimal`
- pydantic/v1/_hypothesis_plugin.py:281:11: error[unresolved-attribute] Type `<module 'pydantic'>` has no attribute `ConstrainedFloat`
- pydantic/v1/_hypothesis_plugin.py:313:11: error[unresolved-attribute] Type `<module 'pydantic'>` has no attribute `ConstrainedInt`
- pydantic/v1/_hypothesis_plugin.py:336:11: error[unresolved-attribute] Type `<module 'pydantic'>` has no attribute `ConstrainedDate`
- pydantic/v1/_hypothesis_plugin.py:355:11: error[unresolved-attribute] Type `<module 'pydantic'>` has no attribute `ConstrainedStr`
- pydantic/v1/_hypothesis_plugin.py:388:17: error[unresolved-attribute] Type `<module 'pydantic.types'>` has no attribute `_DEFINED_TYPES`
- pydantic/v1/_hypothesis_plugin.py:390:1: error[unresolved-attribute] Unresolved attribute `_registered` on type `<module 'pydantic.types'>`.
- Found 769 diagnostics
+ Found 759 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/third_party/pydantic.py:20:23: error[unresolved-attribute] Type `<module 'pydantic'>` has no attribute `validate_arguments`
+ src/hydra_zen/third_party/pydantic.py:20:23: error[call-non-callable] Object of type `object` is not callable

porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/filetypes.py:166:30: error[unresolved-attribute] Type `<module 'pygments.lexers'>` has no attribute `TextLexer`
- porcupine/plugins/highlight/pygments_highlighter.py:9:29: error[unresolved-import] Module `pygments.lexers` has no member `MarkdownLexer`
- porcupine/tabs.py:20:29: error[unresolved-import] Module `pygments.lexers` has no member `TextLexer`
- Found 22 diagnostics
+ Found 19 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- aiohttp_devtools/logs.py:16:29: error[unresolved-import] Module `pygments.lexers` has no member `Python3TracebackLexer`
+ aiohttp_devtools/runserver/config.py:16:20: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/config.py:16:50: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/config.py:16:91: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/config.py:205:33: error[unresolved-attribute] Type `object` has no attribute `Application`
- aiohttp_devtools/runserver/log_handlers.py:35:75: warning[possibly-unbound-attribute] Attribute `startswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp_devtools/runserver/log_handlers.py:35:99: warning[possibly-unbound-attribute] Attribute `endswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp_devtools/runserver/log_handlers.py:60:32: error[no-matching-overload] No overload of bound method `__init__` matches arguments
+ aiohttp_devtools/runserver/config.py:233:58: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/config.py:234:36: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/config.py:242:32: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/log_handlers.py:16:32: error[unresolved-attribute] Type `object` has no attribute `BaseRequest`
+ aiohttp_devtools/runserver/log_handlers.py:16:59: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/log_handlers.py:19:30: error[unresolved-attribute] Type `object` has no attribute `BaseRequest`
+ aiohttp_devtools/runserver/log_handlers.py:19:57: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/log_handlers.py:22:28: error[unresolved-attribute] Type `object` has no attribute `BaseRequest`
+ aiohttp_devtools/runserver/log_handlers.py:22:55: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/log_handlers.py:43:32: error[unresolved-attribute] Type `object` has no attribute `BaseRequest`
+ aiohttp_devtools/runserver/log_handlers.py:43:59: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/log_handlers.py:52:30: error[unresolved-attribute] Type `object` has no attribute `BaseRequest`
+ aiohttp_devtools/runserver/log_handlers.py:52:57: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/log_handlers.py:57:59: error[unresolved-attribute] Type `object` has no attribute `Response`
+ aiohttp_devtools/runserver/log_handlers.py:72:32: error[unresolved-attribute] Type `object` has no attribute `BaseRequest`
+ aiohttp_devtools/runserver/log_handlers.py:72:59: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/serve.py:38:15: error[unresolved-attribute] Type `object` has no attribute `AppKey`
+ aiohttp_devtools/runserver/serve.py:39:21: error[unresolved-attribute] Type `object` has no attribute `AppKey`
+ aiohttp_devtools/runserver/serve.py:40:15: error[unresolved-attribute] Type `object` has no attribute `AppKey`
+ aiohttp_devtools/runserver/serve.py:41:14: error[unresolved-attribute] Type `object` has no attribute `AppKey`
+ aiohttp_devtools/runserver/serve.py:42:6: error[unresolved-attribute] Type `object` has no attribute `AppKey`
+ aiohttp_devtools/runserver/serve.py:42:33: error[unresolved-attribute] Type `object` has no attribute `WebSocketResponse`
+ aiohttp_devtools/runserver/serve.py:45:26: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/serve.py:55:29: error[unresolved-attribute] Type `object` has no attribute `Application`
- aiohttp_devtools/runserver/serve.py:76:20: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp_devtools/runserver/serve.py:84:24: warning[possibly-unbound-attribute] Attribute `startswith` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp_devtools/runserver/serve.py:365:35: error[non-subscriptable] Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
- aiohttp_devtools/runserver/serve.py:375:17: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | under_cached_property[Unknown]` is possibly unbound
- aiohttp_devtools/runserver/serve.py:381:25: warning[possibly-unbound-implicit-call] Method `__setitem__` of type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ aiohttp_devtools/runserver/serve.py:65:26: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/serve.py:74:27: error[unresolved-attribute] Type `object` has no attribute `Request`
+ aiohttp_devtools/runserver/serve.py:81:39: error[unresolved-attribute] Type `object` has no attribute `Request`
+ aiohttp_devtools/runserver/serve.py:81:62: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/serve.py:82:42: error[unresolved-attribute] Type `object` has no attribute `Response`
+ aiohttp_devtools/runserver/serve.py:94:10: error[unresolved-attribute] Type `object` has no attribute `middleware`
+ aiohttp_devtools/runserver/serve.py:95:48: error[unresolved-attribute] Type `object` has no attribute `Request`
+ aiohttp_devtools/runserver/serve.py:95:82: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/serve.py:106:10: error[unresolved-attribute] Type `object` has no attribute `middleware`
+ aiohttp_devtools/runserver/serve.py:107:46: error[unresolved-attribute] Type `object` has no attribute `Request`
+ aiohttp_devtools/runserver/serve.py:107:80: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/serve.py:117:40: error[unresolved-attribute] Type `object` has no attribute `Request`
+ aiohttp_devtools/runserver/serve.py:117:56: error[unresolved-attribute] Type `object` has no attribute `Response`
+ aiohttp_devtools/runserver/serve.py:121:20: error[unresolved-attribute] Type `object` has no attribute `Response`
+ aiohttp_devtools/runserver/serve.py:197:71: error[unresolved-attribute] Type `object` has no attribute `AppRunner`
+ aiohttp_devtools/runserver/serve.py:202:12: error[unresolved-attribute] Type `object` has no attribute `AppRunner`
+ aiohttp_devtools/runserver/serve.py:205:34: error[unresolved-attribute] Type `object` has no attribute `AppRunner`
+ aiohttp_devtools/runserver/serve.py:207:12: error[unresolved-attribute] Type `object` has no attribute `TCPSite`
+ aiohttp_devtools/runserver/serve.py:211:27: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/serve.py:257:32: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/serve.py:264:41: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/serve.py:265:11: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/serve.py:266:19: error[unresolved-attribute] Type `object` has no attribute `WebSocketResponse`
+ aiohttp_devtools/runserver/serve.py:294:34: error[unresolved-attribute] Type `object` has no attribute `Request`
+ aiohttp_devtools/runserver/serve.py:294:50: error[unresolved-attribute] Type `object` has no attribute `Response`
+ aiohttp_devtools/runserver/serve.py:299:12: error[unresolved-attribute] Type `object` has no attribute `Response`
+ aiohttp_devtools/runserver/serve.py:305:38: error[unresolved-attribute] Type `object` has no attribute `Request`
+ aiohttp_devtools/runserver/serve.py:305:54: error[unresolved-attribute] Type `object` has no attribute `WebSocketResponse`
+ aiohttp_devtools/runserver/serve.py:306:10: error[unresolved-attribute] Type `object` has no attribute `WebSocketResponse`
+ aiohttp_devtools/runserver/serve.py:361:39: error[unresolved-attribute] Type `object` has no attribute `Request`
+ aiohttp_devtools/runserver/serve.py:387:40: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/serve.py:387:63: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/serve.py:388:37: error[unresolved-attribute] Type `object` has no attribute `FileResponse`
+ aiohttp_devtools/runserver/serve.py:399:16: error[unresolved-attribute] Type `object` has no attribute `Response`
- aiohttp_devtools/runserver/serve.py:442:17: error[non-subscriptable] Cannot subscript object of type `under_cached_property[Unknown]` with no `__getitem__` method
+ aiohttp_devtools/runserver/serve.py:405:40: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/serve.py:406:10: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/serve.py:413:59: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/serve.py:424:16: error[unresolved-attribute] Type `object` has no attribute `Response`
+ aiohttp_devtools/runserver/serve.py:426:38: error[unresolved-attribute] Type `object` has no attribute `Request`
+ aiohttp_devtools/runserver/serve.py:426:54: error[unresolved-attribute] Type `object` has no attribute `StreamResponse`
+ aiohttp_devtools/runserver/watch.py:23:11: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/watch.py:29:32: error[unresolved-attribute] Type `object` has no attribute `Application`
+ aiohttp_devtools/runserver/watch.py:45:38: error[unresolved-attribute] Type `object` has no attribute `Application`
+ tests/cleanup_app.py:24:12: error[unresolved-attribute] Type `object` has no attribute `Response`
+ tests/cleanup_app.py:41:7: error[unresolved-attribute] Type `object` has no attribute `Application`
+ tests/test_runserver_config.py:62:28: error[unresolved-attribute] Type `object` has no attribute `Application`
+ tests/test_runserver_logs.py:100:30: error[unresolved-attribute] Type `object` has no attribute `Request`
+ tests/test_runserver_logs.py:105:31: error[unresolved-attribute] Type `object` has no attribute `Response`
- tests/test_runserver_main.py:66:32: error[unresolved-attribute] Type `<module 'aiohttp'>` has no attribute `web`
+ tests/test_runserver_main.py:66:32: error[unresolved-attribute] Type `object` has no attribute `Application`
- tests/test_runserver_main.py:115:32: error[unresolved-attribute] Type `<module 'aiohttp'>` has no attribute `web`
+ tests/test_runserver_main.py:115:32: error[unresolved-attribute] Type `object` has no attribute `Application`
- tests/test_runserver_main.py:171:28: error[unresolved-attribute] Type `<module 'aiohttp'>` has no attribute `web`
+ tests/test_runserver_main.py:171:28: error[unresolved-attribute] Type `object` has no attribute `Application`
- tests/test_runserver_main.py:368:32: error[unresolved-attribute] Type `<module 'aiohttp'>` has no attribute `web`
+ tests/test_runserver_main.py:368:32: error[unresolved-attribute] Type `object` has no attribute `Application`
- Found 57 diagnostics
+ Found 123 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/specs/graphql/scalars.py:77:27: error[unresolved-attribute] Type `<module 'src.schemathesis.specs.graphql.nodes'>` has no attribute `String`
- src/schemathesis/specs/graphql/scalars.py:78:27: error[unresolved-attribute] Type `<module 'src.schemathesis.specs.graphql.nodes'>` has no attribute `String`
- src/schemathesis/specs/graphql/scalars.py:79:63: error[unresolved-attribute] Type `<module 'src.schemathesis.specs.graphql.nodes'>` has no attribute `String`
- src/schemathesis/specs/graphql/scalars.py:80:46: error[unresolved-attribute] Type `<module 'src.schemathesis.specs.graphql.nodes'>` has no attribute `String`
- src/schemathesis/specs/graphql/scalars.py:81:51: error[unresolved-attribute] Type `<module 'src.schemathesis.specs.graphql.nodes'>` has no attribute `String`
- src/schemathesis/specs/graphql/scalars.py:82:51: error[unresolved-attribute] Type `<module 'src.schemathesis.specs.graphql.nodes'>` has no attribute `String`
- src/schemathesis/specs/graphql/scalars.py:83:37: error[unresolved-attribute] Type `<module 'src.schemathesis.specs.graphql.nodes'>` has no attribute `Int`
- src/schemathesis/specs/graphql/scalars.py:84:74: error[unresolved-attribute] Type `<module 'src.schemathesis.specs.graphql.nodes'>` has no attribute `Int`
- src/schemathesis/specs/graphql/scalars.py:85:41: error[unresolved-attribute] Type `<module 'src.schemathesis.specs.graphql.nodes'>` has no attribute `String`
- src/schemathesis/specs/openapi/schemas.py:53:26: error[unresolved-import] Module `src.schemathesis.specs.openapi.definitions` has no member `OPENAPI_30_VALIDATOR`
- src/schemathesis/specs/openapi/schemas.py:53:48: error[unresolved-import] Module `src.schemathesis.specs.openapi.definitions` has no member `OPENAPI_31_VALIDATOR`
- src/schemathesis/specs/openapi/schemas.py:53:70: error[unresolved-import] Module `src.schemathesis.specs.openapi.definitions` has no member `SWAGGER_20_VALIDATOR`
- Found 290 diagnostics
+ Found 278 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/utils/templates_tests.py:36:85: error[invalid-argument-type] Argument to function `script_tag_filter` is incorrect: Expected `ExtraScriptValue`, found `Unknown | ExtraScriptValue | str`
- Found 182 diagnostics
+ Found 181 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/compiler.py:1133:38: error[invalid-argument-type] Argument to bound method `ref` is incorrect: Expected `str`, found `Unknown | str | (tuple[str, str] & ~tuple[Unknown, ...])`
- src/jinja2/compiler.py:1136:52: error[invalid-argument-type] Argument to bound method `ref` is incorrect: Expected `str`, found `Unknown | str | (tuple[str, str] & ~tuple[Unknown, ...])`
- src/jinja2/compiler.py:1149:38: error[invalid-argument-type] Argument to bound method `ref` is incorrect: Expected `str`, found `Unknown | str | (tuple[str, str] & ~tuple[Unknown, ...])`
- src/jinja2/compiler.py:1154:24: warning[possibly-unbound-attribute] Attribute `startswith` on type `Unknown | str | (tuple[str, str] & ~tuple[Unknown, ...])` is possibly unbound
- src/jinja2/environment.py:603:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Template`
+ src/jinja2/meta.py:53:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/jinja2/optimizer.py:24:12: warning[redundant-cast] Value is already of type `Any`
- src/jinja2/parser.py:163:34: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Node | list[Node]`
- src/jinja2/parser.py:805:52: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Getattr | Getitem`
- tests/test_loader.py:94:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[DictLoader], Literal["one"]]` and `None`, in comparing `tuple[ReferenceType[DictLoader], Literal["one"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
- tests/test_loader.py:95:16: error[unsupported-operator] Operator `not in` is not supported for types `tuple[ReferenceType[DictLoader], Literal["two"]]` and `None`, in comparing `tuple[ReferenceType[DictLoader], Literal["two"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
- tests/test_loader.py:96:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[DictLoader], Literal["three"]]` and `None`, in comparing `tuple[ReferenceType[DictLoader], Literal["three"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
+ tests/test_loader.py:94:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[Any], Literal["one"]]` and `None`, in comparing `tuple[ReferenceType[Any], Literal["one"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
+ tests/test_loader.py:95:16: error[unsupported-operator] Operator `not in` is not supported for types `tuple[ReferenceType[Any], Literal["two"]]` and `None`, in comparing `tuple[ReferenceType[Any], Literal["two"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
+ tests/test_loader.py:96:16: error[unsupported-operator] Operator `in` is not supported for types `tuple[ReferenceType[Any], Literal["three"]]` and `None`, in comparing `tuple[ReferenceType[Any], Literal["three"]]` with `Unknown | MutableMapping[tuple[ReferenceType[BaseLoader], str], Template] | None`
- Found 191 diagnostics
+ Found 186 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/asyncio_test.py:215:13: error[unresolved-attribute] Type `<module 'tornado.platform.asyncio'>` has no attribute `AnyThreadEventLoopPolicy`
- Found 246 diagnostics
+ Found 245 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- Pythonwin/pywin/test/test_pywin.py:327:26: error[unresolved-attribute] Type `<module '__main__'>` has no attribute `aa`
- Pythonwin/pywin/test/test_pywin.py:329:26: error[unresolved-attribute] Type `<module '__main__'>` has no attribute `ff`
- Found 2006 diagnostics
+ Found 2004 diagnostics

vision (https://github.com/pytorch/vision)
- gallery/transforms/plot_transforms_e2e.py:65:11: error[unresolved-attribute] Type `<module 'torchvision.datasets'>` has no attribute `wrap_dataset_for_transforms_v2`
- gallery/transforms/plot_transforms_e2e.py:115:11: error[unresolved-attribute] Type `<module 'torchvision.datasets'>` has no attribute `wrap_dataset_for_transforms_v2`
- references/detection/coco_utils.py:213:42: error[unresolved-import] Module `torchvision.datasets` has no member `wrap_dataset_for_transforms_v2`
- references/segmentation/coco_utils.py:102:42: error[unresolved-import] Module `torchvision.datasets` has no member `wrap_dataset_for_transforms_v2`
- test/builtin_dataset_mocks.py:1529:36: error[unresolved-import] Module `numpy.core.records` has no member `fromarrays`
- test/datasets_utils.py:33:34: error[unresolved-import] Module `torchvision.datasets` has no member `wrap_dataset_for_transforms_v2`
- test/test_datasets.py:882:27: error[unresolved-attribute] Type `<module 'torchvision.datasets'>` has no attribute `wrap_dataset_for_transforms_v2`
- test/test_datasets.py:887:23: error[unresolved-attribute] Type `<module 'torchvision.datasets'>` has no attribute `wrap_dataset_for_transforms_v2`
- test/test_datasets.py:1674:24: error[unresolved-attribute] Type `<module 'torchvision.datasets'>` has no attribute `folder`
- test/test_datasets.py:2754:40: error[unresolved-import] Module `numpy.core.records` has no member `fromarrays`
- test/test_datasets.py:3587:13: error[unresolved-attribute] Type `<module 'torchvision.datasets'>` has no attribute `wrap_dataset_for_transforms_v2`
- test/test_datasets.py:3596:13: error[unresolved-attribute] Type `<module 'torchvision.datasets'>` has no attribute `wrap_dataset_for_transforms_v2`
- test/test_datasets.py:3602:13: error[unresolved-attribute] Type `<module 'torchvision.datasets'>` has no attribute `wrap_dataset_for_transforms_v2`
- test/test_datasets.py:3618:27: error[unresolved-attribute] Type `<module 'torchvision.datasets'>` has no attribute `wrap_dataset_for_transforms_v2`
- Found 1480 diagnostics
+ Found 1466 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_commands.py:49:59: error[unresolved-attribute] Type `<module 'scrapy'>` has no attribute `settings`
- tests/test_feedexport.py:1348:29: error[unresolved-attribute] Type `<module 'scrapy'>` has no attribute `extensions`
- tests/test_feedexport.py:1352:29: error[unresolved-attribute] Type `<module 'scrapy'>` has no attribute `extensions`
- tests/test_scrapy__getattr__.py:7:13: error[unresolved-import] Module `scrapy` has no member `twisted_version`
- tests/test_scrapy__getattr__.py:21:13: error[unresolved-import] Module `scrapy.settings.default_settings` has no member `CONCURRENT_REQUESTS_PER_IP`
- Found 1064 diagnostics
+ Found 1059 diagnostics

AutoSplit (https://github.com/Toufool/AutoSplit)
+ src/AutoControlledThread.py:12:28: error[unresolved-attribute] Type `list[str]` has no attribute `QThread`
+ src/AutoControlledThread.py:17:6: error[unresolved-attribute] Type `list[str]` has no attribute `Slot`
+ src/AutoSplit.py:101:34: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:102:20: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:103:25: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:104:25: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:105:20: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:106:25: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:107:35: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:108:36: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:109:33: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:111:25: error[unresolved-attribute] Type `list[str]` has no attribute `Signal`
+ src/AutoSplit.py:114:24: error[unresolved-attribute] Type `list[str]` has no attribute `QTimer`
+ src/AutoSplit.py:115:35: error[unresolved-attribute] Type `list[str]` has no attribute `Qt`
+ src/AutoSplit.py:116:25: error[unresolved-attribute] Type `list[str]` has no attribute `QTimer`
+ src/AutoSplit.py:117:36: error[unresolved-attribute] Type `list[str]` has no attribute `Qt`
+ src/AutoSplit.py:122:28: error[unresolved-attribute] Type `list[str]` has no attribute `QThread`
+ src/AutoSplit.py:984:33: error[unresolved-attribute] Type `list[str]` has no attribute `QCloseEvent`
- src/AutoSplit.py:1031:9: warning[possibly-unbound-attribute] Attribute `ignore` on type `QCloseEvent | None` is possibly unbound
+ src/AutoSplit.py:1031:9: warning[possibly-unbound-attribute] Attribute `ignore` on type `Unknown | None` is possibly unbound
+ src/AutoSplit.py:1043:28: error[unresolved-attribute] Type `list[str]` has no attribute `QImage`
+ src/AutoSplit.py:1046:28: error[unresolved-attribute] Type `list[str]` has no attribute `QImage`
+ src/AutoSplit.py:1049:18: error[unresolved-attribute] Type `list[str]` has no attribute `QImage`
+ src/AutoSplit.py:1051:13: error[unresolved-attribute] Type `list[str]` has no attribute `QPixmap`
+ src/AutoSplit.py:1053:17: error[unresolved-attribute] Type `list[str]` has no attribute `Qt`
+ src/AutoSplit.py:1054:17: error[unresolved-attribute] Type `list[str]` has no attribute `Qt`
+ src/AutoSplit.py:1078:27: error[unresolved-attribute] Type `list[str]` has no attribute `QIcon`
+ src/AutoSplit.py:1093:21: error[unresolved-attribute] Type `list[str]` has no attribute `QTimer`
+ src/error_messages.py:30:19: error[unresolved-attribute] Type `list[str]` has no attribute `QMessageBox`
+ src/error_messages.py:32:31: error[unresolved-attribute] Type `list[str]` has no attribute `Qt`
+ src/error_messages.py:36:46: error[unresolved-attribute] Type `list[str]` has no attribute `QMessageBox`
+ src/error_messages.py:40:13: error[unresolved-attribute] Type `list[str]` has no attribute `QMessageBox`
+ src/error_messages.py:47:50: error[unresolved-attribute] Type `list[str]` has no attribute `QMessageBox`
+ src/hotkeys.py:303:14: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/menu_bar.py:50:21: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/menu_bar.py:63:42: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/menu_bar.py:67:29: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/menu_bar.py:108:17: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/menu_bar.py:121:31: error[unresolved-attribute] Type `list[str]` has no attribute `QThread`
+ src/menu_bar.py:161:24: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/menu_bar.py:320:17: error[unresolved-attribute] Type `list[str]` has no attribute `QRect`
+ src/menu_bar.py:339:27: error[unresolved-attribute] Type `list[str]` has no attribute `QLineEdit`
+ src/menu_bar.py:340:39: error[unresolved-attribute] Type `list[str]` has no attribute `QPushButton`
+ src/menu_bar.py:358:23: error[unresolved-attribute] Type `list[str]` has no attribute `QCheckBox`
+ src/menu_bar.py:465:45: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/menu_bar.py:470:19: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/region_selection.py:213:25: error[unresolved-attribute] Type `list[str]` has no attribute `QFileDialog`
+ src/region_selection.py:315:24: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/region_selection.py:338:29: error[unresolved-attribute] Type `list[str]` has no attribute `Qt`
+ src/region_selection.py:342:36: error[unresolved-attribute] Type `list[str]` has no attribute `QKeyEvent`
+ src/region_selection.py:343:27: error[unresolved-attribute] Type `list[str]` has no attribute `Qt`
+ src/region_selection.py:351:40: error[unresolved-attribute] Type `list[str]` has no attribute `QMouseEvent`
+ src/region_selection.py:364:15: error[unresolved-attribute] Type `list[str]` has no attribute `QPoint`
+ src/region_selection.py:365:13: error[unresolved-attribute] Type `list[str]` has no attribute `QPoint`
+ src/region_selection.py:368:9: error[unresolved-attribute] Type `list[str]` has no attribute `QApplication`
+ src/region_selection.py:368:50: error[unresolved-attribute] Type `list[str]` has no attribute `QCursor`
+ src/region_selection.py:368:64: error[unresolved-attribute] Type `list[str]` has no attribute `Qt`
+ src/region_selection.py:372:33: error[unresolved-attribute] Type `list[str]` has no attribute `QPaintEvent`
+ src/region_selection.py:374:24: error[unresolved-attribute] Type `list[str]` has no attribute `QPainter`
+ src/region_selection.py:375:29: error[unresolved-attribute] Type `list[str]` has no attribute `QPen`
+ src/region_selection.py:375:40: error[unresolved-attribute] Type `list[str]` has no attribute `QColor`
+ src/region_selection.py:376:31: error[unresolved-attribute] Type `list[str]` has no attribute `QColor`
+ src/region_selection.py:377:31: error[unresolved-attribute] Type `list[str]` has no attribute `QRect`
+ src/region_selection.py:380:38: error[unresolved-attribute] Type `list[str]` has no attribute `QMouseEvent`
+ src/region_selection.py:386:37: error[unresolved-attribute] Type `list[str]` has no attribute `QMouseEvent`
+ src/region_selection.py:391:40: error[unresolved-attribute] Type `list[str]` has no attribute `QMouseEvent`
+ src/region_selection.py:406:9: error[unresolved-attribute] Type `list[str]` has no attribute `QApplication`
+ src/region_selection.py:406:50: error[unresolved-attribute] Type `list[str]` has no attribute `QCursor`
+ src/region_selection.py:406:64: error[unresolved-attribute] Type `list[str]` has no attribute `Qt`
+ src/user_profile.py:99:31: error[unresolved-attribute] Type `list[str]` has no attribute `QFileDialog`
+ src/user_profile.py:130:28: error[unresolved-attribute] Type `list[str]` has no attribute `QWidget`
+ src/user_profile.py:186:12: error[unresolved-attribute] Type `list[str]` has no attribute `QFileDialog`
+ src/user_profile.py:234:9: error[unresolved-attribute] Type `list[str]` has no attribute `QSettings`
+ src/user_profile.py:249:5: error[unresolved-attribute] Type `list[str]` has no attribute `QSettings`
- Found 38 diagnostics
+ Found 110 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_py/path.py:94:21: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `ENOENT`
- src/_pytest/_py/path.py:94:35: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `ENOTDIR`
- src/_pytest/_py/path.py:94:50: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EBUSY`
- src/_pytest/_py/path.py:116:20: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `ELOOP`
- src/_pytest/_py/path.py:405:19: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EINVAL`
- src/_pytest/_py/path.py:408:16: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EXDEV`
- src/_pytest/_py/path.py:940:20: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EEXIST`
- src/_pytest/_py/path.py:992:16: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EINVAL`
- src/_pytest/_py/path.py:999:16: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `ENOENT`
- src/_pytest/_py/path.py:1092:19: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `ENOENT`
- src/_pytest/_py/path.py:1134:20: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `ENOENT`
- src/_pytest/_py/path.py:1235:28: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EACCES`
- src/_pytest/_py/path.py:1346:21: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EEXIST`
- src/_pytest/_py/path.py:1346:35: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `ENOENT`
- src/_pytest/_py/path.py:1346:49: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EBUSY`
- src/_pytest/_py/path.py:1385:29: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EEXIST`
- src/_pytest/_py/path.py:1385:43: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `ENOENT`
- src/_pytest/_py/path.py:1385:57: error[unresolved-attribute] Type `<module 'src._pytest._py.error'>` has no attribute `EBUSY`
- testing/_py/test_local.py:177:28: error[unresolved-attribute] Type `<module '_pytest._py.error'>` has no attribute `ENOTDIR`
- testing/_py/test_local.py:224:36: error[unresolved-attribute] Type `<module '_pytest._py.error'>` has no attribute `ENOENT`
- testing/_py/test_local.py:423:28: error[unresolved-attribute] Type `<module '_pytest._py.error'>` has no attribute `EEXIST`
- testing/_py/test_local.py:628:23: error[unresolved-attribute] Type `<module '_pytest._py.error'>` has no attribute `ENOENT`
- testing/_py/test_local.py:632:28: error[unresolved-attribute] Type `<module '_pytest._py.error'>` has no attribute `ENOENT`
- testing/_py/test_local.py:876:34: error[unresolved-attribute] Type `<module '_pytest._py.error'>` has no attribute `EEXIST`
- testing/_py/test_local.py:876:48: error[unresolved-attribute] Type `<module '_pytest._py.error'>` has no attribute `ENOENT`
- testing/_py/test_local.py:1400:23: error[unresolved-attribute] Type `<module '_pytest._py.error'>` has no attribute `ENOENT`
- Found 496 diagnostics
+ Found 470 diagnostics

altair (https://github.com/vega/altair)
+ altair/vegalite/v6/api.py:254:30: error[invalid-argument-type] Argument to function `_dataset_name` is incorrect: Expected `dict[str, Any] | list[Unknown] | InlineDataset`, found `Unknown | UndefinedType | (@Todo(Support for `typing.TypeAlias`) & ~InlineDataset)`
+ altair/vegalite/v6/api.py:515:38: warning[possibly-unbound-attribute] Attribute `select` on type `(Unknown & ~VariableParameter) | (UndefinedType & ~VariableParameter)` is possibly unbound
- altair/vegalite/v6/api.py:2052:49: error[invalid-argument-type] Argument to function `compile_with_vegafusion` is incorrect: Expected `dict[str, Any]`, found `Unknown | MutableMapping[Any, Any]`
- altair/vegalite/v6/api.py:2060:20: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any]`, found `Unknown | MutableMapping[Any, Any]`
+ altair/vegalite/v6/api.py:3878:13: error[invalid-assignment] Object of type `Facet | (@Todo(unknown type subscript) & ~str) | (UndefinedType & ~str)` is not assignable to `Facet | FacetMapping`
+ altair/vegalite/v6/api.py:4215:44: error[invalid-argument-type] Argument to function `_repeat_names` is incorrect: Expected `list[str] | LayerRepeatMapping | RepeatMapping`, found `@Todo(unknown type subscript) | UndefinedType`
- tests/examples_arguments_syntax/interval_selection_map_quakes.py:10:29: error[unresolved-import] Module `altair.datasets` has no member `data`
- tests/examples_arguments_syntax/maps_faceted_species.py:11:29: error[unresolved-import] ...*[Comment body truncated]*

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:18 on 2025-08-06 21:35_

minor nit: I think this internal implementation of `__getattr__` is unnecessarily complex, because none of it has any impact on the type system's understanding of this `__getattr__`, and it raises questions for the reader that aren't really part of the purpose of this test, such as, would the type checker emit an error if some attribute other than `"whatever"` were accessed on this module? (Answer, we would not and cannot, but this test somehow implies that we might.)
```suggestion
    return "hi"
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:8 on 2025-08-06 21:35_

```suggestion
# Should work: module `__getattr__` returns `str`
```

This should talk about the annotated types, which is what matters to the type checker, not the implementation of the method, which is irrelevant here

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:36 on 2025-08-06 21:39_

This test seems fully redundant with the previous "basic functionality" test? Is it just confirming that we actually got `str` from the type annotation in the previous test, we didn't just assume it from thin air? I'm not sure that requires a second test.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:58 on 2025-08-06 21:39_

```suggestion
    return "dynamic"
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:1 on 2025-08-06 21:46_

Great thorough tests!

It would be good to also have a test showing the precedence with submodules; that is, that if `mod/__init__.py` has a `__getattr__`, and there also exists a `mod/sub.py`, that if we have `import mod; reveal_type(mod.sub)` we see the return type of `__getattr__`, but if we have `import mod.sub; reveal_type(mod.sub)` we see the submodule.

(I already checked this works correctly in this PR; it would just be ideal to test it.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:79 on 2025-08-06 21:47_

How is this different from the previous test? It seems like the same thing, just with a function instead of an assigned attribute for the "real attribute" -- I'm not sure we need to test both of those (there's nowhere in this PR that we have to handle them differently)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:4666 on 2025-08-06 21:55_

I think David's suggestion to extract this method was based on the theory that we'd reuse it for module-getattr, which we don't do in this PR. Given that we don't reuse it, I would probably revert the extraction; I don't think we need it as a full method if it's still just used in one place.

(At the very least we should remove the `allow_module_type_getattr` flag, which only ever receives the value `false`.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8304 on 2025-08-06 21:57_

I think it's correct that you didn't actually reuse the existing call-getattr logic here, because it doesn't really apply. E.g. type-level `__getattr__` has to be loaded from the meta-type (as with any other dunder method), so we use `try_call_dunder` -- but that doesn't apply to module-level getattr. The logic you have here looks correct to me.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8324 on 2025-08-06 21:58_

Arguably the boundness here should match the boundness of `__getattr__` itself (which is thrown away using `_` above), but I really don't think this matters in practice.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8368 on 2025-08-06 21:59_

There's really no point in checking for unbound here, since we are only here in the first place if `result` is unbound, and one unbound is as good as another.
```suggestion
            return self.try_module_getattr(db, name);
```

---

_@carljm requested changes on 2025-08-06 21:59_

Thank you!

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:36 on 2025-08-06 22:25_

Yup sorry I should have rewritten a bit the tests afterwards. I'll clean that

---

_@PrettyWood reviewed on 2025-08-06 22:25_

---

_@PrettyWood reviewed on 2025-08-06 22:26_

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:79 on 2025-08-06 22:26_

Yup should be removed

---

_@PrettyWood reviewed on 2025-08-06 22:27_

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/src/types.rs`:8324 on 2025-08-06 22:27_

I will check. Thank you for the deep review

---

_Comment by @PrettyWood on 2025-08-07 04:54_

Thanks a lot for the detailed review @carljm. Really appreciate. I’ve taken all your feedback into account, and it’s ready for another look. The ecosystem you're building with uv, ruff, and now ty is genuinely impressive. It's raising the bar for the entire python community. Huge respect for the work you're doing.

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:63 on 2025-08-07 04:58_

If you know how I can merge cases 1 and 2 together, feel free to update directly the PR but since we are doing static analysis, we can't play with `sys.modules` or anything like that

---

_@PrettyWood reviewed on 2025-08-07 04:58_

---

_Label `ty` added by @AlexWaygood on 2025-08-07 09:19_

---

_Closed by @PrettyWood on 2025-08-07 15:51_

---

_Branch deleted on 2025-08-07 15:51_

---

_Branch restored on 2025-08-07 15:51_

---

_Reopened by @PrettyWood on 2025-08-07 15:51_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:63 on 2025-08-08 00:00_

You could share the definitions of `mod/__init__.py` and `mod/sub.py` and then have two different named files (one that does `import mod` and the other that does `import mod.sub`), all in the same test.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:1 on 2025-08-08 00:07_

I thought of one other case we are missing. We don't necessarily need to add support for it in this PR (though if you are up for it, that's also fine!), but I think we should at least add a test for it with TODO comment to help us keep track of the fact that it's missing.

At runtime if module `mod.py` has `__getattr__` implementation, you can also do `from mod import whatever` and it will exercise the `__getattr__`. Currently this PR doesn't implement that, it only implements attribute access. To do it for imports as well, we'd need to add a similar case in the `imported_symbol` function (in `place.rs`) as a fallback. We would probably want to refactor your `try_module_getattr` method to be a standalone function instead that takes a `&Db` and a `File`, so we can call it also from `imported_symbol`.

---

_@carljm reviewed on 2025-08-08 00:07_

This looks good, thank you!

I realized we are missing the case of `from mod import foo`, we only handle `import mod; mod.foo` in this PR.

---

_@PrettyWood reviewed on 2025-08-08 05:38_

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:1 on 2025-08-08 05:38_

Maybe I didn't understand properly what you meant.
The flow is already `from mod import whatever` → `infer_import_from_definition()` → `module_ty.member()` → `module.static_member()` → `try_module_getattr()` (if normal lookup failed)
I added a test that passes that does what you wrote (again I may have misunderstood)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/module_getattr.md`:1 on 2025-08-08 17:38_

Oh! My bad, I thought I had tested this and it wasn't working, but I must have done something wrong. That's great that we actually only need to do this in one place :) The new test looks excellent, thank you!

---

_@carljm approved on 2025-08-08 17:38_

Excellent work, thank you!!

---

_Merged by @carljm on 2025-08-08 17:39_

---

_Closed by @carljm on 2025-08-08 17:39_

---

_Branch deleted on 2025-08-08 17:48_

---
