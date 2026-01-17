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
updated_at: 2026-01-17T00:17:59Z
url: https://github.com/astral-sh/ruff/pull/22562
synced_at: 2026-01-17T01:09:41Z
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


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-13 23:03_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dulwich (https://github.com/dulwich/dulwich)
+ dulwich/index.py:810:20: error[invalid-argument-type] Argument to function `sha_to_hex` is incorrect: Expected `RawObjectID`, found `bytes`
- Found 231 diagnostics
+ Found 232 diagnostics

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
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

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
- Found 11068 diagnostics
+ Found 11072 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- win32/Demos/win32gui_menu.py:434:67: error[invalid-argument-type] Argument to function `ExtTextOut` is incorrect: Expected `PyRECT`, found `tuple[Any, Any, Any, Any]`
+ win32/Demos/win32gui_menu.py:434:67: error[invalid-argument-type] Argument to function `ExtTextOut` is incorrect: Expected `PyRECT`, found `tuple[int, int, int, int]`
+ win32/Lib/win32gui_struct.py:946:41: error[invalid-argument-type] Argument to function `IID` is incorrect: Expected `str`, found `bytes`
+ win32/Lib/win32gui_struct.py:950:41: error[invalid-argument-type] Argument to function `IID` is incorrect: Expected `str`, found `bytes`
- Found 2712 diagnostics
+ Found 2714 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1827 diagnostics
+ Found 1826 diagnostics


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

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/call/bind.rs`:1261 on 2026-01-16 19:02_

This loop should probably be bounded to some reasonable limit, and beyond that we should fall back to some other type? Currently I can exhaust all the memory on my machine and get `ty` OOM killed by checking this :)

```py
def _(buf: bytes):
    reveal_type(struct.unpack("18446744073709551616c", buf))
```

Whatever we choose to do about this, it would make a good extra test case.

---

_@sakgoyal reviewed on 2026-01-16 23:18_

---

_Review comment by @sakgoyal on `crates/ty_python_semantic/src/types/call/bind.rs`:1261 on 2026-01-16 23:18_

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

_Review comment by @dscorbett on `crates/ty_python_semantic/src/types/call/bind.rs`:1261 on 2026-01-17 00:17_

It could also be a `tuple[bytes, ...]`, ignoring the count but keeping the type. Similarly, `struct.unpack("18446744073709551616c?", buf)` could be a `tuple[bytes | bool, ...]`.

---
