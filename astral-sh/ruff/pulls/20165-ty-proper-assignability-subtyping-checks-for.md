```yaml
number: 20165
title: "[ty] Proper assignability/subtyping checks for protocols with method members"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/method-members
created_at: 2025-08-30T11:47:25Z
updated_at: 2025-09-15T09:18:03Z
url: https://github.com/astral-sh/ruff/pull/20165
synced_at: 2026-01-12T15:56:55Z
```

# [ty] Proper assignability/subtyping checks for protocols with method members

---

_@AlexWaygood_

## Summary

When determining whether a nominal type `N` satisfies a protocol type `P` with a method member `P.x`, check that the signature of the method `N.x` is a subtype of the signature of `P.x` before deciding that `N` is a subtype of `P`.

Note that subtyping between two different types that are both protocols is a separate code path in `instance.rs`, and this PR doesn't touch that code path (I'll do that separately). So this doesn't yet provide a fix for e.g. https://github.com/astral-sh/ty/issues/1089 or https://github.com/astral-sh/ty/issues/733.

Fixes https://github.com/astral-sh/ty/issues/637.
Fixes https://github.com/astral-sh/ty/issues/889.

## Test Plan

- Mdtests updated
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable` to check that the `all_type_assignable_to_iterable_are_iterable` test can be safely marked as stable.

## Typing conformance suite impact

These all look good! All the lines where we're emitting new diagnostics have `# E` comments next to them.

## Ecosystem impact

