```yaml
number: 21238
title: "[ty] `dict` is not assignable to `TypedDict`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/typed-dict-assignability
created_at: 2025-11-03T02:52:07Z
updated_at: 2025-11-03T21:57:51Z
url: https://github.com/astral-sh/ruff/pull/21238
synced_at: 2026-01-12T15:57:19Z
```

# [ty] `dict` is not assignable to `TypedDict`

---

_@ibraheemdev_

## Summary

A lot of the bidirectional inference work relies on `dict` not being assignable to `TypedDict`, so I think it makes sense to add this before fully implementing https://github.com/astral-sh/ty/issues/1387.

---

_Label `ty` added by @ibraheemdev on 2025-11-03 02:52_

---

_Review requested from @carljm by @ibraheemdev on 2025-11-03 02:52_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-03 02:52_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-03 02:52_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-03 02:52_

---

_@ibraheemdev reviewed on 2025-11-03 02:53_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/comprehensions/basic.md`:167 on 2025-11-03 02:53_

@sharkdp it looks like the type context is not being propagated correctly here, and only seemed so because of `dict[str, str]` being assignable to `Person`.

---

_Comment by @github-actions[bot] on 2025-11-03 02:54_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-03 17:12:53.086444331 +0000
+++ new-output.txt	2025-11-03 17:12:56.296455134 +0000
@@ -953,14 +953,16 @@
 typeddicts_extra_items.py:293:44: error[invalid-key] Invalid key for TypedDict `ClosedMovie`: Unknown key "year"
 typeddicts_extra_items.py:299:54: error[invalid-key] Invalid key for TypedDict `MovieExtraStr`: Unknown key "summary"
 typeddicts_extra_items.py:302:54: error[invalid-key] Invalid key for TypedDict `MovieExtraInt`: Unknown key "year"
-typeddicts_extra_items.py:303:1: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | int]` is not assignable to `Mapping[str, int]`
 typeddicts_extra_items.py:310:5: error[type-assertion-failure] Argument does not have asserted type `list[tuple[str, int | str]]`
 typeddicts_extra_items.py:311:5: error[type-assertion-failure] Argument does not have asserted type `list[int | str]`
 typeddicts_extra_items.py:327:5: error[unresolved-attribute] Object of type `IntDict` has no attribute `clear`
 typeddicts_extra_items.py:329:52: error[invalid-key] Invalid key for TypedDict `IntDictWithNum`: Unknown key "bar" - did you mean "num"?
+typeddicts_extra_items.py:337:1: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `clear`
 typeddicts_extra_items.py:339:1: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int]`
+typeddicts_extra_items.py:339:13: error[unresolved-attribute] Object of type `IntDictWithNum` has no attribute `popitem`
 typeddicts_extra_items.py:342:27: error[invalid-key] Cannot access `IntDictWithNum` with a key of type `str`. Only string literals are allowed as keys on TypedDicts.
 typeddicts_extra_items.py:343:31: error[invalid-key] Invalid key for TypedDict `IntDictWithNum` of type `str`
+typeddicts_extra_items.py:352:5: error[invalid-assignment] Object of type `dict[str, int]` is not assignable to `IntDict`
 typeddicts_operations.py:22:17: error[invalid-assignment] Invalid assignment to key "name" with declared type `str` on TypedDict `Movie`: value of type `Literal[1982]`
 typeddicts_operations.py:23:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal[""]`
 typeddicts_operations.py:24:7: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "other"
@@ -969,6 +971,7 @@
 typeddicts_operations.py:29:42: error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `Movie`: value of type `float`
 typeddicts_operations.py:32:36: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
+typeddicts_operations.py:47:1: error[unresolved-attribute] Object of type `Movie` has no attribute `clear`
 typeddicts_operations.py:62:1: error[unresolved-attribute] Object of type `MovieOptional` has no attribute `clear`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
 typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only
@@ -988,5 +991,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 990 diagnostics
+Found 993 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-11-03 02:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:3430:13: error[invalid-assignment] Object of type `KeysView[object]` is not assignable to `Sequence[@Todo] | AbstractSet[AnyKeyT@_zaggregate]`
+ aioredis/client.py:3430:13: error[invalid-assignment] Object of type `tuple[KeysView[object], ValuesView[object]]` is not assignable to `Sequence[@Todo] | AbstractSet[AnyKeyT@_zaggregate]`
- aioredis/client.py:3430:24: error[invalid-assignment] Object of type `ValuesView[object]` is not assignable to `ValuesView[int | float] | None`
+ aioredis/client.py:3430:24: error[invalid-assignment] Object of type `tuple[KeysView[object], ValuesView[object]]` is not assignable to `ValuesView[int | float] | None`

