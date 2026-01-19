```yaml
number: 22562
title: "[ty] `struct.unpack` return type inference"
type: pull_request
state: open
author: sakgoyal
labels:
  - ty
assignees: []
base: main
head: structunpack
created_at: 2026-01-13T22:55:50Z
updated_at: 2026-01-19T11:53:04Z
url: https://github.com/astral-sh/ruff/pull/22562
synced_at: 2026-01-19T12:32:36Z
```

# [ty] `struct.unpack` return type inference

---

_@sakgoyal_

https://github.com/astral-sh/ty/issues/2437

## Summary
Allow the checker to analyze the format string for `struct.unpack` to ensure correct types are generated instead of `Any`. 

## Test Plan
Added mdtest cases from original issue. 


> NOTE: the code in `types/call/bind.rs` was written by AI. the rest was written by me. 

---

_Review requested from @carljm by @sakgoyal on 2026-01-13 22:55_

---

_Review requested from @MichaReiser by @sakgoyal on 2026-01-13 22:55_

---

_Review requested from @AlexWaygood by @sakgoyal on 2026-01-13 22:55_

---

_Review requested from @Gankra by @sakgoyal on 2026-01-13 22:55_

---

_Review requested from @sharkdp by @sakgoyal on 2026-01-13 22:55_

---

_Review requested from @dcreager by @sakgoyal on 2026-01-13 22:55_

---

_Renamed from "`struct.unpack` Implementation attempt (with AI)" to "`struct.unpack` return type inference" by @sakgoyal on 2026-01-13 22:56_

---

