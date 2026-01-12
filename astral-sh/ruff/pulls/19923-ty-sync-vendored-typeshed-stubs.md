```yaml
number: 19923
title: "[ty] Sync vendored typeshed stubs"
type: pull_request
state: merged
author: github-actions
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-08-15T00:42:54Z
updated_at: 2025-08-15T01:09:36Z
url: https://github.com/astral-sh/ruff/pull/19923
synced_at: 2026-01-12T15:56:50Z
```

# [ty] Sync vendored typeshed stubs

---

_@github-actions_

Close and reopen this PR to trigger CI

---

_Label `ty` added by @github-actions[bot] on 2025-08-15 00:42_

---

_Review requested from @carljm by @github-actions[bot] on 2025-08-15 00:42_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-08-15 00:42_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-08-15 00:42_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-08-15 00:42_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-08-15 00:42_

---

_Closed by @carljm on 2025-08-15 00:47_

---

_Reopened by @carljm on 2025-08-15 00:47_

---

_Comment by @github-actions[bot] on 2025-08-15 00:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-15 01:02:15.039214285 +0000
+++ new-output.txt	2025-08-15 01:02:15.106215073 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1cc90)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(8047)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-15 00:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
+ pyinstrument/vendor/decorator.py:57:42: warning[deprecated] The function `getargspec` is deprecated: Deprecated since Python 3.0; removed in Python 3.11. Use `inspect.signature()` instead.
- Found 41 diagnostics
+ Found 42 diagnostics

stone (https://github.com/dropbox/stone)
- stone/frontend/ir_generator.py:9:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 79 diagnostics
+ Found 78 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
+ src/zope/interface/exceptions.py:202:27: warning[deprecated] The function `getargspec` is deprecated: Deprecated since Python 3.0; removed in Python 3.11. Use `inspect.signature()` instead.
+ src/zope/interface/exceptions.py:203:33: warning[deprecated] The function `formatargspec` is deprecated: Deprecated since Python 3.5; removed in Python 3.11. Use `inspect.signature()` and the `Signature` class instead.
- Found 338 diagnostics
+ Found 340 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_tests/test_util.py:116:22: warning[deprecated] The function `coroutine` is deprecated: Deprecated since Python 3.8; removed in Python 3.11. Use `async def` instead.
- Found 653 diagnostics
+ Found 654 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ src/urllib3/util/ssl_.py:125:9: error[invalid-assignment] Reassignment of `Final` symbol `HAS_NEVER_CHECK_COMMON_NAME` is not allowed: Reassignment of `Final` symbol
- test/test_http2_connection.py:133:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ test/test_http2_connection.py:133:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = Literal[0], /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- test/test_http2_connection.py:150:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ test/test_http2_connection.py:150:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = Literal[0], /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- test/test_http2_connection.py:173:9: warning[possibly-unbound-attribute] Attribute `assert_has_calls` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ test/test_http2_connection.py:173:9: warning[possibly-unbound-attribute] Attribute `assert_has_calls` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = Literal[0], /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- test/test_http2_connection.py:205:17: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ test/test_http2_connection.py:205:17: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = Literal[0], /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- test/test_http2_connection.py:229:13: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ test/test_http2_connection.py:229:13: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = Literal[0], /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- test/test_http2_connection.py:259:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ test/test_http2_connection.py:259:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = Literal[0], /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- test/test_http2_connection.py:292:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ test/test_http2_connection.py:292:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = Literal[0], /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- test/test_http2_connection.py:325:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ test/test_http2_connection.py:325:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = Literal[0], /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- test/test_http2_connection.py:347:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = ellipsis, /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
+ test/test_http2_connection.py:347:9: warning[possibly-unbound-attribute] Attribute `assert_called_with` on type `(bound method socket.sendall(data: @Todo(Support for `typing.TypeAlias`), flags: int = Literal[0], /) -> None) | @Todo(Support for `typing.TypeAlias`) | (bound method SSLTransport.sendall(data: bytes, flags: int = Literal[0]) -> None)` is possibly unbound
- Found 392 diagnostics
+ Found 393 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/autoreload.py:339:26: warning[deprecated] The function `get_loader` is deprecated: Use importlib.util.find_spec() instead. Will be removed in Python 3.14.
- Found 245 diagnostics
+ Found 244 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ tests/contrib/asyncio/utils.py:46:28: warning[deprecated] The function `coroutine` is deprecated: Deprecated since Python 3.8; removed in Python 3.11. Use `async def` instead.
- Found 6477 diagnostics
+ Found 6478 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Merged by @carljm on 2025-08-15 01:09_

---

_Closed by @carljm on 2025-08-15 01:09_

---

_Branch deleted on 2025-08-15 01:09_

---