python-chess (https://github.com/niklasf/python-chess)
+ chess/engine.py:289:9: error[invalid-assignment] Object of type `(InfoDict & ~AlwaysFalsy) | dict[Unknown, Unknown]` is not assignable to attribute `info` of type `InfoDict`
- Found 24 diagnostics
+ Found 25 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ pyinstrument/context_manager.py:41:9: error[invalid-assignment] Object of type `dict[str, @Todo]` is not assignable to attribute `options` of type `ProfileContextOptions`
- Found 41 diagnostics
+ Found 42 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/bootstrap/core.py:233:18: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `QueryInfo`
+ lib/spack/spack/bootstrap/core.py:233:18: error[invalid-assignment] Object of type `tuple[partial[Unknown], dict[Unknown, Unknown]]` is not assignable to `QueryInfo`
- lib/spack/spack/bootstrap/core.py:246:18: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `QueryInfo`
+ lib/spack/spack/bootstrap/core.py:246:18: error[invalid-assignment] Object of type `tuple[partial[Unknown], dict[Unknown, Unknown]]` is not assignable to `QueryInfo`
+ lib/spack/spack/vendor/distro/distro.py:994:16: error[invalid-return-type] Return type does not match returned value: expected `InfoDict`, found `dict[str, str | dict[str, str]]`
- Found 7869 diagnostics
+ Found 7870 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/urllib3/util/ssl_.py:86:5: error[invalid-assignment] Object of type `Literal[16777216]` is not assignable to `Literal[Options.OP_NO_SSLv2]`
+ src/pip/_vendor/urllib3/util/ssl_.py:86:5: error[invalid-assignment] Object of type `tuple[Literal[16777216], Literal[33554432]]` is not assignable to `Literal[Options.OP_NO_SSLv2]`
- src/pip/_vendor/urllib3/util/ssl_.py:86:18: error[invalid-assignment] Object of type `Literal[33554432]` is not assignable to `Literal[Options.OP_NO_SSLv3]`
+ src/pip/_vendor/urllib3/util/ssl_.py:86:18: error[invalid-assignment] Object of type `tuple[Literal[16777216], Literal[33554432]]` is not assignable to `Literal[Options.OP_NO_SSLv3]`

yarl (https://github.com/aio-libs/yarl)
+ tests/test_pickle.py:29:20: error[invalid-argument-type] Argument to bound method `__setstate__` is incorrect: Expected `tuple[tuple[str, str, str, str, str]] | tuple[None, _InternalURLCache]`, found `tuple[None, dict[Unknown | str, Unknown | tuple[str, str, str, str, str]]]`
- Found 46 diagnostics
+ Found 47 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/contrib/rightsizer_soaconfigs_update.py:282:9: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `KubernetesRecommendation | CassandraRecommendation`
- Found 982 diagnostics
+ Found 983 diagnostics

pyjwt (https://github.com/jpadilla/pyjwt)
+ jwt/api_jws.py:50:13: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to attribute `options` of type `SigOptions`
+ jwt/api_jwt.py:77:16: error[invalid-return-type] Return type does not match returned value: expected `FullOptions`, found `dict[Unknown, Unknown]`
- Found 37 diagnostics
+ Found 39 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/clients/creating.py:21:5: error[invalid-assignment] Object of type `(RawBody & ~None) | dict[Unknown, Unknown]` is not assignable to `RawBody | None`
+ kopf/_cogs/clients/creating.py:23:9: warning[possibly-missing-attribute] Attribute `setdefault` may be missing on object of type `RawBody | None`
+ kopf/_cogs/clients/creating.py:25:9: warning[possibly-missing-attribute] Attribute `setdefault` may be missing on object of type `RawBody | None`
+ kopf/_cogs/clients/creating.py:27:44: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `RawBody | None`
+ kopf/_core/engines/admission.py:449:5: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | str] | Unknown]` is not assignable to `Collection[MatchExpression]`
+ kopf/_kits/webhacks.py:88:42: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `AsyncGenerator[WebhookClientConfig, None]`, found `AsyncGenerator[object, None]`
+ kopf/_kits/webhacks.py:92:42: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `AsyncGenerator[WebhookClientConfig, None]`, found `AsyncGenerator[object, None]`
- Found 47 diagnostics
+ Found 54 diagnostics

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
+ src/pywinctl/_pywinctl_linux.py:307:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, _WINDICT].__setitem__(key: str, value: _WINDICT, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown | str, Unknown | dict[Unknown, Unknown]]` on object of type `dict[str, _WINDICT]`
- Found 30 diagnostics
+ Found 31 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/json_schema.py:1803:9: error[invalid-assignment] Object of type `Any | None` is not assignable to `ConfigDict`
- Found 775 diagnostics
+ Found 776 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/connection.py:998:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 362 diagnostics
+ Found 361 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/data/btanalysis/bt_fileutils.py:238:12: error[invalid-return-type] Return type does not match returned value: expected `list[BacktestHistoryEntryType]`, found `list[dict[Unknown | str, Unknown | str] | Unknown]`
+ freqtrade/exchange/exchange_utils.py:113:9: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown] | Unknown]` is not assignable to `list[TradeModeType]`
+ freqtrade/optimize/backtesting.py:1820:13: error[invalid-assignment] Object of type `dict[str, Any]` is not assignable to attribute `results` of type `BacktestResultType`
+ freqtrade/plugins/pairlist/PercentChangePairList.py:219:9: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | str | None] | Unknown]` is not assignable to `list[SymbolWithPercentage]`
+ freqtrade/rpc/api_server/api_download_data.py:32:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ProgressTask].__setitem__(key: str, value: ProgressTask, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown]` on object of type `dict[str, ProgressTask]`
+ freqtrade/rpc/api_server/api_download_data.py:71:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, JobsContainer].__setitem__(key: str, value: JobsContainer, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | str | None | dict[Unknown, Unknown] | bool]` on object of type `dict[str, JobsContainer]`
+ freqtrade/rpc/api_server/api_pairlists.py:92:5: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, JobsContainer].__setitem__(key: str, value: JobsContainer, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | str | None | bool | dict[Unknown, Unknown]]` on object of type `dict[str, JobsContainer]`
- Found 460 diagnostics
+ Found 467 diagnostics

mypy (https://github.com/python/mypy)
+ mypy/build.py:974:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, FgDepMeta].__setitem__(key: str, value: FgDepMeta, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | (str & ~AlwaysFalsy) | int]` on object of type `dict[str, FgDepMeta]`
+ mypy/typeshed/stdlib/tkinter/__init__.pyi:584:9: error[invalid-parameter-default] Default value of type `dict[Unknown, Unknown]` is not assignable to annotated parameter type `_GridIndexInfo`
+ mypy/typeshed/stdlib/tkinter/__init__.pyi:594:9: error[invalid-parameter-default] Default value of type `dict[Unknown, Unknown]` is not assignable to annotated parameter type `_GridIndexInfo`
- Found 1839 diagnostics
+ Found 1842 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/audit_logs.py:909:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/components.py:871:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/embeds.py:784:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/ext/commands/context.py:285:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/guild.py:664:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/mentions.py:137:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/message.py:736:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ discord/presences.py:60:9: error[invalid-assignment] Object of type `(ClientStatus & ~AlwaysFalsy) | dict[Unknown, Unknown]` is not assignable to `ClientStatus`
+ discord/role.py:450:9: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown] | Unknown]` is not assignable to `list[RolePositionUpdate]`
+ discord/role.py:700:9: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | int] | Unknown]` is not assignable to `list[RolePositionUpdate]`
- discord/ui/label.py:116:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 517 diagnostics
+ Found 512 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/theming.py:340:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 521 diagnostics
+ Found 520 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/tests/test_cftimeindex_resample.py:260:22: error[no-matching-overload] No overload of function `date_range` matches arguments
+ xarray/tests/test_cftimeindex_resample.py:259:5: error[invalid-assignment] Object of type `dict[str, str | int]` is not assignable to `DateRangeKwargs`

