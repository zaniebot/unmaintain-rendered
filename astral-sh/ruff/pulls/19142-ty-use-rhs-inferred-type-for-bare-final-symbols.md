```yaml
number: 19142
title: "[ty] Use RHS inferred type for bare `Final` symbols"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/typing-final-type-inference
created_at: 2025-07-04T08:53:19Z
updated_at: 2025-07-07T11:16:41Z
url: https://github.com/astral-sh/ruff/pull/19142
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Use RHS inferred type for bare `Final` symbols

---

_Pull request opened by @sharkdp on 2025-07-04 08:53_

## Summary

Infer the type of symbols with a `Final` qualifier as their right-hand-side inferred type:
```py
x: Final = 1
y: Final[int] = 1

def _():
    reveal_type(x)  # previously: Unknown, now: Literal[1]
    reveal_type(y)  # int, same as before
```
Part of https://github.com/astral-sh/ty/issues/158

## Ecosystem analysis

### aiohttp

```diff
aiohttp (https://github.com/aio-libs/aiohttp)
+ error[invalid-argument-type] aiohttp/compression_utils.py:131:54: Argument to bound method `__init__` is incorrect: Expected `ZLibBackendProtocol`, found `<module 'zlib'>`
```

This code [creates a protocol](https://github.com/aio-libs/aiohttp/blob/a83597fa88be7ac7dd5f6081d236d751cb40fe4d/aiohttp/compression_utils.py#L52-L77) that looks like
```pyi
class ZLibBackendProtocol(Protocol):
    Z_FULL_FLUSH: int
    Z_SYNC_FLUSH: int
    # more fields…
```

It then [tries to assign](https://github.com/aio-libs/aiohttp/blob/a83597fa88be7ac7dd5f6081d236d751cb40fe4d/aiohttp/compression_utils.py#L131) the module literal `zlib` to that protocol. Howefer, in typeshed, these `zlib` members are annotated like this:
```pyi
Z_FULL_FLUSH: Final = 3
Z_SYNC_FLUSH: Final = 2
```
With the proposed change here, we now infer these as `Literal[3]` / `Literal[2]`. Since protocol members have to be assignable both ways (invariance), we do not consider `zlib` assignable to this protocol anymore.

That seems rather unfortunate. Not sure who is to blame here? That `ZLibBackendProtocol` protocol should probably not annotate the members with `int`, given that `typeshed` doesn't use an explicit annotation here either? But what should they do instead? Annotate those fields with `Any`?

Or is it another case where we should consider literal-widening?

FYI @AlexWaygood 

### cloud-init

```diff
cloud-init (https://github.com/canonical/cloud-init)
+ error[invalid-argument-type] tests/unittests/sources/test_smartos.py:575:32: Argument to function `oct` is incorrect: Expected `SupportsIndex`, found `int | float`
+ error[invalid-argument-type] tests/unittests/sources/test_smartos.py:593:32: Argument to function `oct` is incorrect: Expected `SupportsIndex`, found `int | float`
+ error[invalid-argument-type] tests/unittests/sources/test_smartos.py:647:35: Argument to function `oct` is incorrect: Expected `SupportsIndex`, found `int | float`
```

New false positives on expressions like `oct(os.stat(legacy_script_f)[stat.ST_MODE])`. We now correctly infer `stat.ST_MODE` as `Literal[1]`, because in typeshed, it is annotated as `ST_MODE: Final = 0`. `os.stat` returns a `stat_result` which is a tuple subclass. Accessing it at index 0 should return an `int`, but we currently return `int | float`, presumably due to missing support for tuple subclasses (FYI @AlexWaygood):
```pyi
class stat_result(structseq[float], tuple[int, int, int, int, int, int, int, float, float, float]):
```
In terms of `typing.Final`, things are working as expected here.


### pywin-32

Many new false positives similar to:

```diff
pywin32 (https://github.com/mhammond/pywin32)
+ error[invalid-argument-type] Pythonwin/pywin/docking/DockingBar.py:288:55: Argument to function `LoadCursor` is incorrect: Expected `PyResourceId`, found `Literal[32645]`
```

The line in question calls `win32api.LoadCursor(0, win32con.IDC_ARROW)`. The `win32con.IDC_ARROW` symbol is annotated as [`IDC_ARROW: Final = 32512` in typeshed](https://github.com/python/typeshed/blob/2408c028f4f677e0db4eabef0c70e9f6ab33e365/stubs/pywin32/win32/lib/win32con.pyi#L594), but [`LoadCursor`](https://github.com/python/typeshed/blob/2408c028f4f677e0db4eabef0c70e9f6ab33e365/stubs/pywin32/win32/win32api.pyi#L197) expects a [`PyResourceId`](https://github.com/python/typeshed/blob/2408c028f4f677e0db4eabef0c70e9f6ab33e365/stubs/pywin32/_win32typing.pyi#L1252), which is an empty class. So.. this seems like a true positive to me, unless that typeshed annotation of `IDC_ARROW` is meant to imply that the type should be `Unknown`/`Any`?

### streamlit

```diff
streamlit (https://github.com/streamlit/streamlit)
+ error[invalid-argument-type] lib/streamlit/string_util.py:163:37: Argument to bound method `translate` is incorrect: Expected `bytes`, found `bytearray`
```

This looks like a true positive? The code calls `inp.translate(None, TEXTCHARS)`. `inp` is `bytes`, and `TEXTCHARS` is:
```py
TEXTCHARS: Final = bytearray(
    {7, 8, 9, 10, 12, 13, 27} | set(range(0x20, 0x100)) - {0x7F}
)
```
~~We now infer this as `bytearray`, but `bytes.translate` [expects `bytes` for its `delete` parameter](https://github.com/python/typeshed/blob/2408c028f4f677e0db4eabef0c70e9f6ab33e365/stdlib/builtins.pyi#L710). This seems to work at runtime, so maybe the typeshed annotation is wrong?~~ (Edit: this is now fixed in typeshed)
```pycon
>>> b"abc".translate(None, bytearray(b"b"))
b'ac'
```

## rotki

```diff
+ error[invalid-return-type] rotkehlchen/chain/ethereum/modules/yearn/decoder.py:412:13: Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["yearn-v1", "yearn-v2"]]`
```

The code in question looks like
```py
    def addresses_to_counterparties(self) -> dict[ChecksumEvmAddress, str]:
        return dict.fromkeys(self.vaults, CPT_BEEFY_FINANCE)
```
where `CPT_BEEFY_FINANCE: Final = 'beefy_finance'. We previously inferred the value type of the returned `dict` as `Unknown`, and now we infer it as `Literal["beefy_finance"]`, which does not match the annotated return type because `dict` is invariant in the value type.

```diff
+ error[invalid-argument-type] rotkehlchen/tests/unit/decoders/test_curve.py:249:9: Argument is incorrect: Expected `int`, found `FVal`
```
There are true positives that were previously silenced through the `Unknown`.

## Test Plan

New Markdown tests

---

_Label `ty` added by @sharkdp on 2025-07-04 08:53_

---

_@sharkdp reviewed on 2025-07-04 08:53_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:13 on 2025-07-04 08:53_

Pretty sure I intended to write `FINAL_A: Final[int] = 1` here originally :-)

---

_Comment by @github-actions[bot] on 2025-07-04 08:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
-     memo fields = ~49MB
+     memo fields = ~54MB

beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~88MB

black (https://github.com/psf/black)
-     memo metadata = ~8MB
+     memo metadata = ~9MB

rich (https://github.com/Textualize/rich)
-     memo fields = ~117MB
+     memo fields = ~106MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- info[revealed-type] tests/annotations/declarations.py:122:9: Revealed type: `Unknown`
+ info[revealed-type] tests/annotations/declarations.py:122:9: Revealed type: `@Todo(unsupported nested subscript in type[X])`
- info[revealed-type] tests/annotations/declarations.py:233:9: Revealed type: `Unknown`
+ info[revealed-type] tests/annotations/declarations.py:233:9: Revealed type: `@Todo(unsupported nested subscript in type[X])`
- error[invalid-argument-type] tests/annotations/declarations.py:434:26: Argument to function `make_config` is incorrect: Expected `tuple[type[DataClass_], ...]`, found `tuple[type[DataClass], Unknown, <class 'P3'>, Unknown]`
+ error[invalid-argument-type] tests/annotations/declarations.py:434:26: Argument to function `make_config` is incorrect: Expected `tuple[type[DataClass_], ...]`, found `tuple[type[DataClass], @Todo(unsupported nested subscript in type[X]), <class 'P3'>, @Todo(unsupported nested subscript in type[X])]`
- warning[unused-ignore-comment] tests/annotations/declarations.py:495:15: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/annotations/declarations.py:859:31: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/annotations/declarations.py:1238:45: Unused blanket `type: ignore` directive
+ error[no-matching-overload] tests/annotations/declarations.py:1237:5: No overload of bound method `builds` matches arguments
+ error[no-matching-overload] tests/annotations/declarations.py:1436:5: No overload of bound method `builds` matches arguments
- warning[unused-ignore-comment] tests/annotations/declarations.py:1434:47: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] tests/annotations/declarations.py:1435:51: Unused blanket `type: ignore` directive
- Found 599 diagnostics
+ Found 596 diagnostics
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

pylox (https://github.com/sco1/pylox)
-     memo fields = ~49MB
+     memo fields = ~54MB

cloud-init (https://github.com/canonical/cloud-init)
+ error[invalid-argument-type] tests/unittests/sources/test_smartos.py:575:32: Argument to function `oct` is incorrect: Expected `SupportsIndex`, found `int | float`
+ error[invalid-argument-type] tests/unittests/sources/test_smartos.py:593:32: Argument to function `oct` is incorrect: Expected `SupportsIndex`, found `int | float`
+ error[invalid-argument-type] tests/unittests/sources/test_smartos.py:647:35: Argument to function `oct` is incorrect: Expected `SupportsIndex`, found `int | float`
- Found 663 diagnostics
+ Found 666 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- warning[unused-ignore-comment] pwndbg/dbg/gdb/symbol.py:188:57: Unused blanket `type: ignore` directive
- Found 2274 diagnostics
+ Found 2273 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
-     memo fields = ~80MB
+     memo fields = ~88MB

scrapy (https://github.com/scrapy/scrapy)
-     memo fields = ~207MB
+     memo fields = ~189MB

pywin32 (https://github.com/mhammond/pywin32)
+ error[invalid-argument-type] Pythonwin/pywin/Demos/dlgtest.py:111:56: Argument to bound method `CreateWindow` is incorrect: Expected `PyCWnd | None`, found `int`
+ error[invalid-argument-type] Pythonwin/pywin/docking/DockingBar.py:91:41: Argument to function `LoadCursor` is incorrect: Expected `PyResourceId`, found `Literal[32512]`
+ error[invalid-argument-type] Pythonwin/pywin/docking/DockingBar.py:288:55: Argument to function `LoadCursor` is incorrect: Expected `PyResourceId`, found `Literal[32645]`
+ error[invalid-argument-type] Pythonwin/pywin/docking/DockingBar.py:290:55: Argument to function `LoadCursor` is incorrect: Expected `PyResourceId`, found `Literal[32644]`
+ error[invalid-argument-type] Pythonwin/pywin/framework/scriptutils.py:63:28: Argument to function `CreateFileDialog` is incorrect: Expected `str | None`, found `Literal[4098]`
+ error[invalid-argument-type] Pythonwin/pywin/framework/scriptutils.py:426:28: Argument to function `CreateFileDialog` is incorrect: Expected `str | None`, found `Literal[4098]`
+ error[invalid-argument-type] Pythonwin/pywin/framework/sgrepmdi.py:445:28: Argument to function `CreateFileDialog` is incorrect: Expected `str | None`, found `Literal[2]`
+ error[invalid-argument-type] com/win32com/client/tlbrowse.py:78:55: Argument to function `CreateFileDialog` is incorrect: Expected `str | None`, found `Literal[4098]`
+ error[invalid-argument-type] com/win32com/server/register.py:33:26: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Unknown | Literal[-2147483648]`
+ error[invalid-argument-type] com/win32com/server/register.py:49:31: Argument to function `RegDeleteKey` is incorrect: Expected `PyHKEY`, found `Unknown | Literal[-2147483648]`
- error[invalid-argument-type] com/win32com/test/testClipboard.py:139:26: Argument to bound method `GetData` is incorrect: Expected `PyFORMATETC`, found `tuple[Unknown, None, int, Literal[-1], int]`
+ error[invalid-argument-type] com/win32com/test/testClipboard.py:139:26: Argument to bound method `GetData` is incorrect: Expected `PyFORMATETC`, found `tuple[Literal[1], None, int, Literal[-1], int]`
+ error[invalid-argument-type] win32/Demos/RegCreateKeyTransacted.py:11:5: Argument to function `RegCreateKeyEx` is incorrect: Expected `PyHKEY`, found `Literal[-2147483647]`
+ error[invalid-argument-type] win32/Demos/RegCreateKeyTransacted.py:22:5: Argument to function `RegOpenKeyTransacted` is incorrect: Expected `PyHKEY`, found `Literal[-2147483647]`
+ error[invalid-argument-type] win32/Demos/RegCreateKeyTransacted.py:60:23: Argument to function `RegDeleteKey` is incorrect: Expected `PyHKEY`, found `Literal[-2147483647]`
- error[invalid-argument-type] win32/Demos/RegRestoreKey.py:30:61: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/RegRestoreKey.py:30:61: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]]]`
+ error[invalid-argument-type] win32/Demos/RegRestoreKey.py:38:9: Argument to function `RegCreateKeyEx` is incorrect: Expected `PyHKEY`, found `Literal[-2147483647]`
+ error[invalid-argument-type] win32/Demos/RegRestoreKey.py:62:9: Argument to function `RegCreateKeyEx` is incorrect: Expected `PyHKEY`, found `Literal[-2147483647]`
- error[invalid-argument-type] win32/Demos/security/account_rights.py:25:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/account_rights.py:25:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[invalid-argument-type] win32/Demos/security/explicit_entries.py:46:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/explicit_entries.py:46:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[invalid-argument-type] win32/Demos/security/list_rights.py:25:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/list_rights.py:25:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[invalid-argument-type] win32/Demos/security/regsave_sa.py:37:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/regsave_sa.py:37:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[invalid-argument-type] win32/Demos/security/regsecurity.py:21:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/regsecurity.py:21:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[invalid-argument-type] win32/Demos/security/set_file_audit.py:34:61: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/set_file_audit.py:34:61: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[invalid-argument-type] win32/Demos/security/set_file_owner.py:43:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/set_file_owner.py:43:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[invalid-argument-type] win32/Demos/security/setnamedsecurityinfo.py:75:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/setnamedsecurityinfo.py:75:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[invalid-argument-type] win32/Demos/security/setsecurityinfo.py:74:56: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/setsecurityinfo.py:74:56: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[invalid-argument-type] win32/Demos/security/setuserobjectsecurity.py:74:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown], tuple[int, Unknown]]`
+ error[invalid-argument-type] win32/Demos/security/setuserobjectsecurity.py:74:44: Argument to function `AdjustTokenPrivileges` is incorrect: Expected `PyTOKEN_PRIVILEGES`, found `tuple[tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]], tuple[int, Literal[2]]]`
- error[too-many-positional-arguments] win32/Demos/service/pipeTestService.py:94:67: Too many positional arguments to function `GetNamedPipeHandleState`: expected 2, got 3
- warning[possibly-unresolved-reference] win32/Demos/service/pipeTestService.py:94:82: Name `d` used when possibly not defined
- error[invalid-argument-type] win32/Demos/win32gui_menu.py:147:35: Argument to function `Shell_NotifyIcon` is incorrect: Expected `PyNOTIFYICONDATA`, found `tuple[Unknown, Literal[0], int, Unknown, PyGdiHANDLE | PyWNDCLASS, Literal["Python Demo"]]`
+ error[invalid-argument-type] win32/Demos/win32gui_menu.py:147:35: Argument to function `Shell_NotifyIcon` is incorrect: Expected `PyNOTIFYICONDATA`, found `tuple[Unknown, Literal[0], int, Literal[1044], PyGdiHANDLE | PyWNDCLASS, Literal["Python Demo"]]`
+ error[invalid-argument-type] win32/Demos/win32gui_taskbar.py:26:45: Argument to function `LoadCursor` is incorrect: Expected `PyResourceId`, found `Literal[32512]`
- error[invalid-argument-type] win32/Demos/win32gui_taskbar.py:78:57: Argument to function `Shell_NotifyIcon` is incorrect: Expected `PyNOTIFYICONDATA`, found `tuple[Unknown, Literal[0], int, Unknown, PyGdiHANDLE | PyWNDCLASS, Literal["Python Demo"]]`
+ error[invalid-argument-type] win32/Demos/win32gui_taskbar.py:78:57: Argument to function `Shell_NotifyIcon` is incorrect: Expected `PyNOTIFYICONDATA`, found `tuple[Unknown, Literal[0], int, Literal[1044], PyGdiHANDLE | PyWNDCLASS, Literal["Python Demo"]]`
+ error[invalid-argument-type] win32/Lib/regutil.py:297:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:300:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:303:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:309:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:316:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:319:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:328:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:331:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:337:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:344:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:347:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:362:13: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:369:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:379:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:385:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/regutil.py:391:9: Argument to function `RegSetValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483648]`
+ error[invalid-argument-type] win32/Lib/win32evtlogutil.py:99:13: Argument to function `RegDeleteKey` is incorrect: Expected `PyHKEY`, found `Literal[-2147483646]`
+ error[invalid-argument-type] win32/scripts/regsetup.py:213:59: Argument to function `CreateFileDialog` is incorrect: Expected `str | None`, found `Literal[4096]`
+ error[invalid-argument-type] win32/test/test_win32api.py:80:39: Argument to function `RegDeleteKey` is incorrect: Expected `PyHKEY`, found `Literal[-2147483647]`
+ error[invalid-argument-type] win32/test/test_win32api.py:123:39: Argument to function `RegDeleteKey` is incorrect: Expected `PyHKEY`, found `Literal[-2147483647]`
+ error[invalid-argument-type] win32/test/test_win32api.py:131:13: Argument to function `RegNotifyChangeKeyValue` is incorrect: Expected `PyHKEY`, found `Literal[-2147483647]`
- Found 1973 diagnostics
+ Found 2008 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~251MB