I analysed nearly all the mypy_primer diff in https://github.com/astral-sh/ruff/pull/20165#issuecomment-3276248772. I don't think there's anything there that indicates a bug in protocol assignability/subtyping logic. Most new hits are due to one of:
1. Us not understanding typeshed's overloads for `builtins.open()` (because that function uses PEP-613 type aliases). This is such a common source of new false positives that it might be worth us adding some temporary special-casing for `open()` where we infer it as returning `Todo`?
2. Users using positional-or-keyword parameters in their protocol definitions, when they should probably be using positional-only parameters instead. I'm surprised this is showing up so much in the ecosystem because I would expect other type checkers to complain about this too. We should probably reimplement flake8-pyi's [Y091 rule](https://github.com/PyCQA/flake8-pyi/blob/main/ERRORCODES.md#Y091) in Ruff and encourage users to enable it if they're type-checking their code with ty.


---

_Label `ty` added by @AlexWaygood on 2025-08-30 11:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/len.md`:239 on 2025-08-30 11:48_

I don't understand what these tests were meant to be showing... these `__len__` signatures all look incorrect? I think these new diagnostics are all correct?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/expression/len.md`:269 on 2025-08-30 11:48_

Just because there's a second optional argument in the `__len__` method doesn't mean the `len(SecondOptionalArgument())` call will fail, so I see no reason why we should be emitting a diagnostic here.

---

_Comment by @github-actions[bot] on 2025-08-30 11:49_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-12 10:07:15.533009560 +0000
+++ new-output.txt	2025-09-12 10:07:18.541030789 +0000
@@ -720,6 +720,12 @@
 protocols_definition.py:159:1: error[invalid-assignment] Object of type `Concrete3_Bad4` is not assignable to `Template3`
 protocols_definition.py:160:1: error[invalid-assignment] Object of type `Concrete3_Bad5` is not assignable to `Template3`
 protocols_definition.py:219:1: error[invalid-assignment] Object of type `Concrete4_Bad2` is not assignable to `Template4`
+protocols_definition.py:285:1: error[invalid-assignment] Object of type `Concrete5_Bad1` is not assignable to `Template5`
+protocols_definition.py:286:1: error[invalid-assignment] Object of type `Concrete5_Bad2` is not assignable to `Template5`
+protocols_definition.py:287:1: error[invalid-assignment] Object of type `Concrete5_Bad3` is not assignable to `Template5`
+protocols_definition.py:288:1: error[invalid-assignment] Object of type `Concrete5_Bad4` is not assignable to `Template5`
+protocols_definition.py:289:1: error[invalid-assignment] Object of type `Concrete5_Bad5` is not assignable to `Template5`
+protocols_generic.py:40:1: error[invalid-assignment] Object of type `Concrete1` is not assignable to `Proto1[int, str]`
 protocols_merging.py:52:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable1`
 protocols_merging.py:53:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable2`
 protocols_merging.py:54:1: error[invalid-assignment] Object of type `SCConcrete2` is not assignable to `SizedAndClosable3`
@@ -848,5 +854,5 @@
 typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 849 diagnostics
+Found 855 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/constraints.rs`:114 on 2025-08-30 11:49_

I wanted to do a `dbg!()` call and it didn't work because a `Constraints` instance wasn't guaranteed to implement `Debug`. This improves debugabbility.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1283 on 2025-08-30 11:50_

Without this, we would never accept any functions as subtypes of protocols with `__call__` methods.

---

_@AlexWaygood reviewed on 2025-08-30 11:50_

---

_Comment by @github-actions[bot] on 2025-08-30 11:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/__init__.py:264:13: error[invalid-assignment] Object of type `_FullStackStatusReporter | nullcontext[None]` is not assignable to `AbstractContextManager[object, bool]`
- Found 175 diagnostics
+ Found 176 diagnostics

nionutils (https://github.com/nion-software/nionutils)
+ nion/utils/StructuredModel.py:91:16: error[invalid-return-type] Return type does not match returned value: expected `ModelLike`, found `RecordModel`
+ nion/utils/StructuredModel.py:93:16: error[invalid-return-type] Return type does not match returned value: expected `ModelLike`, found `ArrayModel`
- Found 5 diagnostics
+ Found 7 diagnostics

yarl (https://github.com/aio-libs/yarl)
- yarl/_quoting_py.py:148:19: warning[redundant-cast] Value is already of type `BufferedIncrementalDecoder`
- Found 49 diagnostics
+ Found 48 diagnostics

antidote (https://github.com/Finistere/antidote)
+ tests/lib/interface/test_conditions.py:193:45: error[invalid-argument-type] Argument to bound method `when` is incorrect: Expected `Predicate[Weight | None] | Weight | None | NeutralWeight | bool`, found `OnlyIf`
- tests/lib/interface/test_custom.py:324:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:327:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:330:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:333:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:338:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:368:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:371:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:374:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:377:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:382:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:392:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:395:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:398:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:401:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/lib/interface/test_custom.py:406:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 330 diagnostics
+ Found 316 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_implementations.py:492:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 559 diagnostics
+ Found 558 diagnostics

rich (https://github.com/Textualize/rich)
+ tests/test_containers.py:17:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Renderables`
- Found 310 diagnostics
+ Found 311 diagnostics

comtypes (https://github.com/enthought/comtypes)
+ comtypes/test/test_variant.py:329:32: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
- Found 399 diagnostics
+ Found 400 diagnostics

isort (https://github.com/pycqa/isort)
+ isort/io.py:47:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `TextIOWrapper[_WrappedBuffer]` does not satisfy upper bound `_WrappedBuffer` of type variable `_BufferT_co`
+ isort/io.py:47:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer`, found `TextIOWrapper[_WrappedBuffer]`
- Found 34 diagnostics
+ Found 36 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/auths.py:323:53: error[invalid-argument-type] Argument is incorrect: Expected `AuthProvider[Unknown]`, found `RequestsAuth[Unknown]`
+ src/schemathesis/auths.py:351:17: error[invalid-assignment] Object of type `CachingAuthProvider[Unknown]` is not assignable to `AuthProvider[Unknown]`
+ src/schemathesis/auths.py:353:17: error[invalid-assignment] Object of type `KeyedCachingAuthProvider[Unknown]` is not assignable to `AuthProvider[Unknown]`
+ src/schemathesis/auths.py:360:13: error[invalid-assignment] Object of type `SelectiveAuthProvider[Unknown]` is not assignable to `AuthProvider[Unknown]`
- Found 267 diagnostics
+ Found 271 diagnostics

vision (https://github.com/pytorch/vision)
- test/datasets_utils.py:1045:9: error[invalid-assignment] Object of type `LiteralString` is not assignable to `tuple[str, ...]`
+ torchvision/datasets/lsun.py:28:37: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ torchvision/datasets/lsun.py:32:36: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
- Found 1459 diagnostics
+ Found 1460 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
+ Pythonwin/pywin/scintilla/config.py:104:40: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:107:53: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:108:46: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:109:45: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:110:46: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:117:55: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:163:55: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:164:64: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:165:59: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:166:54: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:167:55: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:168:42: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ com/win32com/client/gencache.py:86:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ com/win32com/client/gencache.py:130:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_ReadableFileobj`, found `BytesIO | TextIOWrapper[_WrappedBuffer]`
- Found 1986 diagnostics
+ Found 2000 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ tornado/websocket.py:773:16: error[invalid-return-type] Return type does not match returned value: expected `_Compressor`, found `_Compress`
+ tornado/websocket.py:810:16: error[invalid-return-type] Return type does not match returned value: expected `_Decompressor`, found `_Decompress`
- Found 246 diagnostics
+ Found 248 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- test/test_response.py:743:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:755:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:760:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/test_response.py:773:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 390 diagnostics
+ Found 386 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- tests/conftest.py:240:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_wsgi.py:158:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `LimitedStream` does not satisfy upper bound `_BufferedReaderStream` of type variable `_BufferedReaderStreamT`
+ tests/test_wsgi.py:158:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_BufferedReaderStream`, found `LimitedStream`
- Found 369 diagnostics
+ Found 370 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/assertion/rewrite.py:402:31: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ src/_pytest/capture.py:143:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `TextIOWrapper[_WrappedBuffer]` does not satisfy upper bound `_WrappedBuffer` of type variable `_BufferT_co`
+ src/_pytest/capture.py:143:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer`, found `TextIOWrapper[_WrappedBuffer]`
- Found 471 diagnostics
+ Found 474 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/lib/zig.py:130:5: error[invalid-assignment] Object of type `LiteralString` is not assignable to `list[Path] | None`
- Found 2529 diagnostics
+ Found 2528 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/backends/api.py:1516:42: error[invalid-argument-type] Argument to function `_remove_path` is incorrect: Expected `NestedSequence[_FLike@_remove_path]`, found `(_FLike@_remove_path & Top[list[Unknown]]) | (NestedSequence[_FLike@_remove_path] & Top[list[Unknown]])`
- xarray/backends/common.py:192:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- examples/addons/contentview-interactive.py:20:18: error[invalid-argument-type] Argument to function `add` is incorrect: Expected `Contentview`, found `<class 'InteractiveSwapCase'>`
- examples/addons/contentview.py:15:18: error[invalid-argument-type] Argument to function `add` is incorrect: Expected `Contentview`, found `<class 'SwapCase'>`
- test/individual_coverage.py:103:46: error[invalid-argument-type] Argument to function `run_tests` is incorrect: Expected `bool`, found `Match[str] | None`
- test/mitmproxy/contentviews/test__registry.py:67:23: error[invalid-argument-type] Argument to bound method `register` is incorrect: Expected `Contentview`, found `<class 'ExampleContentview'>`
- Found 1822 diagnostics
+ Found 1818 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/test_pandas.py:245:9: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
- Found 4962 diagnostics
+ Found 4963 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/ext/apidoc/_generate.py:50:12: error[no-matching-overload] No overload of bound method `join` matches arguments
- sphinx/search/ja.py:143:16: error[invalid-return-type] Return type does not match returned value: expected `list[str]`, found `list[LiteralString]`

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/backend/backends.py:1396:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/backend/backends.py:1405:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/backend/ninjabackend.py:2995:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[ImmutableListProtocol[str], ImmutableListProtocol[str]]`, found `tuple[list[str], list[str] | list[@Todo(list literal element type)]]`
+ mesonbuild/build.py:1093:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[Unknown]`, found `list[Unknown]`
+ mesonbuild/build.py:1126:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[Unknown]`, found `list[Unknown]`
+ mesonbuild/build.py:1877:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[Unknown]`
+ mesonbuild/build.py:2792:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[Unknown]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/cmake/interpreter.py:549:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/cmake/interpreter.py:556:20: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `(Unknown & list[str] & ~AlwaysFalsy) | list[@Todo(list literal element type)]`
+ mesonbuild/cmake/interpreter.py:558:20: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/compilers/mixins/apple.py:27:5: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `ImmutableListProtocol[str]`
+ mesonbuild/compilers/mixins/clike.py:233:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/compilers/mixins/gnu.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/dependencies/pkgconfig.py:211:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list comprehension element type)]`
+ mesonbuild/utils/universal.py:232:9: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `ImmutableListProtocol[str] | None`
+ mesonbuild/utils/universal.py:235:9: error[invalid-assignment] Object of type `Unknown | list[@Todo(list literal element type)]` is not assignable to `ImmutableListProtocol[str] | None`
+ mesonbuild/utils/universal.py:238:9: error[invalid-assignment] Object of type `Unknown | list[@Todo(list literal element type)]` is not assignable to `ImmutableListProtocol[str] | None`
+ mesonbuild/utils/universal.py:712:12: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list comprehension element type)]`
- Found 794 diagnostics
+ Found 812 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/test/unit/test_frame.py:9855:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[<class 'int'>]`, found `repeat[<class 'str'>]`
- static_frame/test/unit/test_quilt.py:1110:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ benchmarks/bm/iast_fixtures/str_methods.py:462:25: error[invalid-argument-type] Argument to bound method `format_map` is incorrect: Expected `_FormatMapMapping`, found `str`
+ ddtrace/vendor/ply/yacc.py:2011:34: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2014:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2015:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2016:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2017:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2018:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
- Found 6665 diagnostics
+ Found 6672 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/views/registration.py:612:17: error[invalid-argument-type] Argument to bound method `update` is incorrect: Expected `SupportsGetItem[str, str]`, found `QueryDict`
- Found 2671 diagnostics
+ Found 2672 diagnostics

pandas (https://github.com/pandas-dev/pandas)
+ pandas/tests/interchange/test_impl.py:270:29: error[invalid-argument-type] Argument to function `from_dlpack` is incorrect: Expected `_SupportsDLPack[None]`, found `Buffer`
+ pandas/tests/scalar/timedelta/test_arithmetic.py:876:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:882:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:895:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:900:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:908:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:919:13: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:943:13: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:946:13: error[no-matching-overload] No overload of function `divmod` matches arguments
- Found 3002 diagnostics
+ Found 3011 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/helpers/device_registry.py:916:50: error[invalid-argument-type] Argument to function `_normalize_connections` is incorrect: Expected `Iterable[tuple[str, str]]`, found `set[tuple[str, str]] | UndefinedType | (Unknown & ~None)`
+ homeassistant/loader.py:1484:9: error[invalid-argument-type] Argument to function `_resolve_integrations_dependencies` is incorrect: Expected `_ResolveDependenciesCacheProtocol`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
- Found 13446 diagnostics
+ Found 13448 diagnostics

sympy (https://github.com/sympy/sympy)
+ sympy/core/tests/test_numbers.py:147:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:148:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:149:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:150:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:151:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:158:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:159:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:160:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:161:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:162:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:163:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:167:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:168:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:169:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:170:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:171:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:173:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:174:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:175:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:176:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:177:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:178:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:179:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:180:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:181:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:183:16: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:185:16: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:187:16: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:188:16: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:189:16: error[no-matching-overload] No overload of function `divmod` matches arguments
- Found 13387 diagnostics
+ Found 13417 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~22MB
+     memo metadata = ~23MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct fields = ~17MB
+     struct fields = ~18MB

```
</details>


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-30 11:54_

---

_Comment by @AlexWaygood on 2025-08-30 11:55_

Everything in the typing conformance diff looks great, _except_ for this one:

> ```diff
> +specialtypes_none.py:24:1: error[invalid-assignment] Object of type `None` is not assignable to `Hashable`
> ```

`None` should be assignable to `Hashable`.

...and I think that this is https://github.com/astral-sh/ty/issues/737 back to bite us again.

---

_Comment by @codspeed-hq[bot] on 2025-08-30 11:58_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmethod-members?runnerMode=Instrumentation)