meson (https://github.com/mesonbuild/meson)
+ docs/refman/generatorjson.py:31:16: error[invalid-return-type] Return type does not match returned value: expected `list[Type]`, found `list[dict[Unknown | str, Unknown | str | list[Type] | list[Unknown]] | Unknown]`
- mesonbuild/build.py:687:17: error[invalid-assignment] Object of type `list[str | Literal[False] | None]` is not assignable to `list[str | Literal[False]]`
+ mesonbuild/build.py:687:17: error[invalid-assignment] Object of type `list[Unknown | str | None]` is not assignable to `list[str | Literal[False]]`
+ mesonbuild/build.py:3120:69: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `BuildTargetKeywordArguments`, found `dict[Unknown | str, Unknown | bool | dict[Unknown | str, Unknown | list[str]] | list[IncludeDirs] | list[Dependency]]`
+ mesonbuild/dependencies/dub.py:469:16: error[invalid-return-type] Return type does not match returned value: expected `list[FindTargetEntry]`, found `list[dict[Unknown | str, Unknown | str] | Unknown]`
+ mesonbuild/interpreter/compiler.py:737:9: error[invalid-assignment] Object of type `(ExtractRequired & ~AlwaysFalsy) | dict[Unknown | str, Unknown | bool]` is not assignable to `ExtractRequired | None`
+ mesonbuild/interpreter/compiler.py:738:62: error[invalid-argument-type] Argument to function `extract_required_kwarg` is incorrect: Expected `ExtractRequired`, found `ExtractRequired | None`
+ mesonbuild/interpreter/compiler.py:844:9: error[invalid-assignment] Object of type `(ExtractRequired & ~AlwaysFalsy) | dict[Unknown | str, Unknown | bool]` is not assignable to `ExtractRequired | None`
+ mesonbuild/interpreter/compiler.py:845:62: error[invalid-argument-type] Argument to function `extract_required_kwarg` is incorrect: Expected `ExtractRequired`, found `ExtractRequired | None`
+ mesonbuild/interpreter/interpreter.py:577:33: error[invalid-argument-type] Argument to bound method `lookup` is incorrect: Expected `DependencyObjectKWs`, found `dict[Unknown | str, Unknown | MachineChoice]`
+ mesonbuild/interpreter/interpreter.py:1375:31: error[invalid-argument-type] Argument to bound method `summary_impl` is incorrect: Expected `Summary`, found `dict[Unknown | str, Unknown | bool | str]`
+ mesonbuild/interpreter/interpreter.py:1395:67: error[invalid-argument-type] Argument to bound method `summary_impl` is incorrect: Expected `Summary`, found `dict[Unknown | str, Unknown | bool | None]`
+ mesonbuild/interpreter/interpreter.py:2282:40: error[invalid-argument-type] Argument to bound method `make_test` is incorrect: Expected `BaseTest`, found `dict[str, Any]`
- mesonbuild/interpreter/interpreter.py:3395:23: error[invalid-assignment] Object of type `list[str]` is not assignable to `tuple[str, Any]`
+ mesonbuild/interpreter/interpreter.py:3395:23: error[invalid-assignment] Object of type `tuple[list[File], list[str]]` is not assignable to `tuple[str, Any]`
- mesonbuild/interpreter/interpreter.py:3400:23: error[invalid-assignment] Object of type `list[str]` is not assignable to `tuple[str, Any]`
+ mesonbuild/interpreter/interpreter.py:3400:23: error[invalid-assignment] Object of type `tuple[list[File], list[str]]` is not assignable to `tuple[str, Any]`
+ mesonbuild/interpreter/interpreter.py:3409:9: error[invalid-assignment] Object of type `dict[str | Unknown, object]` is not assignable to `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3442:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: @Todo | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: @Todo | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | @Todo | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["include_directories"]` and a value of type `list[IncludeDirs]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- mesonbuild/scripts/coverage.py:19:9: error[invalid-assignment] Object of type `None | str` is not assignable to `str`
+ mesonbuild/scripts/coverage.py:19:9: error[invalid-assignment] Object of type `tuple[None, None] | tuple[str, str]` is not assignable to `str`
- mesonbuild/tooldetect.py:83:5: error[invalid-assignment] Object of type `None | str` is not assignable to `str`
+ mesonbuild/tooldetect.py:83:5: error[invalid-assignment] Object of type `tuple[None, None] | tuple[str, str]` is not assignable to `str`
- mesonbuild/wrap/wrap.py:686:13: error[invalid-assignment] Object of type `list[str | Unknown | None]` is not assignable to `list[str]`
+ mesonbuild/wrap/wrap.py:686:13: error[invalid-assignment] Object of type `list[Unknown | str | None]` is not assignable to `list[str]`
+ unittests/allplatformstests.py:2153:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DependencyObjectKWs`, found `dict[Unknown | str, Unknown | bool]`
+ unittests/internaltests.py:668:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DependencyObjectKWs`, found `dict[Unknown | str, Unknown | bool]`
+ unittests/internaltests.py:671:59: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DependencyObjectKWs`, found `dict[Unknown | str, Unknown | bool]`
+ unittests/internaltests.py:673:69: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DependencyObjectKWs`, found `dict[Unknown | str, Unknown | bool]`
+ unittests/linuxliketests.py:154:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DependencyObjectKWs`, found `dict[Unknown | str, Unknown | bool]`
+ unittests/linuxliketests.py:161:69: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DependencyObjectKWs`, found `dict[Unknown | str, Unknown | bool]`
+ unittests/linuxliketests.py:175:57: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DependencyObjectKWs`, found `dict[Unknown | str, Unknown | bool]`
+ unittests/linuxliketests.py:1149:73: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `DependencyObjectKWs`, found `dict[Unknown | str, Unknown | bool]`
- Found 1658 diagnostics
+ Found 1679 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/net/ephemeral.py:140:13: error[unresolved-attribute] Object of type `str` has no attribute `get`
+ cloudinit/net/ephemeral.py:140:13: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `str | Unknown`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-docker/prefect_docker/images.py:64:5: error[invalid-assignment] Object of type `dict[str, dict[str, Any] | str | None | bool]` is not assignable to `dict[str, dict[str, Any]]`
+ src/integrations/prefect-docker/prefect_docker/images.py:64:5: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | None | bool | dict[str, Any]]` is not assignable to `dict[str, dict[str, Any]]`
- src/prefect/cli/deploy/_core.py:78:9: error[invalid-assignment] Object of type `str` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:78:9: error[invalid-assignment] Object of type `dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/locking/filesystem.py:138:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, _LockInfo].__setitem__(key: str, value: _LockInfo, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | str | datetime | None | Path]` on object of type `dict[str, _LockInfo]`
+ src/prefect/locking/filesystem.py:187:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, _LockInfo].__setitem__(key: str, value: _LockInfo, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | str | datetime | None | Path]` on object of type `dict[str, _LockInfo]`
- Found 3301 diagnostics
+ Found 3303 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/structured_configs/_implementations.py:3506:9: error[invalid-assignment] Object of type `@Todo | dict[Unknown, Unknown]` is not assignable to `DataclassOptions`
- tests/annotations/declarations.py:1229:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:1232:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:1235:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:1238:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:1243:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:1434:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/annotations/declarations.py:1435:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 558 diagnostics
+ Found 552 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
+ cwltool/cwlprov/ro.py:263:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, Aggregate].__setitem__(key: str, value: Aggregate, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | str | None]` on object of type `dict[str, Aggregate]`
- Found 138 diagnostics
+ Found 139 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/catalog/add_book/tests/test_load_book.py:124:45: error[invalid-argument-type] Argument to function `find_entity` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str]`
+ openlibrary/catalog/add_book/tests/test_load_book.py:126:47: error[invalid-argument-type] Argument to function `find_entity` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str]`
+ openlibrary/catalog/utils/tests/test_catalog_utils.py:34:31: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str]`
+ openlibrary/core/lists/model.py:498:20: error[invalid-return-type] Return type does not match returned value: expected `str | ThingReferenceDict | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | (str & ~AlwaysFalsy)]`
+ openlibrary/core/lists/model.py:503:20: error[invalid-return-type] Return type does not match returned value: expected `str | ThingReferenceDict | AnnotatedSeedDict`, found `dict[Unknown | str, Unknown | str]`
+ openlibrary/plugins/openlibrary/tests/test_lists.py:106:18: error[invalid-argument-type] Argument to function `normalize_input_seed` is incorrect: Expected `ThingReferenceDict | AnnotatedSeedDict | str`, found `dict[Unknown | str, Unknown | str]`
+ openlibrary/plugins/worksearch/tests/test_worksearch.py:19:9: error[invalid-argument-type] Argument to function `get_doc` is incorrect: Expected `SolrDocument`, found `dict[Unknown | str, Unknown | list[Unknown | str] | str | int | None]`
+ openlibrary/tests/catalog/test_utils.py:69:31: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]]`
+ openlibrary/tests/catalog/test_utils.py:70:31: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]]`
+ openlibrary/tests/catalog/test_utils.py:71:31: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]]`
+ openlibrary/tests/catalog/test_utils.py:72:31: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]]`
+ openlibrary/tests/catalog/test_utils.py:73:31: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]]`
+ openlibrary/tests/catalog/test_utils.py:75:31: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]]`
+ openlibrary/tests/catalog/test_utils.py:77:31: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]]`
+ openlibrary/tests/catalog/test_utils.py:78:31: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]]`
+ openlibrary/tests/catalog/test_utils.py:81:9: error[invalid-argument-type] Argument to function `author_dates_match` is incorrect: Expected `AuthorImportDict`, found `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str]]`
- Found 916 diagnostics
+ Found 932 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/prototype/datasets/_builtin/cub200.py:175:9: error[invalid-assignment] Object of type `tuple[str, tuple[str, BinaryIO]]` is not assignable to `tuple[tuple[str, tuple[str, BinaryIO]], Any]`
+ torchvision/prototype/datasets/_builtin/cub200.py:175:9: error[invalid-assignment] Object of type `tuple[tuple[str, tuple[str, BinaryIO]], Any]` is not assignable to `tuple[tuple[str, tuple[str, BinaryIO]], Any]`
- torchvision/transforms/functional.py:1205:9: error[invalid-assignment] Object of type `list[int | float | (list[int | float] & Number)]` is not assignable to `list[int | float]`
+ torchvision/transforms/functional.py:1205:9: error[invalid-assignment] Object of type `list[Unknown | (list[int | float] & Number) | float]` is not assignable to `list[int | float]`
- torchvision/transforms/functional.py:1225:13: error[invalid-assignment] Object of type `list[int | float]` is not assignable to `list[int] | None`
+ torchvision/transforms/functional.py:1225:13: error[invalid-assignment] Object of type `list[Unknown | int | float]` is not assignable to `list[int] | None`
- torchvision/transforms/v2/functional/_geometry.py:649:9: error[invalid-assignment] Object of type `list[int | float | (list[int | float] & Number)]` is not assignable to `list[int | float]`
+ torchvision/transforms/v2/functional/_geometry.py:649:9: error[invalid-assignment] Object of type `list[Unknown | (list[int | float] & Number) | float]` is not assignable to `list[int | float]`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/format.py:74:16: error[invalid-return-type] Return type does not match returned value: expected `PyprojectFormatter | dict[str, str]`, found `dict[str, Literal["*"]]`
- Found 48 diagnostics
+ Found 49 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
+ sklearn/linear_model/_quantile.py:259:18: error[no-matching-overload] No overload of function `linprog` matches arguments
- sklearn/linear_model/_quantile.py:286:18: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- sklearn/linear_model/_quantile.py:286:40: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- Found 2538 diagnostics
+ Found 2537 diagnostics