psycopg (https://github.com/psycopg/psycopg)
-     memo fields = ~171MB
+     memo fields = ~189MB

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
-     memo metadata = ~28MB
+     memo metadata = ~30MB

aiohttp (https://github.com/aio-libs/aiohttp)
+ error[invalid-argument-type] aiohttp/compression_utils.py:131:54: Argument to bound method `__init__` is incorrect: Expected `ZLibBackendProtocol`, found `<module 'zlib'>`
- Found 176 diagnostics
+ Found 177 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~189MB
+     memo fields = ~171MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
-     memo fields = ~593MB
+     memo fields = ~539MB

rotki (https://github.com/rotki/rotki)
+ error[invalid-return-type] rotkehlchen/chain/ethereum/modules/yearn/decoder.py:412:13: Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["yearn-v1", "yearn-v2"]]`
+ error[invalid-return-type] rotkehlchen/chain/evm/decoding/aave/v2/decoder.py:138:16: Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["aave-v2"]]`
+ error[invalid-return-type] rotkehlchen/chain/evm/decoding/beefy_finance/decoder.py:204:16: Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["beefy_finance"]]`
+ error[invalid-return-type] rotkehlchen/chain/evm/decoding/compound/v3/decoder.py:408:16: Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["compound-v3"]]`
+ error[invalid-return-type] rotkehlchen/chain/evm/decoding/llamazip/decoder.py:81:16: Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["llamazip"]]`
+ error[invalid-return-type] rotkehlchen/chain/evm/decoding/morpho/decoder.py:330:16: Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["morpho"]]`
+ error[invalid-return-type] rotkehlchen/chain/evm/decoding/uniswap/v3/decoder.py:596:16: Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["uniswap-v3"]]`
+ error[invalid-return-type] rotkehlchen/chain/evm/decoding/zerox/decoder.py:294:16: Return type does not match returned value: expected `dict[Unknown, str]`, found `dict[Unknown, Literal["0x"]]`
+ error[invalid-argument-type] rotkehlchen/tests/unit/decoders/test_curve.py:249:9: Argument is incorrect: Expected `int`, found `FVal`
+ error[invalid-argument-type] rotkehlchen/tests/unit/decoders/test_curve.py:565:9: Argument is incorrect: Expected `int`, found `FVal`
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:125:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:231:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:359:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:471:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:554:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:695:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:823:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:889:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:937:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:1021:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:1133:68: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:1217:98: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] rotkehlchen/tests/unit/globaldb/test_upgrades.py:1247:64: Unused blanket `type: ignore` directive
- Found 1728 diagnostics
+ Found 1725 diagnostics

```
</details>


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-04 08:58_