### Merging #20165 will **improve performances by 13.92%**

<sub>Comparing <code>alex/method-members</code> (80fc8fe) with <code>main</code> (bb9be26)</sub>



### Summary

`⚡ 1` improvement  
`✅ 42` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` DateType `` | 256.9 ms | 225.5 ms | +13.92% |


---

_Comment by @github-actions[bot] on 2025-08-30 12:06_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 40 | 4 | 0 |
| `no-matching-overload` | 39 | 0 | 0 |
| `unused-ignore-comment` | 0 | 23 | 0 |
| `invalid-return-type` | 18 | 1 | 0 |
| `invalid-assignment` | 8 | 2 | 0 |
| `redundant-cast` | 0 | 1 | 0 |
| `type-assertion-failure` | 1 | 0 | 0 |
| **Total** | **106** | **31** | **0** |

**[Full report with detailed diff](https://alex-method-members.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-30 14:29_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-30 14:29_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-30 14:48_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-30 14:48_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-30 14:57_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-30 14:57_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-08-30 17:57_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-08-30 17:57_

---

_Comment by @codspeed-hq[bot] on 2025-08-30 18:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmethod-members?runnerMode=WallTime)

### Merging #20165 will **not alter performance**

<sub>Comparing <code>alex/method-members</code> (80fc8fe) with <code>main</code> (bb9be26)</sub>



### Summary

`✅ 8` untouched  





---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-10 17:12_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-10 17:12_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-10 17:15_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-10 17:16_

---

_Comment by @AlexWaygood on 2025-09-10 19:27_

# Mypy_primer analysis

## `comtypes`

<details>

```diff
comtypes (https://github.com/enthought/comtypes)
+ comtypes/test/test_variant.py:329:32: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
```

This is because we don't yet understand PEP-613 type aliases. We should be inferring the result of the `open()` call [here](https://github.com/enthought/comtypes/blob/2dc690a133cc1506bc3ec0baeaf85f8a7fdf3fda/comtypes/test/test_variant.py#L329) as being `_io.BufferedReader`, but instead we're inferring `io.TextIOWrapper` because typeshed uses type aliases in [its (nightmarish) overloads for `builtins.open`](https://github.com/python/typeshed/blob/d9c76e1d9f0c0000c1971c3e936a69dd55f49ce1/stdlib/builtins.pyi#L1711-L1796). We're correct in saying that [`TextIOWrapper`](https://github.com/python/typeshed/blob/d9c76e1d9f0c0000c1971c3e936a69dd55f49ce1/stdlib/_io.pyi#L240) is not assignable to [`_ReadableFileobj`](https://github.com/python/typeshed/blob/d9c76e1d9f0c0000c1971c3e936a69dd55f49ce1/stdlib/_pickle.pyi#L8-L10) -- `_ReadableFileObj` states that classes must have a `read()` method returning `bytes` in order to satisfy its interface. `TextIOWrapper.read()` returns `str`.

</details>

## `ddtrace`

<details>

```diff
+ benchmarks/bm/iast_fixtures/str_methods.py:462:25: error[invalid-argument-type] Argument to bound method `format_map` is incorrect: Expected `_FormatMapMapping`, found `str`
```

This looks correct; [this function](https://github.com/DataDog/dd-trace-py/blob/2b8bd63baaf1a6e954ea2994c4ba044fa6aeaa93/benchmarks/bm/iast_fixtures/str_methods.py#L460C1-L462C31) causes [other type checkers to issue complaints too](https://mypy-play.net/?mypy=latest&python=3.12&gist=f53d9312746bdb2d88d854825deea02f). 

```diff
+ ddtrace/vendor/ply/yacc.py:2011:34: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2014:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2015:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2016:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2017:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ ddtrace/vendor/ply/yacc.py:2018:38: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
```

All the hits in this file have the same root cause as the one on `comtypes`

</details>

## `isort`, `pytest`, `pywin32`, `vision`

<details>

```diff
isort (https://github.com/pycqa/isort)
+ isort/io.py:47:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `TextIOWrapper[_WrappedBuffer]` does not satisfy upper bound `_WrappedBuffer` of type variable `_BufferT_co`
+ isort/io.py:47:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer`, found `TextIOWrapper[_WrappedBuffer]`

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/assertion/rewrite.py:402:31: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ src/_pytest/capture.py:143:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `TextIOWrapper[_WrappedBuffer]` does not satisfy upper bound `_WrappedBuffer` of type variable `_BufferT_co`
+ src/_pytest/capture.py:143:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_WrappedBuffer`, found `TextIOWrapper[_WrappedBuffer]`

pywin32 (https://github.com/mhammond/pywin32)
+ Pythonwin/pywin/scintilla/config.py:104:40: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:107:53: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:108:46: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:109:45: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:110:46: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:117:55: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `SupportsRead[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:163:55: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:164:64: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:165:59: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:166:54: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:167:55: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ Pythonwin/pywin/scintilla/config.py:168:42: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ com/win32com/client/gencache.py:86:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
+ com/win32com/client/gencache.py:130:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_ReadableFileobj`, found `BytesIO | TextIOWrapper[_WrappedBuffer]`

vision (https://github.com/pytorch/vision)
+ torchvision/datasets/lsun.py:28:37: error[invalid-argument-type] Argument to function `load` is incorrect: Expected `_ReadableFileobj`, found `TextIOWrapper[_WrappedBuffer]`
+ torchvision/datasets/lsun.py:32:36: error[invalid-argument-type] Argument to function `dump` is incorrect: Expected `SupportsWrite[bytes]`, found `TextIOWrapper[_WrappedBuffer]`
```

These are also caused by the same issue as the `comtypes` hit: we don't understand typeshed's overloads for `open()` because we don't yet understand PEP-613 type aliases.

</details>

## `antidote`

<details>

```diff
antidote (https://github.com/Finistere/antidote)
+ tests/lib/interface/test_conditions.py:193:45: error[invalid-argument-type] Argument to bound method `when` is incorrect: Expected `Predicate[Weight | None] | Weight | None | NeutralWeight | bool`, found `OnlyIf`
```

This looks correct to me. The call to `when()` is [here](https://github.com/Finistere/antidote/blob/7d64ff76b7e283e5d9593ca09ea7a52b9b054957/tests/lib/interface/test_conditions.py#L193) and the `when()` method is defined [here](https://github.com/Finistere/antidote/blob/7d64ff76b7e283e5d9593ca09ea7a52b9b054957/src/antidote/lib/interface_ext/__init__.py#L1035-L1044). The input parameter to `when()` is annotated as accepting a union of nominal types or `Weight`, which [is a `TypeVar` bound to a protocol](https://github.com/Finistere/antidote/blob/7d64ff76b7e283e5d9593ca09ea7a52b9b054957/src/antidote/lib/interface_ext/__init__.py#L59). The [protocol definition](https://github.com/Finistere/antidote/blob/7d64ff76b7e283e5d9593ca09ea7a52b9b054957/src/antidote/lib/interface_ext/predicate.py#L28) doesn't appear to be satisfied by [the `OnlyIf` class](https://github.com/Finistere/antidote/blob/7d64ff76b7e283e5d9593ca09ea7a52b9b054957/tests/lib/interface/common.py#L53) so it doesn't satisfy the upper bound of the TypeVar. I don't think it satisfies any of the other nominal types in the union either. If anything, I'm not sure why we accepted this call previously?

</details>

## `core`

<details>

```diff
core (https://github.com/home-assistant/core)
+ homeassistant/helpers/device_registry.py:916:50: error[invalid-argument-type] Argument to function `_normalize_connections` is incorrect: Expected `Iterable[tuple[str, str]]`, found `set[tuple[str, str]] | UndefinedType | (Unknown & ~None)`
```

This is expected with our current behaviour. They're doing type narrowing like this, but it doesn't work because of the fact that we view `SINGLETON` here as having type `Foo | Unknown` from inside the function scope, rather than `Foo`:

```py
from enum import Enum

class Foo(Enum):
    SINGLETON = object()

SINGLETON = Foo.SINGLETON

def _(x: Foo | int):
    reveal_type(SINGLETON)
    if x is not SINGLETON:
        reveal_type(x)
```

If you add an explicit `Final` annotation to `SINGLETON`, we're happy to narrow the type here in the way that they'd expect: https://play.ty.dev/a603b1ab-4d53-4f6f-ada4-09b0c398943f. Possibly we could make our behaviour more intuitive to users here in the future by treating SCREAMING_CASE global constants as being implicitly `Final` (pyright actually does this). https://github.com/astral-sh/ty/issues/1069 might also help here?

```diff
+ homeassistant/loader.py:1484:9: error[invalid-argument-type] Argument to function `_resolve_integrations_dependencies` is incorrect: Expected `_ResolveDependenciesCacheProtocol`, found `dict[@Todo(dict literal key type), @Todo(dict literal value type)]`
```

This one is correct because the protocol states that a class must have positional-or-keyword parameters in its `get` and `__setitem__` methods in order for it to be a subtype. All parameters in `dict.get` and `dict.__setitem__` are positional-only.

</details>

## `meson`

<details>

```diff
meson (https://github.com/mesonbuild/meson)
+ mesonbuild/backend/backends.py:1396:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/backend/backends.py:1405:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/backend/ninjabackend.py:2995:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[ImmutableListProtocol[str], ImmutableListProtocol[str]]`, found `tuple[list[str], list[str] | list[@Todo(list literal element type)]]`
+ mesonbuild/build.py:1093:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[Unknown]`, found `list[Unknown]`
+ mesonbuild/build.py:1126:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[Unknown]`, found `list[Unknown]`
+ mesonbuild/build.py:1877:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[Unknown]`
+ mesonbuild/build.py:2792:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[Unknown]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/cmake/interpreter.py:549:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/cmake/interpreter.py:556:20: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `(Unknown & list[str] & ~AlwaysFalsy) | list[@Todo(list literal element type)]`
+ mesonbuild/cmake/interpreter.py:558:20: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/compilers/mixins/apple.py:27:5: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `ImmutableListProtocol[str]`
+ mesonbuild/compilers/mixins/clike.py:233:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/compilers/mixins/gnu.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list literal element type)]`
+ mesonbuild/dependencies/pkgconfig.py:211:16: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list comprehension element type)]`
+ mesonbuild/utils/universal.py:232:9: error[invalid-assignment] Object of type `list[@Todo(list literal element type)]` is not assignable to `ImmutableListProtocol[str] | None`
+ mesonbuild/utils/universal.py:235:9: error[invalid-assignment] Object of type `Unknown | list[@Todo(list literal element type)]` is not assignable to `ImmutableListProtocol[str] | None`
+ mesonbuild/utils/universal.py:238:9: error[invalid-assignment] Object of type `Unknown | list[@Todo(list literal element type)]` is not assignable to `ImmutableListProtocol[str] | None`
+ mesonbuild/utils/universal.py:712:12: error[invalid-return-type] Return type does not match returned value: expected `ImmutableListProtocol[str]`, found `list[@Todo(list comprehension element type)]`
```

These all look like they're because [the `ImmutableListProtocol` protocol](https://github.com/mesonbuild/meson/blob/3734ff4bb11e82e59cf66a82288b80db3def83ec/mesonbuild/_typing.py#L30) has many method members that have positional-or-keyword parameters, whereas `list` only has positional-only parameters for the corresponding methods; therefore, `list` is not assignable to the protocol. I think our behaviour is correct here.

</details>

## `nionutils`

<details>

```diff
nionutils (https://github.com/nion-software/nionutils)
+ nion/utils/StructuredModel.py:91:16: error[invalid-return-type] Return type does not match returned value: expected `ModelLike`, found `RecordModel`
+ nion/utils/StructuredModel.py:93:16: error[invalid-return-type] Return type does not match returned value: expected `ModelLike`, found `ArrayModel`
```

It took me a while to figure this one out. I think this is correct. The `ModelLike` protocol [here](https://github.com/nion-software/nionutils/blob/762ef25d29759c47610d0aa8e70600cc5fc5d785/nion/utils/StructuredModel.py#L66) states that a class must have a `from_dict_value` method with a `value` positional-or-only parameter in order for that class to be a subtype of `ModelLike`. Both `RecordModel` and `ArrayModel` have `from_dict_value` methods... but they both have `from_dict_value` methods with a parameter named `values` rather than `value` (notice the `s` at the end!).

</details>

## `pandas`

<details>

```diff
pandas (https://github.com/pandas-dev/pandas)
+ pandas/tests/interchange/test_impl.py:270:29: error[invalid-argument-type] Argument to function `from_dlpack` is incorrect: Expected `_SupportsDLPack[None]`, found `Buffer`
```

This looks correct to me. The `_SupportsDLPack` protocol is defined in `numpy` [here](https://github.com/numpy/numpy/blob/85f973e241656142af868d020a080906b32ec290/numpy/__init__.pyi#L1065). It states that in order for a class to satisfy the protocol, it must have a `__dlpack__` method which will not fail if you pass in a `stream` argument. But [`Buffer.__dlpack__`](https://github.com/pandas-dev/pandas/blob/2f2664433f6ae18fc2013a0c1e86fe562cd74c3b/pandas/core/interchange/dataframe_protocol.py#L151) has no `stream` parameter.

```diff
+ pandas/tests/scalar/timedelta/test_arithmetic.py:876:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:882:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:895:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:900:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:908:18: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:919:13: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:943:13: error[no-matching-overload] No overload of function `divmod` matches arguments
+ pandas/tests/scalar/timedelta/test_arithmetic.py:946:13: error[no-matching-overload] No overload of function `divmod` matches arguments
```

I think our behaviour is correct here; this looks like a bug in pandas's type annotations. Their stub file [here](https://github.com/pandas-dev/pandas/blob/2f2664433f6ae18fc2013a0c1e86fe562cd74c3b/pandas/_libs/tslibs/timedeltas.pyi#L153) clearly states that `Timedelta.__divmod__` only accepts instances of `timedelta`, but that's obviously not what their tests are asserting here. It's also clearly not true at runtime:

```pycon
% uvx --with=pandas python                  
Installed 6 packages in 54ms
Python 3.13.7 (main, Sep  2 2025, 14:05:52) [Clang 20.1.4 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from pandas import Timedelta
>>> td = Timedelta(days=2, hours=6)
>>> td
Timedelta('2 days 06:00:00')
>>> result = divmod(td, 53 * 3600 * 1e9)
>>> result
(Timedelta('0 days 00:00:00.000000001'), Timedelta('0 days 01:00:00'))
```

And in the actual (Cython!) implementation of the `__floordiv__` method (which the `__divmod__` method just defers to) here we can clearly see that there's a code path specifically for if a `float` is passed as the second argument: https://github.com/pandas-dev/pandas/blob/1fa95a15535b72c95c0e79e1bdc6164476c4af30/pandas/_libs/tslibs/timedeltas.pyx#L2396.

</details>

## `pytest-robotframework`

<details>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/__init__.py:264:13: error[invalid-assignment] Object of type `_FullStackStatusReporter | nullcontext[None]` is not assignable to `AbstractContextManager[object, bool]`
```

This is correct: `nullcontext[None]` is not assignable to `AbstractContextManager[object, bool]` since `nullcontext.__exit__` returns `None`, not `bool`. They already have this line `pyright: ignore`d and a comment suggesting they know type checkers will complain here: https://github.com/DetachHead/pytest-robotframework/blob/cc6c6532abb66e885f81e3a027f80eafc65df5d3/pytest_robotframework/__init__.py#L262-L264.

</details>

## `rich`

<details>

```diff
rich (https://github.com/Textualize/rich)
+ tests/test_containers.py:17:17: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Renderables`
```

This looks correct to me. `Renderables` has an `__iter__` method, but it [is annotated as returning `Iterable`](https://github.com/Textualize/rich/blob/ea9d4db5d84b4e834979304e3053bf757daae322/rich/containers.py#L62-L63). In order for a class `X` to be considered a subtype of `Iterable`, however, `X.__iter__` must return `Iterator`, not `Iterable`.

</details>

## `schemathesis`

<details>

```diff
schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/auths.py:323:53: error[invalid-argument-type] Argument is incorrect: Expected `AuthProvider[Unknown]`, found `RequestsAuth[Unknown]`
+ src/schemathesis/auths.py:351:17: error[invalid-assignment] Object of type `CachingAuthProvider[Unknown]` is not assignable to `AuthProvider[Unknown]`
+ src/schemathesis/auths.py:353:17: error[invalid-assignment] Object of type `KeyedCachingAuthProvider[Unknown]` is not assignable to `AuthProvider[Unknown]`
+ src/schemathesis/auths.py:360:13: error[invalid-assignment] Object of type `SelectiveAuthProvider[Unknown]` is not assignable to `AuthProvider[Unknown]`
```

Looks like another case where they really should have used positional-only arguments in their protocol's method members.

</details>

## `sphinx`

<details>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
+ sphinx/ext/apidoc/_generate.py:50:12: error[no-matching-overload] No overload of bound method `join` matches arguments
```

This is because we have this behaviour:

```py
def module_join(*modnames: str | None) -> str:
    reveal_type(modnames)  # tuple[str | None, ...]
    reveal_type(filter(None, modnames))  # filter[str | None]`, but `filter[str]` would be better!
```

We're doing a suboptimal job at solving the TypeVar in [this overload](https://github.com/python/typeshed/blob/6937a9b193bfc2f0696452d58aad96d7627aa29a/stdlib/builtins.pyi#L1506-L1507) of `filter.__new__`; this should be fixed by the new constraint solver.

</details>

## `static-frame`

<details>

```diff
static-frame (https://github.com/static-frame/static-frame)
+ static_frame/test/unit/test_frame.py:9855:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[<class 'int'>]`, found `repeat[<class 'str'>]`
```

Looks like something that could be solved with an improved constraints solver. The TypeVar should be solved as `<class 'int'> | <class 'str'>` here; we shouldn't reject this `chain.__new__` call.

</details>

## `sympy`

<details>

```diff
sympy (https://github.com/sympy/sympy)
+ sympy/core/tests/test_numbers.py:147:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:148:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:149:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:150:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:151:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:158:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:159:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:160:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:161:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:162:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:163:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:167:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:168:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:169:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:170:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:171:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:173:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:174:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:175:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:176:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:177:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:178:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:179:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:180:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:181:12: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:183:16: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:185:16: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:187:16: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:188:16: error[no-matching-overload] No overload of function `divmod` matches arguments
+ sympy/core/tests/test_numbers.py:189:16: error[no-matching-overload] No overload of function `divmod` matches arguments
```

I'm not totally sure what's going on here TBH (and struggling to repro these errors locally). But what I can say is that neither mypy nor pyright like this function at all either (they both also emit an error on almost every line -- though it is a slightly different error to what we report).

</details>

## `tornado`

<details>

```diff
tornado (https://github.com/tornadoweb/tornado)
+ tornado/websocket.py:773:16: error[invalid-return-type] Return type does not match returned value: expected `_Compressor`, found `_Compress`
+ tornado/websocket.py:810:16: error[invalid-return-type] Return type does not match returned value: expected `_Decompressor`, found `_Decompress`
```

Another example where their protocol definition should be using positional-only parameters, but is instead using positional-or-keyword parameters.

</details>

## `werkzeug`

<details>

```diff
werkzeug (https://github.com/pallets/werkzeug)
+ tests/test_wsgi.py:158:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `LimitedStream` does not satisfy upper bound `_BufferedReaderStream` of type variable `_BufferedReaderStreamT`
+ tests/test_wsgi.py:158:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_BufferedReaderStream`, found `LimitedStream`
```

It looks like the issue here is that typeshed's `_io._BufferedReaderStream` protocol has [a `readinto` method member that takes in a `memoryview`](https://github.com/python/typeshed/blob/6937a9b193bfc2f0696452d58aad96d7627aa29a/stdlib/_io.pyi#L130). But werkzeug's `LimitedStream` class has [a `readinto` method](https://github.com/pallets/werkzeug/blob/504a8c4fbda9b8b2fd09e817544ffd228f23458e/src/werkzeug/wsgi.py#L520) that takes in an instance of `bytearray`. There's no subtyping relation between `bytearray` and `memoryview`, so `LimitedStream` cannot be considered a subtype of `_BuffereadReaderStream`. `LimitedStream.readinto()` is even `type: ignore`d so that a type checker won't complain about the Liskov violation!

</details>

---

_@AlexWaygood reviewed on 2025-09-11 18:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2232 on 2025-09-11 18:26_

I believe this is because we don't consider `Foo` a fully static type currently, because it has an unannotated `self` parameter

---

_@AlexWaygood reviewed on 2025-09-11 18:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:564 on 2025-09-11 18:27_

I wanted to get a MVP of this feature out before digging into why we have lots of false positives for method members with function-scoped generic contexts

---

_Marked ready for review by @AlexWaygood on 2025-09-11 18:38_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-11 18:38_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-11 18:38_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-11 18:38_

---

_Comment by @AlexWaygood on 2025-09-11 18:41_

> Merging #20165 will **improve performances by 13.56%**

Quite unexpected, but I'll take it!!

The `DateType` benchmark is so strange, because `DateType` has such huge protocols. My guess is that we're able to short-circuit subtyping checks sooner now for some of its very big protocols, because we more quickly realise that a given type cannot be a subtype of a given protocol due to a signature mismatch. The more quickly we short-circuit during a subtype check, the fewer protocol members we need to iterate through and test the existence of.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/expression/len.md`:239 on 2025-09-11 23:05_

I would just remove this whole section, I don't think it's testing anything useful.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3275 on 2025-09-11 23:08_

Do we have any direct tests of this (that the `__call__` of a callable type is itself), or only indirect tests via protocols? Would it be worth a direct test for it, to make it clearer what's happening if it ever regresses?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/protocol_class.rs`:561 on 2025-09-11 23:17_

I guess using `any_over_type` rather than checking for a function with generic context maybe catches some other troublesome cases, like `Callable` annotations including typevars?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/protocol_class.rs`:568 on 2025-09-11 23:22_

The docs I wrote on CycleDetector suggest that we should always try to `visitor.visit` only in the top-level `Type::has_relation_to_impl` method (e.g. before visiting a Protocol type) rather than inside methods of particular kinds of types. The reason is that otherwise it can be easy to accidentally `visitor.visit` the same key twice in succession, causing incorrect detection of a non-existent "cycle". For instance, if the pair of (`attribute_type`, `proto_member_as_bound_method`) here happens to be a pair of types which we `visitor.visit` in `Type::has_relation_to_impl`, that would happen.

Does something go wrong here if we restrict `visitor.visit` to only `Type::has_relation_to_impl`?

---

_@carljm approved on 2025-09-11 23:23_

Nice job extracting so many fixes along the way and leaving this such a small diff!

---

_@AlexWaygood reviewed on 2025-09-12 09:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:568 on 2025-09-12 09:02_

Ah, thank you! Fixed in https://github.com/astral-sh/ruff/pull/20165/commits/b3b0b5fa8f2b0a5e3ae8befb20ecf1d660f63006

---

_@AlexWaygood reviewed on 2025-09-12 09:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3275 on 2025-09-12 09:43_

What's changing here is a bit subtle. On `main` [we are already able to infer](https://play.ty.dev/139ebe43-c29d-447e-9c2d-3ca17ad88286) that for an object `c` of a callable type `T`, the type of `c.__call__` will also be `T`. But naively looking up the `__call__` attribute directly on `c` isn't good enough for protocol assignability/subtyping -- it would mean that this assertion would not pass, for example, because `Foo.__iter__` has type `Callable[[Foo], str]` rather than `Callable[[], Foo]`. (The enum class object itself is iterable, but so is each enum member. Iterating over the enum members uses `str.__iter__` because each enum member is an instance of `str`; iterating over the enum class object itself uses `EnumType.__iter__`.):

```py
from typing import Iterable
from enum import StrEnum
from ty_extensions import static_assert, is_assignable_to, TypeOf

class Foo(StrEnum):
    X = "foo"
    Y = "bar"

static_assert(is_assignable_to(TypeOf[Foo], Iterable[Foo]))
```

```pycon
>>> from enum import StrEnum
>>> class Foo(StrEnum):
...     X = "foo"
...     Y = "bar"
...     
>>> list(Foo)
[<Foo.X: 'foo'>, <Foo.Y: 'bar'>]
>>> list(Foo.X)
['f', 'o', 'o']
```

Therefore to get the "instance get-type" of a method member, we can't simply lookup the type of that member on an object; instead we must lookup the type of that member on the object's meta-type and then invoke the descriptor protocol on the result of that lookup. On most types that works great, but not on `Callable` types, because we currently give the meta-type of a `Callable` type as being just `type` (it's a very lossy operation currently!). Ideally we would synthesize some kind of meta-protocol as the meta-type of a `Callable` type (I'd like to do that at some point!) so that it's not so lossy, but that feels out of scope for this PR.

TL;DR: no, I don't think it's really possible to add an isolated unit test for this that doesn't involve protocols. It's a change that's very specific to the way we need to look up the `__call__` attribute on `Callable` types when determining whether a `Callable` type is a subtype of a protocol type with a `__call__` method.

_However_, I will add a comment to this bit of code noting that while this isn't inaccurate, it's basically a workaround until we have a better answer for the meta-type of `Callable` types.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:561 on 2025-09-12 10:03_

Yes... also I'm not sure we have a great API right now for checking whether a method has a function-scoped generic context? Maybe I could add one, but I'm not sure it's worth it just for this (it looks kinda complicated -- we'd need to iterate over all overloads of the function?), since this is hopefully just a temporary workaround.

---

_@AlexWaygood reviewed on 2025-09-12 10:03_

---

_Merged by @AlexWaygood on 2025-09-12 10:10_

---

_Closed by @AlexWaygood on 2025-09-12 10:10_

---

_Branch deleted on 2025-09-12 10:10_

---

_@AlexWaygood reviewed on 2025-09-12 17:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3275 on 2025-09-12 17:20_

I'm actually getting rid of this again in https://github.com/astral-sh/ruff/pull/20363 FWIW, and replacing it with special-casing somewhere else instead 😄

---

_@sharkdp reviewed on 2025-09-15 08:09_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/protocol_class.rs`:552 on 2025-09-15 08:09_

Why do you call `invoke_descriptor_protocol` directly here instead of going through `Type::member`? Does this work correctly if `other` is a union/intersection (is that already handled at a higher level)? And does it handle classmethods, method accesses on class objects etc. correctly?

---

_@AlexWaygood reviewed on 2025-09-15 09:05_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:552 on 2025-09-15 09:05_

> Why do you call `invoke_descriptor_protocol` directly here instead of going through `Type::member`?

I answered this in my reply to Carl [here](https://github.com/astral-sh/ruff/pull/20165#discussion_r2343655171)

> Does this work correctly if `other` is a union/intersection (is that already handled at a higher level)?

I think so... I should probably add more tests around this. Is there a reason why you think this might not work for unions/intersections?

> And does it handle classmethods, method accesses on class objects etc. correctly?

I should definitely add more tests around this too, thanks. Note that there are some more checks that I'd like to do (such as checking the type on the meta-type as well as the type on the instance-type) which we can't yet do while we consider methods with unannotated `self`/`cls` to be non-fully-static. This PR is really an MVP

---

_@sharkdp reviewed on 2025-09-15 09:18_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/protocol_class.rs`:552 on 2025-09-15 09:18_

> Is there a reason why you think this might not work for unions/intersections?

Not sure, but I think `member` does the "distribute this operation over unions/intersections" part of `invoke_descriptor_protocol`.

If you plan to add more tests for both of these cases, we should be fine. Thanks.

---