altair (https://github.com/vega/altair)
+ altair/datasets/_constraints.py:57:24: error[invalid-return-type] Return type does not match returned value: expected `Metadata`, found `dict[str, @Todo]`
+ altair/datasets/_constraints.py:60:16: error[invalid-return-type] Return type does not match returned value: expected `Metadata`, found `dict[Unknown, Unknown]`
+ altair/datasets/_constraints.py:111:33: error[invalid-argument-type] Argument to bound method `from_metadata` is incorrect: Expected `Metadata`, found `dict[str, @Todo]`
+ altair/datasets/_reader.py:177:9: error[invalid-assignment] Object of type `(Metadata & Top[Mapping[Unknown, object]]) | (Path & Top[Mapping[Unknown, object]]) | (str & Top[Mapping[Unknown, object]]) | dict[Unknown | str, Unknown]` is not assignable to `Metadata | Path | str`
+ altair/datasets/_reader.py:178:28: error[invalid-argument-type] Argument to bound method `_solve` is incorrect: Expected `Metadata`, found `Metadata | Path | str`
+ altair/vegalite/v6/api.py:664:12: error[invalid-return-type] Return type does not match returned value: expected `_ConditionExtra`, found `dict[Unknown | str, Unknown]`
- tests/vegalite/v6/test_api.py:410:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/vegalite/v6/test_api.py:430:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/vegalite/v6/test_api.py:465:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/vegalite/v6/test_theme.py:416:12: error[invalid-return-type] Return type does not match returned value: expected `ThemeConfig`, found `dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | RectConfigKwds | AreaConfigKwds | ... omitted 7 union elements]]`
- Found 1037 diagnostics
+ Found 1041 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/tests/test_array_api.py:385:13: error[invalid-assignment] Object of type `NDArray` is not assignable to `None`
+ python/tests/test_array_api.py:385:13: error[invalid-assignment] Object of type `tuple[EGraph, NDArray, NDArray, Program]` is not assignable to `None`
- python/tests/test_array_api.py:385:16: error[invalid-assignment] Object of type `NDArray` is not assignable to `None`
+ python/tests/test_array_api.py:385:16: error[invalid-assignment] Object of type `tuple[EGraph, NDArray, NDArray, Program]` is not assignable to `None`

streamlit (https://github.com/streamlit/streamlit)
+ lib/streamlit/elements/widgets/data_editor.py:144:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | dict[Unknown, Unknown] | list[Unknown]] | Any` is not assignable to `EditingState`
+ lib/streamlit/elements/widgets/data_editor.py:168:67: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'possibly-missing-attribute'
- Found 168 diagnostics
+ Found 170 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/ddtrace_api/patch.py:74:5: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to `tuple[list[Unknown], ...]`
+ ddtrace/contrib/internal/ddtrace_api/patch.py:74:5: error[invalid-assignment] Object of type `tuple[list[Unknown], dict[Unknown, Unknown]]` is not assignable to `tuple[list[Unknown], ...]`
+ ddtrace/llmobs/_experiment.py:189:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, UpdatableDatasetRecord].__setitem__(key: str, value: UpdatableDatasetRecord, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | str]` on object of type `dict[str, UpdatableDatasetRecord]`
+ ddtrace/llmobs/_experiment.py:194:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: DatasetRecord, /) -> None, (key: slice[Any, Any, Any], value: Iterable[DatasetRecord], /) -> None]` cannot be called with a key of type `int` and a value of type `dict[Unknown | str, Unknown | str]` on object of type `list[DatasetRecord]`
+ ddtrace/llmobs/_experiment.py:604:16: error[invalid-return-type] Return type does not match returned value: expected `LLMObsExperimentEvalMetricEvent`, found `dict[Unknown | str, Unknown | str | int | list[str] | None]`
+ tests/contrib/langgraph/conftest.py:344:16: error[invalid-return-type] Return type does not match returned value: expected `State`, found `dict[Unknown | str, Unknown | list[Unknown]]`
+ tests/llmobs/test_llmobs_evaluator_runner.py:38:34: error[invalid-argument-type] Argument to bound method `enqueue` is incorrect: Expected `LLMObsSpanEvent`, found `dict[Unknown, Unknown]`
+ tests/llmobs/test_llmobs_evaluator_runner.py:56:30: error[invalid-argument-type] Argument to bound method `enqueue` is incorrect: Expected `LLMObsSpanEvent`, found `dict[Unknown | str, Unknown | str]`
+ tests/llmobs/test_llmobs_evaluator_runner.py:82:30: error[invalid-argument-type] Argument to bound method `enqueue` is incorrect: Expected `LLMObsSpanEvent`, found `dict[Unknown | str, Unknown | str]`
+ tests/llmobs/test_llmobs_span_agent_writer.py:40:36: error[invalid-argument-type] Argument to bound method `enqueue` is incorrect: Expected `LLMObsSpanEvent`, found `dict[Unknown, Unknown]`
+ tests/llmobs/test_llmobs_span_agentless_writer.py:34:36: error[invalid-argument-type] Argument to bound method `enqueue` is incorrect: Expected `LLMObsSpanEvent`, found `dict[Unknown, Unknown]`
+ tests/utils.py:1165:16: error[invalid-return-type] Return type does not match returned value: expected `list[TestAgentRequest]`, found `list[dict[str, Any]]`
+ tests/utils.py:1218:16: error[invalid-return-type] Return type does not match returned value: expected `list[dict[str, Any]]`, found `list[TestAgentRequest]`
- Found 8152 diagnostics
+ Found 8163 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/snowflake/tests/conftest.py:175:23: error[invalid-assignment] Object of type `HTTPMessage` is not assignable to `dict[str, Any]`
+ ibis/backends/snowflake/tests/conftest.py:175:23: error[invalid-assignment] Object of type `tuple[str, HTTPMessage]` is not assignable to `dict[str, Any]`
- ibis/expr/types/relations.py:2966:14: error[invalid-assignment] Object of type `Value` is not assignable to `BooleanValue | Deferred | None`
+ ibis/expr/types/relations.py:2966:14: error[invalid-assignment] Object of type `Iterator[Value]` is not assignable to `BooleanValue | Deferred | None`
- ibis/expr/types/relations.py:3011:14: error[invalid-assignment] Object of type `Value` is not assignable to `BooleanValue | Deferred | None`
+ ibis/expr/types/relations.py:3011:14: error[invalid-assignment] Object of type `Iterator[Value]` is not assignable to `BooleanValue | Deferred | None`

jax (https://github.com/google/jax)
- jax/_src/pallas/mosaic/lowering.py:3228:3: error[invalid-assignment] Object of type `Jaxpr` is not assignable to `ClosedJaxpr`
+ jax/_src/pallas/mosaic/lowering.py:3228:3: error[invalid-assignment] Object of type `tuple[Jaxpr, bool]` is not assignable to `ClosedJaxpr`
- jax/_src/pallas/mosaic_gpu/lowering.py:2955:3: error[invalid-assignment] Object of type `Jaxpr` is not assignable to `ClosedJaxpr`
+ jax/_src/pallas/mosaic_gpu/lowering.py:2955:3: error[invalid-assignment] Object of type `tuple[Jaxpr, bool]` is not assignable to `ClosedJaxpr`
- jax/_src/pallas/pallas_call.py:1186:33: error[invalid-assignment] Object of type `list[Any]` is not assignable to `tuple[Any, ...]`
+ jax/_src/pallas/pallas_call.py:1186:33: error[invalid-assignment] Object of type `list[list[Any]]` is not assignable to `tuple[Any, ...]`
- jax/experimental/mosaic/gpu/tcgen05.py:552:19: error[invalid-assignment] Object of type `tuple[int, ...] | None` is not assignable to `tuple[tuple[int, ...], tuple[int, ...]] | None`
+ jax/experimental/mosaic/gpu/tcgen05.py:552:19: error[invalid-assignment] Object of type `tuple[tuple[int, ...], tuple[int, ...]] | tuple[None, None]` is not assignable to `tuple[tuple[int, ...], tuple[int, ...]] | None`
- jax/experimental/mosaic/gpu/tcgen05.py:553:19: error[invalid-assignment] Object of type `tuple[int, ...]` is not assignable to `tuple[tuple[int, ...], tuple[int, ...]]`
+ jax/experimental/mosaic/gpu/tcgen05.py:553:19: error[invalid-assignment] Object of type `tuple[tuple[int, ...], tuple[int, ...]]` is not assignable to `tuple[tuple[int, ...], tuple[int, ...]]`

pandas (https://github.com/pandas-dev/pandas)
- pandas/io/formats/style.py:4316:9: error[invalid-assignment] Object of type `floating[Any]` is not assignable to `int | float`
+ pandas/io/formats/style.py:4316:9: error[invalid-assignment] Object of type `tuple[floating[Any], Literal["zero"]]` is not assignable to `int | float`
+ pandas/io/formats/style_render.py:1896:12: error[invalid-return-type] Return type does not match returned value: expected `list[CSSDict]`, found `list[dict[Unknown | str, Unknown | str] | Unknown]`
- Found 3381 diagnostics
+ Found 3382 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/test/unit/test_type_clinic.py:1078:14: error[invalid-argument-type] Argument to function `proc1` is incorrect: Expected `Model`, found `Literal["f"]`
+ static_frame/test/unit/test_type_clinic.py:1078:21: error[invalid-argument-type] Argument to function `proc1` is incorrect: Expected `Model`, found `Literal[False]`
+ static_frame/test/unit/test_type_clinic.py:1079:14: error[invalid-argument-type] Argument to function `proc1` is incorrect: Expected `Model`, found `Literal[True]`
+ static_frame/test/unit/test_type_clinic.py:1079:22: error[invalid-argument-type] Argument to function `proc1` is incorrect: Expected `Model`, found `Literal["g"]`
+ static_frame/test/unit/test_type_clinic.py:1082:18: error[invalid-argument-type] Argument to function `proc1` is incorrect: Expected `Model`, found `Literal["f"]`
+ static_frame/test/unit/test_type_clinic.py:1082:25: error[invalid-argument-type] Argument to function `proc1` is incorrect: Expected `Model`, found `Literal["g"]`
- Found 2000 diagnostics
+ Found 2006 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/actions/message_send.py:1135:13: error[invalid-assignment] Object of type `dict[str, Unknown | None]` is not assignable to `UserData`
+ zerver/lib/export.py:1951:9: error[invalid-assignment] Object of type `dict[str, list[dict[str, Any] | Unknown] | list[int] | Unknown]` is not assignable to `MessagePartial`
+ zerver/lib/message.py:932:5: error[invalid-assignment] Object of type `dict[str, dict[int, RawUnreadDirectMessageDict] | dict[int, RawUnreadStreamDict] | set[Unknown] | dict[int, RawUnreadDirectMessageGroupDict] | bool]` is not assignable to `RawUnreadMessagesResult`
+ zerver/lib/message.py:992:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, RawUnreadStreamDict].__setitem__(key: int, value: RawUnreadStreamDict, /) -> None` cannot be called with a key of type `Any` and a value of type `dict[str, Any]` on object of type `dict[int, RawUnreadStreamDict]`
+ zerver/lib/message.py:1005:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, RawUnreadDirectMessageDict].__setitem__(key: int, value: RawUnreadDirectMessageDict, /) -> None` cannot be called with a key of type `Any` and a value of type `dict[str, Any]` on object of type `dict[int, RawUnreadDirectMessageDict]`
+ zerver/lib/message.py:1024:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, RawUnreadDirectMessageDict].__setitem__(key: int, value: RawUnreadDirectMessageDict, /) -> None` cannot be called with a key of type `Any` and a value of type `dict[str, int | Unknown]` on object of type `dict[int, RawUnreadDirectMessageDict]`
+ zerver/lib/message.py:1028:17: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, RawUnreadDirectMessageGroupDict].__setitem__(key: int, value: RawUnreadDirectMessageGroupDict, /) -> None` cannot be called with a key of type `Any` and a value of type `dict[str, str]` on object of type `dict[int, RawUnreadDirectMessageGroupDict]`
+ zerver/lib/message.py:1157:5: error[invalid-assignment] Object of type `dict[str, list[UnreadDirectMessageInfo] | list[UnreadStreamInfo] | list[UnreadDirectMessageGroupInfo] | list[int] | int]` is not assignable to `UnreadMessagesResult`
+ zerver/lib/push_notifications.py:1973:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, PushDeviceInfoDict]`, found `dict[str | Unknown, dict[Unknown | str, Unknown] | Unknown]`
+ zerver/lib/subscription_info.py:807:5: error[invalid-assignment] Object of type `dict[Any, dict[str, Any] | Unknown]` is not assignable to `dict[int, RawStreamDict]`
+ zerver/lib/subscription_info.py:830:5: error[invalid-assignment] Object of type `QuerySet[Subscription, dict[str, Any]]` is not assignable to `Iterable[RawSubscriptionDict]`
+ zerver/lib/user_groups.py:708:9: error[invalid-assignment] Object of type `dict[str, Unknown | int | None | list[int] | UserGroupMembersDict]` is not assignable to `UserGroupDict`
+ zerver/lib/users.py:495:12: error[invalid-return-type] Return type does not match returned value: expected `list[Account]`, found `list[dict[str, Unknown | str | None] | Unknown]`
+ zerver/models/realm_emoji.py:88:9: error[invalid-assignment] Object of type `dict[str, str | Unknown | None]` is not assignable to `EmojiInfo`
+ zerver/tests/test_channel_creation.py:98:13: error[invalid-argument-type] Argument to function `create_streams_if_needed` is incorrect: Expected `list[StreamDict]`, found `list[dict[Unknown | str, Unknown | int] | Unknown]`
+ zerver/tests/test_channel_creation.py:128:13: error[invalid-argument-type] Argument to function `create_streams_if_needed` is incorrect: Expected `list[StreamDict]`, found `list[dict[Unknown | str, Unknown | bool] | Unknown]`
+ zerver/tests/test_import_export.py:3063:23: error[invalid-argument-type] Argument to function `do_update_user_custom_profile_data_if_changed` is incorrect: Expected `list[ProfileDataElementUpdateDict]`, found `list[Unknown | dict[str, Unknown | str]]`
+ zerver/tests/test_message_dict.py:359:9: error[invalid-assignment] Object of type `list[Unknown | dict[str, str | int]]` is not assignable to `list[UserDisplayRecipient]`
+ zerver/tests/test_subs.py:422:9: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown | bool] | Unknown]` is not assignable to `list[StreamDict]`
+ zerver/tests/test_user_status.py:23:12: error[invalid-return-type] Return type does not match returned value: expected `UserInfoDict`, found `UserInfoDict | None`
+ zerver/tornado/event_queue.py:1042:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ClientInfo].__setitem__(key: str, value: ClientInfo, /) -> None` cannot be called with a key of type `Unknown | str` and a value of type `dict[str, ClientDescriptor | list[Unknown] | bool]` on object of type `dict[str, ClientInfo]`
+ zerver/tornado/event_queue.py:1053:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, ClientInfo].__setitem__(key: str, value: ClientInfo, /) -> None` cannot be called with a key of type `Unknown | str` and a value of type `dict[str, ClientDescriptor | Any | bool]` on object of type `dict[str, ClientInfo]`
+ zerver/views/streams.py:595:5: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown] | Unknown]` is not assignable to `list[StreamDict]`
+ zilencer/views.py:1733:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, RemoteRealmDictValue].__setitem__(key: str, value: RemoteRealmDictValue, /) -> None` cannot be called with a key of type `str` and a value of type `dict[Unknown | str, Unknown | int | None]` on object of type `dict[str, RemoteRealmDictValue]`
+ zproject/backends.py:1782:16: error[invalid-return-type] Return type does not match returned value: expected `list[ExternalAuthMethodDictT]`, found `list[ExternalAuthMethodDictT | dict[str, Unknown | str]]`
+ zproject/backends.py:2506:16: error[invalid-return-type] Return type does not match returned value: expected `list[ExternalAuthMethodDictT]`, found `list[ExternalAuthMethodDictT | dict[str, Unknown | str]]`
+ zproject/backends.py:3441:13: error[invalid-assignment] Object of type `dict[str, str | Any]` is not assignable to `ExternalAuthMethodDictT`
+ zproject/backends.py:3536:16: error[invalid-return-type] Return type does not match returned value: expected `list[ExternalAuthMethodDictT]`, found `list[ExternalAuthMethodDictT | dict[str, str | Unknown]]`
- Found 3339 diagnostics
+ Found 3367 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/api/rest.py:4039:30: error[invalid-assignment] Object of type `Unknown | list[tuple[int, HistoryBaseEntry[Unknown]]] | list[HistoryBaseEntry[Unknown]]` is not assignable to `list[HistoryBaseEntry[Unknown]]`
+ rotkehlchen/api/rest.py:4039:30: error[invalid-assignment] Object of type `zip[Unknown] | tuple[list[Unknown | None], list[tuple[int, HistoryBaseEntry[Unknown]]] | list[HistoryBaseEntry[Unknown]]]` is not assignable to `list[HistoryBaseEntry[Unknown]]`
+ rotkehlchen/balances/historical.py:73:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[Asset, HistoricalBalance].__setitem__(key: Asset, value: HistoricalBalance, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown | str, Unknown | FVal]` on object of type `dict[Asset, HistoricalBalance]`
+ rotkehlchen/chain/evm/decoding/gitcoinv2/constants.py:27:66: error[invalid-argument-type] Invalid argument to key "inputs" with declared type `Sequence[ABIComponentIndexed]` on TypedDict `ABIEvent`: value of type `list[Unknown | dict[Unknown | str, Unknown | bool | str]]`
+ rotkehlchen/chain/evm/decoding/gitcoinv2/constants.py:29:71: error[invalid-argument-type] Invalid argument to key "inputs" with declared type `Sequence[ABIComponentIndexed]` on TypedDict `ABIEvent`: value of type `list[Unknown | dict[Unknown | str, Unknown | bool | str] | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | str]] | bool | str]]`
+ rotkehlchen/chain/evm/decoding/thegraph/constants.py:15:1: error[invalid-assignment] Object of type `list[Unknown | dict[Unknown | str, Unknown | bool | list[Unknown | dict[Unknown | str, Unknown | bool | str]] | str]]` is not assignable to `Sequence[ABIEvent]`
+ rotkehlchen/db/upgrades/v45_v46.py:121:25: error[invalid-argument-type] Argument to function `maybe_set_transaction_extra_data` is incorrect: Expected `AssetMovementExtraData | None`, found `dict[Unknown | str, Unknown] | None`
+ rotkehlchen/exchanges/coinbase.py:1063:21: error[invalid-argument-type] Argument to function `maybe_set_transaction_extra_data` is incorrect: Expected `AssetMovementExtraData | None`, found `dict[Unknown | str, Unknown] | None`
- rotkehlchen/tests/fixtures/rotkehlchen.py:124:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1683 diagnostics
+ Found 1688 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/core/evalf.py:1410:5: error[invalid-assignment] Object of type `dict[type[Expr], ((Expr, int, dict[str, Any], /) -> Any) | (def evalf_float(expr: Float, prec: int, options: dict[str, Any]) -> Any) | (def evalf_rational(expr: Rational, prec: int, options: dict[str, Any]) -> Any) | ... omitted 16 union elements]` is not assignable to `dict[type[Expr], (Expr, int, dict[str, Any], /) -> Any]`
+ sympy/core/evalf.py:1410:5: error[invalid-assignment] Object of type `dict[Unknown | <class 'Symbol'> | <class 'Dummy'> | ... omitted 31 union elements, Unknown | ((x: Expr, prec: int, options: dict[str, Any]) -> Any) | ((expr: Float, prec: int, options: dict[str, Any]) -> Any) | ... omitted 17 union elements]` is not assignable to `dict[type[Expr], (Expr, int, dict[str, Any], /) -> Any]`