---

_Comment by @AlexWaygood on 2025-07-04 10:25_

> With the proposed change here, we now infer these as `Literal[3]` / `Literal[2]`. Since protocol members have to be assignable both ways (invariance), we do not consider `zlib` assignable to this protocol anymore.
> 
> That seems rather unfortunate. Not sure who is to blame here? That `ZLibBackendProtocol` protocol should probably not annotate the members with `int`, given that `typeshed` doesn't use an explicit annotation here either? But what should they do instead? Annotate those fields with `Any`?
> 
> Or is it another case where we should consider literal-widening?

I think our behaviour is correct here. Even if we put aside the issue of variance, the module still shouldn't satisfy `ZLibBackendProtocol`, because the protocol declares `Z_FULL_FLUSH` and `Z_SYNC_FLUSH` as mutable attribute members -- but because these members are declared as `Final` in the module, they therefore cannot be reassigned on the class, so they are immutable, so they should not satisfy the protocol interface. (We don't yet look at qualifiers when deciding subtyping/assignability of types to protocol types, but we'll need to in due course.)

For the module to satisfy the protocol, either the members on the module would need to be made mutable (and declared as `int` if the author wants precise types), e.g.

```diff
- Z_FULL_FLUSH: Final = 3
- Z_SYNC_FLUSH: Final = 2
+ Z_FULL_FLUSH: int = 3
+ Z_SYNC_FLUSH: int = 2
```

