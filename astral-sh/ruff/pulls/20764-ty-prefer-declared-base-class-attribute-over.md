```yaml
number: 20764
title: "[ty] Prefer declared base class attribute over inferred attribute on subclass"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1067
created_at: 2025-10-08T12:40:23Z
updated_at: 2025-10-13T07:34:13Z
url: https://github.com/astral-sh/ruff/pull/20764
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Prefer declared base class attribute over inferred attribute on subclass

---

_Pull request opened by @sharkdp on 2025-10-08 12:40_

## Summary

When accessing an (instance) attribute on a given class, we were previously traversing its MRO, and building a union of types (if the attribute was available on multiple classes in the MRO) until we found a *definitely bound* symbol. The idea was that possibly unbound symbols in a subclass might only partially shadow the underlying base class attribute.

This behavior was problematic for two reasons:
* if the attribute was definitely bound on a class (e.g. `self.x = None`), we would have stopped iterating, even if there might be a `x: str | None` declaration in a base class (the bug reported in https://github.com/astral-sh/ty/issues/1067).
* if the attribute originated from an implicit instance attribute assignment (e.g. `self.x = 1` in method `Sub.foo`), we might stop looking and miss another implicit instance attribute assignment in a base class method (e.g. `self.x = 2` in method `Base.bar`).

With this fix, we still iterate the MRO of the class, but we only stop iterating if we find a *definitely declared* symbol. In this case, we only return the declared attribute type. Otherwise, we keep building a union of inferred attribute types.

The implementation here seemed to be the easiest fix for https://github.com/astral-sh/ty/issues/1067 that also kept the ecosystem impact low (the changes that I see all look correct). However, as the Markdown tests show, there are other things to fix in this area. For example, we should do a similar thing for *class attributes*. This is more involved, though (affects many different areas and probably involves a change to our descriptor protocol implementation), so I'd like to postpone this to a follow-up.

closes https://github.com/astral-sh/ty/issues/1067

## Test Plan

Updated Markdown tests, including a regression test for https://github.com/astral-sh/ty/issues/1067.


---

_Label `ty` added by @sharkdp on 2025-10-08 12:40_

---

_Comment by @github-actions[bot] on 2025-10-08 12:42_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `bug` added by @sharkdp on 2025-10-08 12:44_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-08 12:46_

---

_Comment by @github-actions[bot] on 2025-10-08 12:51_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unsupported-operator` | 0 | 0 | 514 |
| `possibly-missing-attribute` | 13 | 10 | 11 |
| `invalid-argument-type` | 5 | 11 | 2 |
| `unresolved-attribute` | 9 | 0 | 0 |
| `invalid-assignment` | 2 | 0 | 1 |
| `non-subscriptable` | 2 | 0 | 0 |
| `not-iterable` | 2 | 0 | 0 |
| **Total** | **33** | **21** | **528** |