core (https://github.com/home-assistant/core)
+ homeassistant/components/accuweather/weather.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `list[Forecast] | None`, found `list[dict[Unknown | str, Unknown | str | None] | Unknown]`
+ homeassistant/components/accuweather/weather.py:220:16: error[invalid-return-type] Return type does not match returned value: expected `list[Forecast] | None`, found `list[dict[Unknown | str, Unknown | str | None] | Unknown]`
+ homeassistant/components/alexa/state_report.py:341:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventStateChangedData]`
+ homeassistant/components/alexa/state_report.py:342:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `def _async_entity_state_listener(event_: Event[EventStateChangedData]) -> CoroutineType[Any, Any, None]`
+ homeassistant/components/alexa/state_report.py:343:9: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `((Mapping[str, Any], /) -> bool) | None`, found `def _async_entity_state_filter(data: EventStateChangedData) -> bool`
+ homeassistant/components/anthropic/config_flow.py:129:21: error[invalid-argument-type] Argument to bound method `async_create_entry` is incorrect: Expected `Iterable[ConfigSubentryData] | None`, found `list[Unknown | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | bool | list[Unknown | str] | str] | None]]`
+ homeassistant/components/apache_kafka/__init__.py:133:37: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventStateChangedData]`
+ homeassistant/components/apache_kafka/__init__.py:133:58: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `bound method Self@start.write(event: Event[EventStateChangedData]) -> CoroutineType[Any, Any, None]`
+ homeassistant/components/backup/manager.py:1610:16: error[invalid-return-type] Return type does not match returned value: expected `StoredKnownBackup`, found `dict[Unknown | str, Unknown | str | list[dict[Unknown | str, Unknown | str | None] | Unknown] | list[str]]`
+ homeassistant/components/backup/manager.py:1612:30: error[invalid-argument-type] Invalid argument to key "failed_addons" with declared type `list[StoredAddonInfo]` on TypedDict `StoredKnownBackup`: value of type `list[dict[Unknown | str, Unknown | str | None] | Unknown]`
+ homeassistant/components/cloud/alexa_config.py:272:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventEntityRegistryUpdatedData]`
+ homeassistant/components/cloud/alexa_config.py:273:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `bound method Self@async_initialize._handle_entity_registry_updated(event: Event[_EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update]) -> CoroutineType[Any, Any, None]`
+ homeassistant/components/cloud/google_config.py:269:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventEntityRegistryUpdatedData]`
+ homeassistant/components/cloud/google_config.py:270:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `bound method Self@async_initialize._handle_entity_registry_updated(event: Event[_EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update]) -> None`
+ homeassistant/components/cloud/google_config.py:275:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventDeviceRegistryUpdatedData]`
+ homeassistant/components/cloud/google_config.py:276:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `bound method Self@async_initialize._handle_device_registry_updated(event: Event[_EventDeviceRegistryUpdatedData_Create | _EventDeviceRegistryUpdatedData_Remove | _EventDeviceRegistryUpdatedData_Update]) -> None`
+ homeassistant/components/conversation/default_agent.py:287:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventAreaRegistryUpdatedData]`
+ homeassistant/components/conversation/default_agent.py:291:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventFloorRegistryUpdatedData]`
+ homeassistant/components/conversation/default_agent.py:295:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventEntityRegistryUpdatedData]`
+ homeassistant/components/conversation/default_agent.py:297:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `((Mapping[str, Any], /) -> bool) | None`, found `bound method Self@_listen_clear_slot_list._filter_entity_registry_changes(event_data: _EventEntityRegistryUpdatedData_CreateRemove | _EventEntityRegistryUpdatedData_Update) -> bool`
+ homeassistant/components/conversation/default_agent.py:300:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventStateChangedData]`
+ homeassistant/components/conversation/default_agent.py:302:17: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `((Mapping[str, Any], /) -> bool) | None`, found `bound method Self@_listen_clear_slot_list._filter_state_changes(event_data: EventStateChangedData) -> bool`
+ homeassistant/components/datadog/__init__.py:124:31: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventStateChangedData]`
+ homeassistant/components/device_tracker/config_entry.py:166:27: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventDeviceRegistryUpdatedData]`
+ homeassistant/components/device_tracker/config_entry.py:166:61: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `def handle_device_event(ev: Event[EventDeviceRegistryUpdatedData]) -> None`
+ homeassistant/components/ekeybionyx/config_flow.py:110:13: error[invalid-assignment] Object of type `list[dict[Unknown | str, Unknown] | Unknown]` is not assignable to `list[SelectOptionDict]`
+ homeassistant/components/esphome/encryption_key_storage.py:49:13: error[invalid-assignment] Object of type `(Unknown & ~AlwaysFalsy) | (EncryptionKeyData & ~AlwaysFalsy) | dict[Unknown | str, Unknown | dict[Unknown, Unknown]]` is not assignable to attribute `_data` of type `EncryptionKeyData | None`
- homeassistant/components/evohome/storage.py:78:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ homeassistant/components/google_assistant/report_state.py:190:13: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `EventType[Mapping[str, Any]] | str`, found `EventType[EventStateChangedData]`
+ homeassistant/components/google_assistant/report_state.py:191:13: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `(Event[Mapping[str, Any]], /) -> Coroutine[Any, Any, None] | None`, found `def _async_entity_state_listener(event: Event[EventStateChangedData]) -> CoroutineType[Any, Any, None]`
+ homeassistant/components/google_assistant/report_state.py:192:13: error[invalid-argument-type] Argument to bound method `async_listen` is incorrect: Expected `((Mapping[str, Any], /) -> bool) | None`, found `def _async_entity_state_filter(data: EventStateChangedData) -> bool`
+ homeassistant/components/google_generative_ai_conversation/config_flow.py:132:21: error[invalid-argument-type] Argument to bound method `async_create_entry` is incorr...*[Comment body truncated]*

---

_Comment by @ibraheemdev on 2025-11-03 02:56_

I'm not sure if we want to be emitting `invalid-assignment` errors if we have already emitted a `invalid-key` error. Maybe we should special case `TypedDict` annotated assignments to avoid this? Note that we *do* want to infer a `dict` for the RHS in that case (see https://github.com/astral-sh/ruff/pull/21169), but the diagnostics are effectively being duplicated.

---

_Comment by @codspeed-hq[bot] on 2025-11-03 04:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ftyped-dict-assignability?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21238 will **degrade performances by 17.57%**

<sub>Comparing <code>ibraheem/typed-dict-assignability</code> (2aa777e) with <code>main</code> (21ec8aa)</sub>



### Summary

`❌ 1` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ftyped-dict-assignability?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ftyped-dict-assignability?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 11.2 s | 13.6 s | -17.57% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Ftyped-dict-assignability?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_@sharkdp reviewed on 2025-11-03 14:52_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/comprehensions/basic.md`:167 on 2025-11-03 14:52_