or the protocol would need to be changed so that it uses immutable, covariant members rather than mutable, invariant members. The only way to do that on a protocol currently (at least until https://peps.python.org/pep-0767/ is approved) is to use `@property` members:

```diff
  class ZLibBackendProtocol(Protocol):
-     Z_FULL_FLUSH: int
-     Z_SYNC_FLUSH: int
+     @property
+     def Z_FULL_FLUSH(self) -> int: ...
+     @property
+     def Z_SYNC_FLUSH(self) -> int: ...
```

---

_Comment by @AlexWaygood on 2025-07-04 10:27_

> So.. this seems like a true positive to me, unless that typeshed annotation of `IDC_ARROW` is meant to imply that the type should be `Unknown`/`Any`?

yeah, that does sound like a true positive. Possibly a typeshed bug?

---

_Comment by @AlexWaygood on 2025-07-04 10:30_

> We now infer this as `bytearray`, but `bytes.translate` [expects `bytes` for its `delete` parameter](https://github.com/python/typeshed/blob/2408c028f4f677e0db4eabef0c70e9f6ab33e365/stdlib/builtins.pyi#L710). This seems to work at runtime, so maybe the typeshed annotation is wrong?
> 
> ```
> >>> b"abc".translate(None, bytearray(b"b"))
> b'ac'
> ```

That sounds like a typeshed bug, yes. I think it's probably a remnant of a special case that used to exist in the type system, but now no longer does, where `bytes` in a type expression would be interpreted in a similar way to `bytes | bytearray | memoryview` by type checkers in some situations (similar to the special case that still exists for `float`/`int`/`complex`). See https://peps.python.org/pep-0688 for details.

---

_Renamed from "[ty] Use inferred type for `Final` symbols" to "[ty] Use RHS inferred type for bare `Final` symbols" by @sharkdp on 2025-07-07 09:16_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-07 09:18_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-07 10:03_

---

_Marked ready for review by @sharkdp on 2025-07-07 10:24_

---

_Review requested from @carljm by @sharkdp on 2025-07-07 10:24_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-07 10:24_

---

_Review requested from @dcreager by @sharkdp on 2025-07-07 10:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/place.rs`:540 on 2025-07-07 10:57_

Does this mean that we would consider `x` to be "qualified with bare `Final`" here?

```py
from ty_extensions import Unknown
from typing import Final

x: Final[Unknown]
```

I think that's okay if so; people just shouldn't do that ;)

---

_@AlexWaygood approved on 2025-07-07 10:58_

nice!

---

_@sharkdp reviewed on 2025-07-07 11:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/place.rs`:540 on 2025-07-07 11:16_

Yes, well spotted :smile:. I agree that it's not as clean as it could be. The alternative is much more complicated as it requires us to return `Option<Type>` and `TypeQualifiers` instead of just `Type` and `TypeQualifiers` from `infer_annotation_expression` and related methods (such that we could return `None` for a bare `Final` or `ClassVar`), which has a lot of downstream consequences.

---

_Merged by @sharkdp on 2025-07-07 11:16_

---

_Closed by @sharkdp on 2025-07-07 11:16_

---

_Branch deleted on 2025-07-07 11:16_

---