_Comment by @astral-sh-bot[bot] on 2026-01-13 23:02_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-13 23:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dulwich (https://github.com/dulwich/dulwich)
+ dulwich/index.py:810:20: error[invalid-argument-type] Argument to function `sha_to_hex` is incorrect: Expected `RawObjectID`, found `bytes`
- Found 230 diagnostics
+ Found 231 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
- tornado/websocket.py:1220:51: error[invalid-argument-type] Argument to bound method `_handle_message` is incorrect: Expected `int`, found `Any | None`
+ tornado/websocket.py:1220:51: error[invalid-argument-type] Argument to bound method `_handle_message` is incorrect: Expected `int`, found `Unknown | int | None`

manticore (https://github.com/trailofbits/manticore)
+ manticore/wasm/executor.py:1194:14: error[invalid-assignment] Object of type `int` is not assignable to `F32`
+ manticore/wasm/executor.py:1202:14: error[invalid-assignment] Object of type `int` is not assignable to `F64`
+ manticore/wasm/executor.py:1505:14: error[invalid-assignment] Object of type `float` is not assignable to `I32`
+ manticore/wasm/executor.py:1513:14: error[invalid-assignment] Object of type `float` is not assignable to `I64`
- Found 11070 diagnostics
+ Found 11074 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- win32/Demos/win32gui_menu.py:434:67: error[invalid-argument-type] Argument to function `ExtTextOut` is incorrect: Expected `PyRECT`, found `tuple[Any, Any, Any, Any]`
+ win32/Demos/win32gui_menu.py:434:67: error[invalid-argument-type] Argument to function `ExtTextOut` is incorrect: Expected `PyRECT`, found `tuple[int, int, int, int]`
+ win32/Lib/win32gui_struct.py:946:41: error[invalid-argument-type] Argument to function `IID` is incorrect: Expected `str`, found `bytes`
+ win32/Lib/win32gui_struct.py:950:41: error[invalid-argument-type] Argument to function `IID` is incorrect: Expected `str`, found `bytes`
- Found 2694 diagnostics
+ Found 2696 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Yarn[Any] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1824 diagnostics
+ Found 1825 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/chain/decoding/tools.py:96:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rotkehlchen/chain/decoding/tools.py:99:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `Sequence[A@BaseDecoderTools]`, found `Unknown | tuple[BTCAddress, ...] | tuple[ChecksumAddress, ...] | tuple[SubstrateAddress, ...] | tuple[SolanaAddress, ...]`
- rotkehlchen/chain/decoding/tools.py:100:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ rotkehlchen/chain/decoding/tools.py:97:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress`, found `A@BaseDecoderTools`
+ rotkehlchen/chain/decoding/tools.py:98:13: error[invalid-argument-type] Argument to function `decode_transfer_direction` is incorrect: Expected `BTCAddress | ChecksumAddress | SubstrateAddress | SolanaAddress | None`, found `A@BaseDecoderTools | None`
- Found 2057 diagnostics
+ Found 2056 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @carljm on 2026-01-13 23:16_

Thanks! Please take a look at https://github.com/astral-sh/ruff/blob/main/crates/ty/CONTRIBUTING.md for help with resolving the CI failures. Looks like prek and clippy are failing, as well as the new mdtests you added. Putting the PR back in draft; please mark it ready for review once CI is green.

---

_Converted to draft by @carljm on 2026-01-13 23:16_

---

_Comment by @sakgoyal on 2026-01-14 02:09_

Will CI run in draft mode?

---

_Comment by @charliermarsh on 2026-01-14 02:09_

Yeah.

---

_Marked ready for review by @sakgoyal on 2026-01-14 02:37_

---

_Label `ty` added by @ntBre on 2026-01-14 14:39_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2026-01-15 15:03_

---

_Renamed from "`struct.unpack` return type inference" to "[ty] `struct.unpack` return type inference" by @AlexWaygood on 2026-01-16 11:04_

---

_@oconnor663 reviewed on 2026-01-16 19:02_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/call/bind.rs`:1260 on 2026-01-16 19:02_

This loop should probably be bounded to some reasonable limit, and beyond that we should fall back to some other type? Currently I can exhaust all the memory on my machine and get `ty` OOM killed by checking this :)

```py
def _(buf: bytes):
    reveal_type(struct.unpack("18446744073709551616c", buf))
```

Whatever we choose to do about this, it would make a good extra test case.

---

_@sakgoyal reviewed on 2026-01-16 23:18_

---

_Review comment by @sakgoyal on `crates/ty_python_semantic/src/types/call/bind.rs`:1260 on 2026-01-16 23:18_

thats a good point. maybe we put a limit at say 2^16? if it exceeds that, set the type to `Unknown`? 

eg:
```
reveal_type(struct.unpack("18446744073709551616c", buf))
reveal_type(struct.unpack("@i18446744073709551616c", buf))
```
should return `tuple[Unknown]`, `Unknown`, or `Any`?

i'm ok with either option, though i'd prefer `tuple[Unknown]`

---

_@dscorbett reviewed on 2026-01-17 00:17_

---

_Review comment by @dscorbett on `crates/ty_python_semantic/src/types/call/bind.rs`:1260 on 2026-01-17 00:17_

It could also be a `tuple[bytes, ...]`, ignoring the count but keeping the type. Similarly, `struct.unpack("18446744073709551616c?", buf)` could be a `tuple[bytes | bool, ...]`.

---

_@carljm reviewed on 2026-01-17 01:51_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:1260 on 2026-01-17 01:51_

I think it is not super important how precise the fallback type is in this edge case. Feel free to use the most precise fallback type that is definitely correct and doesn't require a lot of additional work to implement.

---

_@sakgoyal reviewed on 2026-01-17 08:01_

---

_Review comment by @sakgoyal on `crates/ty_python_semantic/src/types/call/bind.rs`:1260 on 2026-01-17 08:01_

I did a quick and dirty fix. I think it's probably fine, but I dont know rust well enough to know how to do this properly. 

---

_Comment by @sakgoyal on 2026-01-17 08:01_

Rolled up into 1 commit to make git history nicer if this gets merged

---

_@MichaReiser reviewed on 2026-01-19 11:53_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/call/bind.rs`:1245 on 2026-01-19 11:53_

Should we error / early return if we see any unsupported/unknown format specifiers?

---