Yes, absolutely. I was aware about this, but should have added a comment. But this is why I added the other test below (where it's supposed to be an error).

---

_@sharkdp reviewed on 2025-11-03 14:54_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/comprehensions/basic.md`:167 on 2025-11-03 14:54_

See also this comment: https://github.com/astral-sh/ruff/pull/20962#discussion_r2482437556

---

_Comment by @sharkdp on 2025-11-03 15:01_

> I'm not sure if we want to be emitting `invalid-assignment` errors if we have already emitted a `invalid-key` error. Maybe we should special case `TypedDict` annotated assignments to avoid this? Note that we _do_ want to infer a `dict` for the RHS in that case (see #21169), but the diagnostics are effectively being duplicated.

I was about to add a comment to this PR that we should maybe try to avoid emitting the additional `invalid-assignment` diagnostic, and only then saw your comment. I think it would be great if we could attempt to do this, if it's not too complicated.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:1983 on 2025-11-03 15:06_

I guess intersections (of a `TypedDict` and something else), typevars, and `Never` would be types that can subtype `TypedDict`, but they were all handled above...

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:208 on 2025-11-03 15:07_

I'm confused. Where does `invalid-argument-type` come from?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:6208 on 2025-11-03 15:09_

I know you're not the one to invent/introduce this, but I think I would prefer expanding `elt`/`elts` to `element`/`elements`

---

_@sharkdp approved on 2025-11-03 15:11_

Nice, thank you!

---

_@ibraheemdev reviewed on 2025-11-03 15:36_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:208 on 2025-11-03 15:36_

We emit `invalid-argument-type` when a value is not assignable to its declared key type, but this should be removed now.

---

_Comment by @ibraheemdev on 2025-11-03 15:52_

I added a special case to avoid the duplicated diagnostics for simple annotated assignments, but attribute assignment and function arguments are trickier, because we may hide typed-dict diagnostics during multi-inference and rely on the assignability check to fail.

There is one correct new typing comformance diagnostic, but also two regressions because we do not correctly handle `extra_items` (and were hiding diagnostics by treating a `dict` as a `TypedDict`).

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-03 15:53_

---

_Comment by @github-actions[bot] on 2025-11-03 15:59_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 167 | 0 | 0 |
| `invalid-assignment` | 82 | 0 | 32 |
| `invalid-return-type` | 38 | 0 | 0 |
| `unused-ignore-comment` | 1 | 33 | 0 |
| `non-subscriptable` | 0 | 11 | 0 |
| `possibly-missing-attribute` | 4 | 0 | 0 |
| `invalid-parameter-default` | 2 | 0 | 0 |
| `no-matching-overload` | 1 | 1 | 0 |
| `possibly-missing-implicit-call` | 0 | 1 | 0 |
| `unresolved-attribute` | 0 | 1 | 0 |
| `unsupported-operator` | 1 | 0 | 0 |
| **Total** | **296** | **47** | **32** |

**[Full report with detailed diff](https://ibraheem-typed-dict-assignab.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-typed-dict-assignab.ecosystem-663.pages.dev/timing))


---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-11-03 17:34_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-03 17:34_

---

_Comment by @ibraheemdev on 2025-11-03 21:57_

Most of the diagnostics seem to be duplicated diagnostics (in the not-so-simple special case). I opened https://github.com/astral-sh/ty/issues/1472 to track that issue, but I think landing this is high priority because of how much bidirectional-inference relies on the fallback untyped `dict`.

---

_Merged by @ibraheemdev on 2025-11-03 21:57_

---

_Closed by @ibraheemdev on 2025-11-03 21:57_

---

_Branch deleted on 2025-11-03 21:57_

---
