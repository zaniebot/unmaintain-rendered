```yaml
number: 18443
title: "[ty] Add generic inference for dataclasses"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: dataclass-generic-inference
created_at: 2025-06-03T16:04:31Z
updated_at: 2025-06-28T23:12:43Z
url: https://github.com/astral-sh/ruff/pull/18443
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Add generic inference for dataclasses

---

_@MatthewMckee4_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

An issue seen here https://github.com/astral-sh/ty/issues/500

The `__init__` method of dataclasses had no inherited generic context, so we could not infer the type of an instance from a constructor call with generics

## Test Plan

Add tests to classes.md` in generics folder

---

_Review requested from @carljm by @MatthewMckee4 on 2025-06-03 16:04_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-06-03 16:04_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-06-03 16:04_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-06-03 16:04_

---

_Label `ty` added by @ntBre on 2025-06-03 16:05_

---

_Renamed from "Add generic inference for dataclasses" to "[ty] Add generic inference for dataclasses" by @MatthewMckee4 on 2025-06-03 16:05_

---

_Comment by @github-actions[bot] on 2025-06-03 16:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- error[invalid-argument-type] src/anyio/_core/_streams.py:52:40: Argument is incorrect: Expected `MemoryObjectStreamState[T_contra]`, found `MemoryObjectStreamState[T_Item]`
- error[invalid-argument-type] src/anyio/_core/_streams.py:52:74: Argument is incorrect: Expected `MemoryObjectStreamState[T_co]`, found `MemoryObjectStreamState[T_Item]`
- Found 113 diagnostics
+ Found 111 diagnostics

Expression (https://github.com/cognitedata/Expression)
- error[invalid-argument-type] expression/collections/maptree.py:403:42: Argument is incorrect: Expected `Value`, found `Result`
- error[invalid-argument-type] expression/collections/maptree.py:405:42: Argument is incorrect: Expected `Value`, found `Result`
- error[invalid-argument-type] expression/core/option.py:58:23: Argument is incorrect: Expected `_TSourceOut`, found `_TResult`
- error[invalid-argument-type] expression/core/option.py:244:31: Argument is incorrect: Expected `_TErrorOut`, found `_TError`
- error[invalid-argument-type] expression/core/option.py:254:31: Argument is incorrect: Expected `_TErrorOut`, found `_TError`
- error[invalid-argument-type] expression/core/result.py:66:33: Argument is incorrect: Expected `_TSourceOut`, found `_TResult`
- error[invalid-argument-type] expression/core/result.py:71:36: Argument is incorrect: Expected `_TErrorOut`, found `_TError`
- error[invalid-argument-type] tests/test_tagged_union.py:120:16: Argument is incorrect: Expected `_T`, found `Literal[1]`
- error[invalid-argument-type] tests/test_tagged_union.py:129:16: Argument is incorrect: Expected `_T`, found `Literal[1]`
- error[invalid-argument-type] tests/test_tagged_union.py:149:16: Argument is incorrect: Expected `_T`, found `Literal[1]`
- error[invalid-argument-type] tests/test_tagged_union.py:160:16: Argument is incorrect: Expected `_T`, found `Shape`
- error[invalid-argument-type] tests/test_tagged_union.py:169:16: Argument is incorrect: Expected `_T`, found `Literal[1]`
- error[invalid-argument-type] tests/test_tagged_union.py:170:16: Argument is incorrect: Expected `_T`, found `Literal[2]`
- error[invalid-argument-type] tests/test_tagged_union.py:176:16: Argument is incorrect: Expected `_T`, found `Literal[1]`
- error[invalid-argument-type] tests/test_tagged_union.py:184:16: Argument is incorrect: Expected `_T`, found `Literal[1]`
- error[invalid-argument-type] tests/test_tagged_union.py:197:16: Argument is incorrect: Expected `_T`, found `Literal[1]`
- Found 268 diagnostics
+ Found 252 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- error[invalid-argument-type] koda_validate/dictionary.py:60:26: Argument is incorrect: Expected `A`, found `Just[@Todo(generic `typing.Awaitable` type)]`
- error[invalid-argument-type] koda_validate/dictionary.py:67:26: Argument is incorrect: Expected `A`, found `Just[Unknown]`
- error[invalid-argument-type] koda_validate/dictionary.py:159:26: Argument is incorrect: Expected `A`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] koda_validate/dictionary.py:209:26: Argument is incorrect: Expected `A`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] koda_validate/valid.py:29:22: Argument is incorrect: Expected `A`, found `B`
- Found 50 diagnostics
+ Found 45 diagnostics