**[Full report with detailed diff](https://david-fix-1067.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1067.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-08 15:08_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-08 15:08_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-08 16:04_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-08 16:04_

---

_Comment by @github-actions[bot] on 2025-10-08 16:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pip (https://github.com/pypa/pip)
+ src/pip/_internal/cli/index_command.py:133:9: error[invalid-assignment] Object of type `Any` is not assignable to attribute `prompting` on type `Unknown | MultiDomainBasicAuth | None`
+ src/pip/_internal/cli/index_command.py:134:9: error[invalid-assignment] Object of type `Any` is not assignable to attribute `keyring_provider` on type `Unknown | MultiDomainBasicAuth | None`
- Found 461 diagnostics
+ Found 463 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/worktree.py:942:28: error[invalid-argument-type] Method `__getitem__` of type `bound method DiskRefsContainer.__getitem__(name: bytes) -> bytes` cannot be called with key of type `str | bytes` on object of type `DiskRefsContainer`
+ dulwich/worktree.py:942:28: error[invalid-argument-type] Method `__getitem__` of type `bound method RefsContainer.__getitem__(name: bytes) -> bytes` cannot be called with key of type `str | bytes` on object of type `RefsContainer`
+ dulwich/worktree.py:947:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method RefsContainer.__setitem__(name: bytes, ref: bytes) -> None)` cannot be called with a key of type `str | bytes` and a value of type `bytes` on object of type `Unknown | RefsContainer`
- dulwich/worktree.py:942:28: error[invalid-argument-type] Method `__getitem__` of type `bound method ReftableRefsContainer.__getitem__(name: bytes) -> bytes` cannot be called with key of type `str | bytes` on object of type `ReftableRefsContainer`
- dulwich/worktree.py:947:17: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method DiskRefsContainer.__setitem__(name: bytes, ref: bytes) -> None) | (bound method ReftableRefsContainer.__setitem__(name: bytes, ref: bytes) -> None)` cannot be called with a key of type `str | bytes` and a value of type `bytes` on object of type `Unknown | DiskRefsContainer | ReftableRefsContainer`
- dulwich/worktree.py:972:48: error[invalid-argument-type] Argument to bound method `set_symbolic_ref` is incorrect: Expected `bytes`, found `str | bytes`
- Found 186 diagnostics
+ Found 184 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/iostream_test.py:1128:34: warning[possibly-missing-attribute] Attribute `cipher` on type `Unknown | None | SSLSocket` may be missing
+ tornado/test/iostream_test.py:1128:34: warning[possibly-missing-attribute] Attribute `cipher` on type `Unknown | None | socket` may be missing

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/transaction_processor/level_1_actions.py:181:54: error[invalid-argument-type] Argument to function `set_current_block_level_sync` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/transaction_processor/level_1_actions.py:182:59: error[invalid-argument-type] Argument to function `schedule_block_for_broadcast_sync` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/transaction_processor/level_2_actions.py:54:37: error[invalid-argument-type] Argument to function `verify_transaction_count` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/transaction_processor/level_2_actions.py:54:53: error[invalid-argument-type] Argument to function `verify_transaction_count` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/transaction_processor/level_2_actions.py:107:42: error[invalid-argument-type] Argument to function `get_verifying_keys` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/transaction_processor/level_3_actions.py:144:45: error[invalid-argument-type] Argument to function `get_verifying_keys` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/transaction_processor/level_3_actions.py:149:63: error[invalid-argument-type] Argument to function `get_registration` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/transaction_processor/level_4_actions.py:143:42: error[invalid-argument-type] Argument to function `get_verifying_keys` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/transaction_processor/level_5_actions.py:140:27: error[invalid-argument-type] Argument to function `set_last_block_number` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/transaction_processor/level_5_actions.py:196:65: error[invalid-argument-type] Argument to function `put_document` is incorrect: Expected `str`, found `str | Unknown | None`
- dragonchain/webserver/lib/dragonnet.py:89:86: error[invalid-argument-type] Argument to function `add_receipt` is incorrect: Expected `str`, found `str | Unknown | None`
- Found 316 diagnostics
+ Found 305 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ test/test_http2_connection.py:133:9: warning[possibly-missing-attribute] Attribute `sendall` on type `socket | @Todo | SSLTransport | None` may be missing
+ test/test_http2_connection.py:150:9: warning[possibly-missing-attribute] Attribute `sendall` on type `socket | @Todo | SSLTransport | None` may be missing
+ test/test_http2_connection.py:173:9: warning[possibly-missing-attribute] Attribute `sendall` on type `socket | @Todo | SSLTransport | None` may be missing
+ test/test_http2_connection.py:205:17: warning[possibly-missing-attribute] Attribute `sendall` on type `socket | @Todo | SSLTransport | None` may be missing
+ test/test_http2_connection.py:229:13: warning[possibly-missing-attribute] Attribute `sendall` on type `socket | @Todo | SSLTransport | None` may be missing
+ test/test_http2_connection.py:246:19: warning[possibly-missing-attribute] Attribute `sendall` on type `socket | @Todo | SSLTransport | None` may be missing
+ test/test_http2_connection.py:279:19: warning[possibly-missing-attribute] Attribute `sendall` on type `socket | @Todo | SSLTransport | None` may be missing
+ test/test_http2_connection.py:312:19: warning[possibly-missing-attribute] Attribute `sendall` on type `socket | @Todo | SSLTransport | None` may be missing
+ test/test_http2_connection.py:334:19: warning[possibly-missing-attribute] Attribute `sendall` on type `socket | @Todo | SSLTransport | None` may be missing
- Found 359 diagnostics
+ Found 368 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ pandera/api/pyspark/container.py:585:22: error[not-iterable] Object of type `Unknown | (list[Unknown] & ~Check) | list[Unknown | (list[Unknown] & Check)] | None` may not be iterable
+ tests/pandas/test_schemas.py:619:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | (list[Unknown] & ~Check) | list[Unknown | (list[Unknown] & Check)] | None`
+ tests/pandas/test_schemas.py:620:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | (list[Unknown] & ~Check) | list[Unknown | (list[Unknown] & Check)] | None`
+ tests/pandas/test_schemas.py:621:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | (list[Unknown] & ~Check) | list[Unknown | (list[Unknown] & Check)] | None`
- Found 1593 diagnostics
+ Found 1597 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ tests/test_tstring.py:216:25: warning[possibly-missing-attribute] Attribute `query` on type `PostgresQuery | None` may be missing
+ tests/test_tstring.py:217:25: warning[possibly-missing-attribute] Attribute `params` on type `PostgresQuery | None` may be missing
- tests/test_tstring.py:217:25: warning[possibly-missing-attribute] Attribute `decode` on type `Unknown | None` may be missing
+ tests/test_tstring.py:217:25: warning[possibly-missing-attribute] Attribute `decode` on type `@Todo | None` may be missing
+ tests/test_tstring.py:218:27: warning[possibly-missing-attribute] Attribute `query` on type `PostgresQuery | None` may be missing
+ tests/test_tstring.py:219:27: warning[possibly-missing-attribute] Attribute `params` on type `PostgresQuery | None` may be missing
- tests/test_tstring.py:219:27: warning[possibly-missing-attribute] Attribute `decode` on type `Unknown | None` may be missing
+ tests/test_tstring.py:219:27: warning[possibly-missing-attribute] Attribute `decode` on type `@Todo | None` may be missing
- Found 686 diagnostics
+ Found 690 diagnostics

manticore (https://github.com/trailofbits/manticore)
- tests/native/test_x86_pcmpxstrx.py:8872:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:8872:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:8876:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:8876:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:8959:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:8959:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:8963:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:8963:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9046:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9046:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9050:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9050:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9133:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9133:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9137:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9137:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9220:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9220:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9224:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9224:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9307:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9307:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9311:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9311:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9394:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9394:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9398:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9398:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9481:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9481:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9485:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9485:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9568:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9568:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9572:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9572:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9655:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9655:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9659:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9659:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9742:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9742:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9746:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9746:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9829:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9829:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9833:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9833:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9916:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9916:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:9920:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:9920:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10003:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10003:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10007:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10007:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10090:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10090:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10094:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10094:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10177:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10177:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10181:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10181:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10264:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10264:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10268:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10268:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10351:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10351:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10355:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10355:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10438:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10438:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10442:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10442:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10525:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10525:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10529:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10529:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10612:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10612:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10616:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10616:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10699:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10699:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10703:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10703:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10786:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10786:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10790:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10790:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10873:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10873:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10877:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10877:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10960:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10960:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:10964:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:10964:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11047:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11047:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11051:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11051:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11134:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11134:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11138:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11138:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11221:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11221:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11225:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11225:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11308:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11308:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11312:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11312:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11395:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11395:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11399:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11399:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11482:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11482:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11486:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11486:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11569:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11569:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11573:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11573:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11656:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11656:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11660:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11660:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11743:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11743:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11747:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11747:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11830:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11830:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11834:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11834:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11917:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11917:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:11921:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:11921:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12004:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12004:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12008:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12008:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12091:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12091:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12095:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12095:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12178:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12178:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12182:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12182:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12265:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12265:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12269:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12269:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12352:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12352:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12356:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12356:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12439:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12439:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12443:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12443:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12526:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12526:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12530:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12530:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12613:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12613:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12617:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12617:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12700:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12700:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12704:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12704:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12787:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12787:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12791:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12791:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12874:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12874:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12878:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12878:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12961:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12961:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:12965:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:12965:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13048:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13048:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13052:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13052:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13135:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13135:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13139:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13139:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13222:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13222:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13226:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13226:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13309:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13309:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13313:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13313:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13396:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13396:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13400:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13400:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13483:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13483:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13487:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13487:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13570:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13570:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13574:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13574:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13657:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13657:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13661:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13661:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13744:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13744:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13748:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13748:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13831:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13831:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13835:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13835:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13918:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13918:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:13922:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:13922:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14005:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14005:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14009:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14009:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14092:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14092:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14096:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14096:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14179:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14179:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14183:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14183:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14266:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14266:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14270:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14270:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14353:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14353:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14357:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14357:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14438:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14438:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14442:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14442:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14522:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14522:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14526:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14526:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14606:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14606:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14610:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14610:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14690:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14690:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14694:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14694:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14774:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14774:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14778:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14778:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14858:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14858:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14862:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14862:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14942:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14942:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:14946:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:14946:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15026:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15026:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15030:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15030:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15110:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15110:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15114:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15114:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15194:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15194:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15198:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15198:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15278:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15278:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15282:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15282:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15362:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15362:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15366:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15366:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15446:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15446:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15450:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15450:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15530:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15530:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15534:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15534:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15614:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15614:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15618:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15618:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15698:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15698:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15702:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15702:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15782:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15782:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15786:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15786:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15866:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15866:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15870:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15870:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15950:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15950:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:15954:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:15954:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:16034:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:16034:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:16038:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression` and `int`
+ tests/native/test_x86_pcmpxstrx.py:16038:29: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | int | Expression | None` and `int`
- tests/native/test_x86_pcmpxstrx.py:16118:34: error[unsupported-operator] Operator `+` is unsup...*[Comment body truncated]*

---

_@sharkdp reviewed on 2025-10-08 18:27_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/place.rs`:238 on 2025-10-08 18:27_

We could think about moving this to the newly created `member` module...

---

_@sharkdp reviewed on 2025-10-08 18:44_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/member.rs`:10 on 2025-10-08 18:44_

I don't *love* the fact that we now have this `Member(PlaceAndQualifiers(Place(Type)))` onion structure, but each single layer has its own dedicated purpose and is used in a lot of places.

I considered adding `is_declared` to `PlaceAndQualifiers` somehow, as this flag might be relevant in other areas as well. This would be a huge refactoring, but more importantly, I think that we probably want to add new flags to `Member` soon (or use `bitflags`) in order to convey even more metadata (e.g. is this an implicit instance attribute? is the attribute a data/non-data descriptor?).

---

_Marked ready for review by @sharkdp on 2025-10-08 18:47_

---

_Review requested from @carljm by @sharkdp on 2025-10-08 18:47_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-08 18:47_

---

_Review requested from @dcreager by @sharkdp on 2025-10-08 18:47_

---

_@sharkdp reviewed on 2025-10-08 18:56_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2892 on 2025-10-08 18:56_

This (and the few lines below) is the key change. Everything else is just refactoring (and recording whether or not a particular `Place` is declared or inferred).

---

_@sharkdp reviewed on 2025-10-08 18:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/member.rs`:28 on 2025-10-08 18:57_

Maybe `non_declared` or `inferred` is clearer?

---

_Comment by @AlexWaygood on 2025-10-09 18:01_

> ```diff
> pip (https://github.com/pypa/pip)
> + src/pip/_internal/cli/index_command.py:133:9: error[invalid-assignment] Object of type `Any` is not assignable to attribute `prompting` on type `Unknown | MultiDomainBasicAuth | None`
> + src/pip/_internal/cli/index_command.py:134:9: error[invalid-assignment] Object of type `Any` is not assignable to attribute `keyring_provider` on type `Unknown | MultiDomainBasicAuth | None`
> - Found 460 diagnostics
> + Found 462 diagnostics
> ```

That's a weird diagnostic. Shouldn't `Any` be assignable to everything?

---

_Comment by @sharkdp on 2025-10-09 18:15_

> That's a weird diagnostic. Shouldn't `Any` be assignable to everything?

I stumbled over the exact same thing and assumed it was just me. But we should definitely improve the diagnostic message here\*. The problem is not the type of the object that we're trying to assign. The problem is in the type of the object that we're acessing the attribute *on* (`None` doesn't have a `prompting` attribute).

> Object of type `Any` is not assignable to **attribute `prompting` on type `Unknown | MultiDomainBasicAuth | None`**

\* ideally by adding a subdiagnostic that explains the underlying reason why the assignment to the attribute *on that union type* has failed (because the assignment to that attribute on one particular element failed).

---

_Comment by @AlexWaygood on 2025-10-09 18:18_

Ahhh, I see. Thanks!

In that case, the type of the object we're trying to assign to the `prompting` attribute seems irrelevant? Shouldn't we just say something like this in the diagnostic?

```
Attribute `prompting` may not be settable on object of type `Unknown | MultiDomainBasicAuth | None`
```

---

_Comment by @sharkdp on 2025-10-09 18:20_

> Shouldn't we just say something like this in the diagnostic

That would be one way to improve the diagnostic, yes. The other thing that I proposed would also split up that union type and add to yours: "… because it can not be set on union element `None`", or similar.

---

_Comment by @AlexWaygood on 2025-10-09 18:21_

yes -- both sound good ;) but not for this PR, of course.

---

_@sharkdp reviewed on 2025-10-10 07:47_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/member.rs`:28 on 2025-10-10 07:47_

Changed to `inferred`, also added some doc comments.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/member.rs`:10 on 2025-10-10 23:28_

This feels like it intersects with discussions we've had a long time ago already (and several TODO comments we have) about how the `Boundness` part of `Place` currently conflates both "boundness" and "declaredness" in a way that isn't always consistent. I think we always vaguely had the intention of cleaning that up, but it never became a priority. it seems like this new flag is providing the same metadata that would be provided by that refactor, but at a new outer layer, mostly in order to avoid the need for a big refactor of `Place`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/class.rs`:2894 on 2025-10-10 23:46_

Don't we walk _up_ the MRO? So how could it be that we find a definitely-declared attribute and have to discard possibly collected inferred types from _superclasses_? Should this say _subclasses_ instead, or am I missing something?

---

_@carljm approved on 2025-10-10 23:48_

Looks good, thank you! I definitely think we should go ahead with this, as the impact is all positive. The `Member` struct seems like it could still make sense for other things we want to add to it, though it does seem to me like ideally bound-vs-declared would be recorded with better precision all the way down in `Place` instead.

---

_@sharkdp reviewed on 2025-10-13 07:18_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2894 on 2025-10-13 07:18_

> Should this say _subclasses_ instead

Yes, absolutely.

---

_Merged by @sharkdp on 2025-10-13 07:28_

---

_Closed by @sharkdp on 2025-10-13 07:28_

---

_Branch deleted on 2025-10-13 07:28_

---

_@sharkdp reviewed on 2025-10-13 07:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/member.rs`:10 on 2025-10-13 07:34_

I opened https://github.com/astral-sh/ty/issues/1341 to track this.

---