trio (https://github.com/python-trio/trio)
- error[invalid-argument-type] src/trio/_tests/test_highlevel_generic.py:41:29: Argument is incorrect: Expected `SendStreamT`, found `RecordSendStream`
- error[invalid-argument-type] src/trio/_tests/test_highlevel_generic.py:41:42: Argument is incorrect: Expected `ReceiveStreamT`, found `RecordReceiveStream`
- error[invalid-argument-type] src/trio/_tests/test_highlevel_generic.py:91:29: Argument is incorrect: Expected `SendStreamT`, found `BrokenSendStream`
- error[invalid-argument-type] src/trio/_tests/test_highlevel_generic.py:91:49: Argument is incorrect: Expected `ReceiveStreamT`, found `BrokenReceiveStream`
- error[invalid-argument-type] src/trio/testing/_memory_streams.py:377:29: Argument is incorrect: Expected `SendStreamT`, found `SendStreamT`
- error[invalid-argument-type] src/trio/testing/_memory_streams.py:377:41: Argument is incorrect: Expected `ReceiveStreamT`, found `ReceiveStreamT`
- error[invalid-argument-type] src/trio/testing/_memory_streams.py:378:29: Argument is incorrect: Expected `SendStreamT`, found `SendStreamT`
- error[invalid-argument-type] src/trio/testing/_memory_streams.py:378:41: Argument is incorrect: Expected `ReceiveStreamT`, found `ReceiveStreamT`
- Found 1102 diagnostics
+ Found 1094 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[invalid-argument-type] mkosi/config.py:2541:9: Argument is incorrect: Expected `T | None`, found `Literal[True]`
- error[invalid-argument-type] mkosi/config.py:2656:9: Argument is incorrect: Expected `T | None`, found `Literal[True]`
- error[invalid-argument-type] mkosi/config.py:2743:9: Argument is incorrect: Expected `T | None`, found `Literal[3]`
- error[invalid-argument-type] mkosi/config.py:2825:9: Argument is incorrect: Expected `T | None`, found `UUID`
- error[invalid-argument-type] mkosi/config.py:2899:9: Argument is incorrect: Expected `T | None`, found `Literal[True]`
- error[invalid-argument-type] mkosi/config.py:3105:9: Argument is incorrect: Expected `T | None`, found `Literal[False]`
- error[invalid-argument-type] mkosi/config.py:3115:9: Argument is incorrect: Expected `T | None`, found `list[Unknown]`
- error[invalid-argument-type] mkosi/config.py:3194:9: Argument is incorrect: Expected `T | None`, found `Literal[True]`
- error[invalid-argument-type] mkosi/config.py:3368:9: Argument is incorrect: Expected `T | None`, found `Literal[True]`
- error[invalid-argument-type] mkosi/config.py:3385:9: Argument is incorrect: Expected `T | None`, found `KeySource`
- error[invalid-argument-type] mkosi/config.py:3403:9: Argument is incorrect: Expected `T | None`, found `CertificateSource`
- error[invalid-argument-type] mkosi/config.py:3438:9: Argument is incorrect: Expected `T | None`, found `KeySource`
- error[invalid-argument-type] mkosi/config.py:3456:9: Argument is incorrect: Expected `T | None`, found `CertificateSource`
- error[invalid-argument-type] mkosi/config.py:3482:9: Argument is incorrect: Expected `T | None`, found `KeySource`
- error[invalid-argument-type] mkosi/config.py:3500:9: Argument is incorrect: Expected `T | None`, found `CertificateSource`
- error[invalid-argument-type] mkosi/config.py:3537:9: Argument is incorrect: Expected `T | None`, found `Literal["gpg"]`
- error[invalid-argument-type] mkosi/config.py:3639:9: Argument is incorrect: Expected `T | None`, found `Literal[True]`
- error[invalid-argument-type] mkosi/config.py:3715:9: Argument is incorrect: Expected `T | None`, found `Literal["&d~&r~&a~&I"]`
- error[invalid-argument-type] mkosi/config.py:3748:9: Argument is incorrect: Expected `T | None`, found `Literal["&d~&r~&a"]`
- error[invalid-argument-type] mkosi/config.py:3764:9: Argument is incorrect: Expected `T | None`, found `Literal[True]`
- error[invalid-argument-type] mkosi/config.py:3829:9: Argument is incorrect: Expected `T | None`, found `Literal[True]`
- error[invalid-argument-type] mkosi/config.py:4052:9: Argument is incorrect: Expected `T | None`, found `Literal[1]`
- error[invalid-argument-type] mkosi/config.py:4064:9: Argument is incorrect: Expected `T | None`, found `int`
- Found 248 diagnostics
+ Found 225 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- error[invalid-argument-type] src/schemathesis/specs/openapi/schemas.py:155:48: Argument is incorrect: Expected `D`, found `None`
+ error[invalid-argument-type] src/schemathesis/specs/openapi/schemas.py:155:48: Argument is incorrect: Argument type `None` does not satisfy upper bound of type variable `D`
- error[invalid-argument-type] src/schemathesis/specs/openapi/schemas.py:155:58: Argument is incorrect: Expected `D`, found `None`
+ error[invalid-argument-type] src/schemathesis/specs/openapi/schemas.py:155:58: Argument is incorrect: Argument type `None` does not satisfy upper bound of type variable `D`
- error[invalid-argument-type] src/schemathesis/specs/openapi/schemas.py:429:44: Argument is incorrect: Expected `D`, found `dict[str, Any]`
- error[invalid-argument-type] src/schemathesis/specs/openapi/schemas.py:429:49: Argument is incorrect: Expected `D`, found `dict[str, Any]`
- Found 416 diagnostics
+ Found 414 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[invalid-argument-type] mesonbuild/ast/interpreter.py:78:54: Argument is incorrect: Expected `TV_TokenTypes`, found `str`
+ error[invalid-argument-type] mesonbuild/ast/interpreter.py:678:48: Argument to bound method `__init__` is incorrect: Expected `Token[str]`, found `Token[int]`
- error[invalid-argument-type] mesonbuild/backend/ninjabackend.py:491:48: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] mesonbuild/backend/ninjabackend.py:491:55: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] mesonbuild/backend/ninjabackend.py:2197:27: Argument is incorrect: Expected `_T`, found `Literal["_FOR_BUILD"]`
- error[invalid-argument-type] mesonbuild/backend/ninjabackend.py:2197:41: Argument is incorrect: Expected `_T`, found `Literal[""]`
- error[invalid-argument-type] mesonbuild/build.py:255:77: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/build.py:255:81: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/build.py:256:82: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/build.py:256:86: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/build.py:257:92: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/build.py:257:96: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/build.py:258:97: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/build.py:258:101: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/build.py:266:67: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/build.py:266:73: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/build.py:275:35: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/build.py:275:39: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
+ error[invalid-assignment] mesonbuild/cmake/executor.py:29:5: Object of type `PerMachine[None]` is not assignable to `PerMachine[ExternalProgram | None]`
+ error[invalid-assignment] mesonbuild/cmake/executor.py:30:5: Object of type `PerMachine[None]` is not assignable to `PerMachine[str | None]`
- error[invalid-argument-type] mesonbuild/cmake/executor.py:29:74: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/cmake/executor.py:29:80: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/cmake/executor.py:30:63: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/cmake/executor.py:30:69: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/cmake/interpreter.py:994:70: Argument is incorrect: Expected `TV_TokenTypes`, found `Unknown | Literal[""]`
- error[invalid-argument-type] mesonbuild/coredata.py:262:72: Argument is incorrect: Expected `_T`, found `OrderedDict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/coredata.py:262:87: Argument is incorrect: Expected `_T`, found `OrderedDict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/coredata.py:285:68: Argument is incorrect: Expected `_T`, found `CMakeStateCache`
- error[invalid-argument-type] mesonbuild/coredata.py:285:87: Argument is incorrect: Expected `_T`, found `CMakeStateCache`
+ error[invalid-assignment] mesonbuild/dependencies/cmake.py:34:5: Object of type `PerMachine[None]` is not assignable to `PerMachine[CMakeInfo | None]`
- error[invalid-argument-type] mesonbuild/dependencies/cmake.py:34:69: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/dependencies/cmake.py:34:75: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/dependencies/detect.py:101:28: Argument is incorrect: Expected `_T`, found `Literal["Build-time"]`
- error[invalid-argument-type] mesonbuild/dependencies/detect.py:101:42: Argument is incorrect: Expected `_T`, found `Literal["Run-time"]`
+ error[invalid-assignment] mesonbuild/dependencies/pkgconfig.py:32:5: Object of type `PerMachine[bool]` is not assignable to `PerMachine[Literal[False] | PkgConfigInterface | None]`
+ error[invalid-assignment] mesonbuild/dependencies/pkgconfig.py:33:5: Object of type `PerMachine[bool]` is not assignable to `PerMachine[Literal[False] | PkgConfigCLI | None]`
+ error[invalid-assignment] mesonbuild/dependencies/pkgconfig.py:34:5: Object of type `PerMachine[None]` is not assignable to `PerMachine[ExternalProgram | None]`
- error[invalid-argument-type] mesonbuild/dependencies/pkgconfig.py:32:98: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] mesonbuild/dependencies/pkgconfig.py:32:105: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] mesonbuild/dependencies/pkgconfig.py:33:96: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] mesonbuild/dependencies/pkgconfig.py:33:103: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] mesonbuild/dependencies/pkgconfig.py:34:79: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/dependencies/pkgconfig.py:34:85: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/environment.py:77:10: Argument is incorrect: Expected `_T`, found `list[Unknown]`
- error[invalid-argument-type] mesonbuild/environment.py:79:9: Argument is incorrect: Expected `_T`, found `list[Unknown]`
- error[invalid-argument-type] mesonbuild/interpreter/interpreter.py:308:84: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/interpreter/interpreter.py:308:88: Argument is incorrect: Expected `_T`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] mesonbuild/mformat.py:272:115: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal[""]`
- error[invalid-argument-type] mesonbuild/mformat.py:399:119: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal[""]`
- error[invalid-argument-type] mesonbuild/mformat.py:662:99: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal[","]`
- error[invalid-argument-type] mesonbuild/mformat.py:664:120: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal[""]`
+ error[invalid-assignment] mesonbuild/modules/rust.py:89:5: Object of type `PerMachine[None]` is not assignable to `PerMachine[ExternalProgram | None]`
- error[invalid-argument-type] mesonbuild/modules/rust.py:89:67: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/modules/rust.py:89:73: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] mesonbuild/mparser.py:701:65: Argument is incorrect: Expected `TV_TokenTypes`, found `None`
+ error[invalid-argument-type] mesonbuild/mparser.py:701:65: Argument is incorrect: Argument type `None` does not satisfy constraints of type variable `TV_TokenTypes`
- error[invalid-argument-type] mesonbuild/mparser.py:727:173: Argument is incorrect: Expected `TV_TokenTypes`, found `None`
+ error[invalid-argument-type] mesonbuild/mparser.py:727:173: Argument is incorrect: Argument type `None` does not satisfy constraints of type variable `TV_TokenTypes`
- error[invalid-argument-type] mesonbuild/rewriter.py:152:62: Argument is incorrect: Expected `TV_TokenTypes`, found `str`
- error[invalid-argument-type] mesonbuild/rewriter.py:164:57: Argument is incorrect: Expected `TV_TokenTypes`, found `bool`
- error[invalid-argument-type] mesonbuild/rewriter.py:178:52: Argument is incorrect: Expected `TV_TokenTypes`, found `str`
- error[invalid-argument-type] mesonbuild/rewriter.py:196:58: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal[""]`
- error[invalid-argument-type] mesonbuild/rewriter.py:275:62: Argument is incorrect: Expected `TV_TokenTypes`, found `str`
- error[invalid-argument-type] mesonbuild/rewriter.py:299:52: Argument is incorrect: Expected `TV_TokenTypes`, found `str`
- error[invalid-argument-type] mesonbuild/rewriter.py:730:107: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal["[]"]`
- error[invalid-argument-type] mesonbuild/rewriter.py:735:95: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal["extra_files"]`
- error[invalid-argument-type] mesonbuild/rewriter.py:782:69: Argument is incorrect: Expected `TV_TokenTypes`, found `str`
- error[invalid-argument-type] mesonbuild/rewriter.py:904:82: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal[""]`
- error[invalid-argument-type] mesonbuild/rewriter.py:906:82: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal[""]`
- error[invalid-argument-type] mesonbuild/rewriter.py:907:87: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal["files"]`
- error[invalid-argument-type] mesonbuild/rewriter.py:908:89: Argument is incorrect: Expected `TV_TokenTypes`, found `str`
- error[invalid-argument-type] mesonbuild/rewriter.py:913:82: Argument is incorrect: Expected `TV_TokenTypes`, found `Literal[""]`
- error[invalid-argument-type] mesonbuild/rewriter.py:915:89: Argument is incorrect: Expected `TV_TokenTypes`, found `str`
+ error[invalid-argument-type] mesonbuild/rewriter.py:914:48: Argument to bound method `__init__` is incorrect: Expected `Token[str]`, found `Token[int]`
- error[invalid-argument-type] mesonbuild/rewriter.py:918:65: Argument is incorrect: Expected `TV_TokenTypes`, found `str`
- error[invalid-argument-type] mesonbuild/scripts/clippy.py:19:58: Argument is incorrect: Expected `_T`, found `list[Unknown]`
- error[invalid-argument-type] mesonbuild/scripts/clippy.py:19:62: Argument is incorrect: Expected `_T`, found `list[Unknown]`
- error[invalid-argument-type] mesonbuild/scripts/rustdoc.py:26:58: Argument is incorrect: Expected `_T`, found `list[Unknown]`
- error[invalid-argument-type] mesonbuild/scripts/rustdoc.py:26:62: Argument is incorrect: Expected `_T`, found `list[Unknown]`
+ error[invalid-assignment] run_project_tests.py:557:5: Object of type `PerMachine[None]` is not assignable to attribute `class_cmakeinfo` of type `PerMachine[CMakeInfo | None]`
+ error[invalid-assignment] run_project_tests.py:558:5: Object of type `PerMachine[bool]` is not assignable to attribute `class_impl` of type `PerMachine[Literal[False] | PkgConfigInterface | None]`
+ error[invalid-assignment] run_project_tests.py:559:5: Object of type `PerMachine[bool]` is not assignable to attribute `class_cli_impl` of type `PerMachine[Literal[False] | PkgConfigCLI | None]`
+ error[invalid-assignment] run_project_tests.py:560:5: Object of type `PerMachine[None]` is not assignable to attribute `pkg_bin_per_machine` of type `PerMachine[ExternalProgram | None]`
- error[invalid-argument-type] run_project_tests.py:557:50: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] run_project_tests.py:557:56: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] run_project_tests.py:558:48: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] run_project_tests.py:558:55: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] run_project_tests.py:559:52: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] run_project_tests.py:559:59: Argument is incorrect: Expected `_T`, found `Literal[False]`
- error[invalid-argument-type] run_project_tests.py:560:57: Argument is incorrect: Expected `_T`, found `None`
- error[invalid-argument-type] run_project_tests.py:560:63: Argument is incorrect: Expected `_T`, found `None`
- Found 1395 diagnostics
+ Found 1334 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ error[invalid-return-type] src/_pytest/capture.py:968:16: Return type does not match returned value: expected `CaptureResult[AnyStr]`, found `CaptureResult[str]`
- error[invalid-argument-type] testing/test_capture.py:945:24: Argument is incorrect: Expected `AnyStr`, found `Literal["out"]`
- error[invalid-argument-type] testing/test_capture.py:945:31: Argument is incorrect: Expected `AnyStr`, found `Literal["err"]`
- error[invalid-argument-type] testing/test_capture.py:955:32: Argument is incorrect: Expected `AnyStr`, found `Literal["out"]`
- error[invalid-argument-type] testing/test_capture.py:955:39: Argument is incorrect: Expected `AnyStr`, found `Literal["err"]`
- error[invalid-argument-type] testing/test_capture.py:956:32: Argument is incorrect: Expected `AnyStr`, found `Literal["wrong"]`
- error[invalid-argument-type] testing/test_capture.py:956:41: Argument is incorrect: Expected `AnyStr`, found `Literal["err"]`
- error[invalid-argument-type] testing/test_capture.py:959:43: Argument is incorrect: Expected `AnyStr`, found `Literal["out"]`
- error[invalid-argument-type] testing/test_capture.py:959:50: Argument is incorrect: Expected `AnyStr`, found `Literal["err"]`
- Found 791 diagnostics
+ Found 784 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-argument-type] src/bokeh/models/util/structure.py:258:13: Argument is incorrect: Expected `GlyphType`, found `Scatter`
- error[invalid-argument-type] src/bokeh/models/util/structure.py:264:38: Argument is incorrect: Expected `GlyphType`, found `MultiLine`
- error[invalid-argument-type] src/bokeh/plotting/_graph.py:131:9: Argument is incorrect: Expected `GlyphType`, found `Glyph | None`
- error[invalid-argument-type] src/bokeh/plotting/_graph.py:134:9: Argument is incorrect: Expected `GlyphType | None`, found `Glyph | None`
- error[invalid-argument-type] src/bokeh/plotting/_graph.py:149:9: Argument is incorrect: Expected `GlyphType`, found `Glyph | None`
- error[invalid-argument-type] src/bokeh/plotting/_graph.py:152:9: Argument is incorrect: Expected `GlyphType | None`, found `Glyph | None`
- error[invalid-argument-type] src/bokeh/plotting/_renderer.py:126:9: Argument is incorrect: Expected `GlyphType`, found `Glyph | None`
+ error[invalid-argument-type] src/bokeh/plotting/_renderer.py:139:43: Argument to function `update_legend` is incorrect: Expected `GlyphRenderer[Glyph]`, found `GlyphRenderer[Glyph | None | str]`
- error[invalid-argument-type] src/bokeh/plotting/_renderer.py:129:9: Argument is incorrect: Expected `GlyphType | None`, found `Glyph | None`
+ error[invalid-return-type] src/bokeh/plotting/_renderer.py:141:12: Return type does not match returned value: expected `GlyphRenderer[Glyph]`, found `GlyphRenderer[Glyph | None | str]`
- error[invalid-argument-type] src/bokeh/plotting/contour.py:248:37: Argument is incorrect: Expected `GlyphType`, found `MultiPolygons`
+ error[invalid-argument-type] src/bokeh/plotting/contour.py:248:9: Argument is incorrect: Expected `GlyphRenderer[Glyph]`, found `GlyphRenderer[MultiPolygons]`
- error[invalid-argument-type] src/bokeh/plotting/contour.py:249:37: Argument is incorrect: Expected `GlyphType`, found `MultiLine`
+ error[invalid-argument-type] src/bokeh/plotting/contour.py:249:9: Argument is incorrect: Expected `GlyphRenderer[Glyph]`, found `GlyphRenderer[MultiLine]`
- Found 952 diagnostics
+ Found 946 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- error[invalid-argument-type] src/integrations/prefect-dbt/tests/core/test_profiles.py:237:27: Argument is incorrect: Expected `SecretStr | Secret[T]`, found `Literal["super-secret-password"]`
+ error[invalid-argument-type] src/integrations/prefect-dbt/tests/core/test_profiles.py:237:27: Argument is incorrect: Expected `SecretStr | Secret[Unknown]`, found `Literal["super-secret-password"]`
- error[invalid-argument-type] src/integrations/prefect-dbt/tests/core/test_profiles.py:256:27: Argument is incorrect: Expected `SecretStr | Secret[T]`, found `Literal["super-secret-password"]`
+ error[invalid-argument-type] src/integrations/prefect-dbt/tests/core/test_profiles.py:256:27: Argument is incorrect: Expected `SecretStr | Secret[Unknown]`, found `Literal["super-secret-password"]`
- error[invalid-argument-type] src/prefect/cli/deploy.py:1092:17: Argument is incorrect: Expected `SecretStr | Secret[T]`, found `str`
+ error[invalid-argument-type] src/prefect/cli/deploy.py:1092:17: Argument is incorrect: Expected `SecretStr | Secret[Unknown]`, found `str`
- error[invalid-argument-type] src/prefect/results.py:988:13: Argument is incorrect: Expected `R`, found `dict[str, Any]`
- error[invalid-argument-type] src/prefect/task_worker.py:537:9: Argument is incorrect: Expected `R`, found `dict[str, Any]`
- Found 4503 diagnostics
+ Found 4501 diagnostics

```
</details>


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-03 16:35_

---

_@carljm approved on 2025-06-03 16:59_

Nice! This looks right to me.

---

_Merged by @carljm on 2025-06-03 16:59_

---

_Closed by @carljm on 2025-06-03 16:59_

---

_Branch deleted on 2025-06-28 23:12_

---
