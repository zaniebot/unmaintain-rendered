```yaml
number: 21296
title: "[ty] Add support for `Literal`s in implicit type aliases"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/implicit-type-aliases-literal
created_at: 2025-11-06T15:38:26Z
updated_at: 2025-11-07T16:48:19Z
url: https://github.com/astral-sh/ruff/pull/21296
synced_at: 2026-01-12T15:57:20Z
```

# [ty] Add support for `Literal`s in implicit type aliases

---

_@sharkdp_

## Summary

Add support for `Literal` types in implicit type aliases.

part of https://github.com/astral-sh/ty/issues/221

## Ecosystem analysis

This looks good to me, true positives and known problems.

## Test Plan

New Markdown tests.


---

_Label `ty` added by @sharkdp on 2025-11-06 15:38_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-06 15:38_

---

_Comment by @github-actions[bot] on 2025-11-06 15:41_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-07 16:35:19.406661502 +0000
+++ new-output.txt	2025-11-07 16:35:22.836690933 +0000
@@ -16,7 +16,6 @@
 aliases_explicit.py:59:5: error[type-assertion-failure] Argument does not have asserted type `int | str | None | list[list[int]]`
 aliases_explicit.py:60:5: error[type-assertion-failure] Argument does not have asserted type `(...) -> None`
 aliases_explicit.py:61:5: error[type-assertion-failure] Argument does not have asserted type `int | str`
-aliases_explicit.py:63:5: error[type-assertion-failure] Argument does not have asserted type `Literal[3, 4, 5] | None`
 aliases_explicit.py:98:1: error[type-assertion-failure] Argument does not have asserted type `list[str]`
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
@@ -1002,5 +1001,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1004 diagnostics
+Found 1003 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-11-06 15:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pip (https://github.com/pypa/pip)
+ src/pip/_vendor/distlib/util.py:1637:47: error[invalid-argument-type] Argument to function `reader` is incorrect: Expected `Literal[0, 1, 2, 3]`, found `Unknown | str`
+ src/pip/_vendor/distlib/util.py:1657:47: error[invalid-argument-type] Argument to function `writer` is incorrect: Expected `Literal[0, 1, 2, 3]`, found `Unknown | str`
+ src/pip/_vendor/rich/console.py:1540:43: warning[redundant-cast] Value is already of type `Literal["left", "center", "right"]`
- Found 582 diagnostics
+ Found 585 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:564:43: error[invalid-argument-type] Argument to bound method `_check_scope` is incorrect: Expected `Scope`, found `Scope | (@Todo & ~str) | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str)`
+ src/_pytest/fixtures.py:564:43: error[invalid-argument-type] Argument to bound method `_check_scope` is incorrect: Expected `Scope`, found `Scope | (((str, Config, /) -> Literal["session", "package", "module", "class", "function"]) & ~(() -> object) & ~str)`
- src/_pytest/fixtures.py:1032:16: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Scope | (@Todo & ~str) | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str)`
+ src/_pytest/fixtures.py:1010:17: error[invalid-argument-type] Argument to bound method `from_user` is incorrect: Expected `Literal["session", "package", "module", "class", "function"]`, found `Literal["session", "package", "module", "class", "function"] | (((str, Config, /) -> Literal["session", "package", "module", "class", "function"]) & str & ~(() -> object))`
+ src/_pytest/fixtures.py:1032:16: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Scope | (((str, Config, /) -> Literal["session", "package", "module", "class", "function"]) & ~(() -> object) & ~str)`
- src/_pytest/fixtures.py:1691:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (@Todo & ~str) | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str)`
+ src/_pytest/fixtures.py:1691:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (((str, Config, /) -> Literal["session", "package", "module", "class", "function"]) & ~(() -> object) & ~str)`
+ src/_pytest/mark/structures.py:520:13: error[invalid-parameter-default] Default value of type `EllipsisType` is not assignable to annotated parameter type `Literal["session", "package", "module", "class", "function"] | None`
- src/_pytest/python.py:1569:20: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `Scope | Unknown | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/_pytest/python.py:1569:20: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `Scope | (((str, Config, /) -> str) & ~(() -> object) & ~str) | Unknown` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- testing/python/metafunc.py:137:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/test_scope.py:41:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

beartype (https://github.com/beartype/beartype)
- beartype/_data/typing/datatyping.py:126:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 498 diagnostics
+ Found 497 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/on.py:899:9: error[invalid-assignment] Object of type `frozenset[Unknown | str]` is not assignable to `Collection[Literal["CREATE", "UPDATE", "DELETE", "CONNECT"]] | None`
- Found 85 diagnostics
+ Found 86 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/console.py:1540:43: warning[redundant-cast] Value is already of type `Literal["left", "center", "right"]`
+ tests/test_align.py:18:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Literal["left", "center", "right"]`, found `None`
+ tests/test_align.py:20:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Literal["left", "center", "right"]`, found `Literal["middle"]`
+ tests/test_align.py:22:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Literal["left", "center", "right"]`, found `Literal[""]`
+ tests/test_align.py:24:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Literal["left", "center", "right"]`, found `Literal["LEFT"]`
+ tests/test_align.py:26:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Literal["top", "middle", "bottom"] | None`, found `Literal["somewhere"]`
+ tests/test_rule.py:34:29: error[invalid-argument-type] Argument to bound method `rule` is incorrect: Expected `Literal["left", "center", "right"]`, found `Literal["foo"]`
- Found 316 diagnostics
+ Found 323 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/json_schema.py:1745:13: error[type-assertion-failure] Argument does not have asserted type `Never`
+ pydantic/v1/schema.py:962:34: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member

poetry (https://github.com/python-poetry/poetry)
- src/poetry/console/commands/build.py:242:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/console/commands/test_build.py:223:84: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 977 diagnostics
+ Found 975 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- tests/pandas/test_decorators.py:850:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1635 diagnostics
+ Found 1634 diagnostics

Expression (https://github.com/cognitedata/Expression)
- expression/collections/block.py:271:16: error[invalid-return-type] Return type does not match returned value: expected `_TSourceSum@sum | Literal[0]`, found `int`
- expression/collections/block.py:934:12: error[invalid-return-type] Return type does not match returned value: expected `_TSourceSum@sum | Literal[0]`, found `int`
+ expression/collections/block.py:940:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 201 diagnostics
+ Found 200 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ tests/arti/types/test_python_adapters.py:91:52: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
+ tests/arti/types/test_python_adapters.py:91:57: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
+ tests/arti/types/test_python_adapters.py:103:59: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
+ tests/arti/types/test_python_adapters.py:105:40: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
- Found 170 diagnostics
+ Found 174 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/addons/next_layer.py:338:17: error[type-assertion-failure] Argument does not have asserted type `Never`
- mitmproxy/net/http/http1/read.py:126:17: error[type-assertion-failure] Argument does not have asserted type `Never`
- mitmproxy/net/http/validate.py:131:17: error[type-assertion-failure] Argument does not have asserted type `Never`
+ mitmproxy/types.py:128:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ test/mitmproxy/addons/test_next_layer.py:444:5: error[invalid-assignment] Object of type `None` is not assignable to `Literal["SSLv3", "TLSv1", "TLSv1.1", "TLSv1.2", "TLSv1.3", ... omitted 4 literals]`
- Found 1890 diagnostics
+ Found 1889 diagnostics

vision (https://github.com/pytorch/vision)
- references/detection/engine.py:38:22: error[unresolved-attribute] Object of type `int` has no attribute `item`
- references/detection/engine.py:51:13: error[unresolved-attribute] Object of type `int` has no attribute `backward`
- test/test_backbone_utils.py:184:9: warning[possibly-missing-attribute] Attribute `backward` may be missing on object of type `int | Unknown`
+ test/test_backbone_utils.py:184:9: warning[possibly-missing-attribute] Attribute `backward` may be missing on object of type `Literal[0] | Unknown`
- test/test_backbone_utils.py:225:9: warning[possibly-missing-attribute] Attribute `backward` may be missing on object of type `int | Unknown`
+ test/test_backbone_utils.py:225:9: warning[possibly-missing-attribute] Attribute `backward` may be missing on object of type `Literal[0] | Unknown`
+ torchvision/transforms/v2/_container.py:149:19: warning[division-by-zero] Cannot divide object of type `int` by zero
+ torchvision/transforms/v2/_container.py:149:19: warning[division-by-zero] Cannot divide object of type `float` by zero

mypy (https://github.com/python/mypy)
+ mypy/typeshed/stdlib/_typeshed/__init__.pyi:223:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/_typeshed/__init__.pyi:252:29: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/array.pyi:14:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/builtins.pyi:249:19: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/math.pyi:98:19: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/sqlite3/__init__.pyi:224:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `None`
+ mypy/typeshed/stdlib/sys/__init__.pyi:306:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `None`
+ mypy/typeshed/stdlib/xml/sax/expatreader.pyi:12:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `<class 'bool'>`
+ mypyc/irbuild/util.py:35:1: error[invalid-assignment] Object of type `frozenset[Unknown | str]` is not assignable to `frozenset[Literal["native_class", "allow_interpreted_subclasses", "serializable", "free_list_len"]]`
- Found 1830 diagnostics
+ Found 1839 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/state.py:1991:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 509 diagnostics
+ Found 508 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/util/typing.py:553:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 515 diagnostics
+ Found 514 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/exchange/exchange.py:2133:16: error[invalid-return-type] Return type does not match returned value: expected `Literal["bid", "ask"]`, found `Unknown | str`
- freqtrade/strategy/parameters.py:287:23: error[unsupported-operator] Operator `*` is unsupported between objects of type `object` and `int`
- freqtrade/strategy/parameters.py:288:24: error[unsupported-operator] Operator `*` is unsupported between objects of type `object` and `int`
- Found 609 diagnostics
+ Found 608 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreter/interpreter.py:3447:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: @Todo | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: @Todo | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | @Todo | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["dependencies"]` and a value of type `list[Unknown]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3447:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["rust", "c"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["rust", "c"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | @Todo | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["dependencies"]` and a value of type `list[Unknown]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- mesonbuild/interpreter/interpreter.py:3479:13: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: @Todo | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: @Todo | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | @Todo | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["d_import_dirs"]` and a value of type `list[IncludeDirs]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3479:13: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["rust", "c"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["rust", "c"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | @Todo | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["d_import_dirs"]` and a value of type `list[IncludeDirs]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- mesonbuild/interpreter/interpreter.py:3516:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: @Todo | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: @Todo | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["vala_header"], value: str | None, /) -> None, (key: Literal["vala_vapi"], value: str | None, /) -> None, (key: Literal["vala_gir"], value: str | None, /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_crate_type"], value: Literal["bin", "lib", "rlib", "dylib", "cdylib", "staticlib", "proc-macro"] | None, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | @Todo], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: ...*[Comment body truncated]*

---

_Comment by @github-actions[bot] on 2025-11-06 15:44_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 98 | 2 | 13 |
| `unused-ignore-comment` | 2 | 72 | 0 |
| `no-matching-overload` | 28 | 1 | 0 |
| `unresolved-attribute` | 8 | 17 | 0 |
| `invalid-assignment` | 14 | 1 | 4 |
| `unsupported-operator` | 9 | 2 | 2 |
| `invalid-return-type` | 3 | 4 | 4 |
| `type-assertion-failure` | 0 | 11 | 0 |
| `redundant-cast` | 5 | 3 | 0 |
| `invalid-type-form` | 6 | 0 | 0 |
| `possibly-missing-attribute` | 0 | 3 | 3 |
| `possibly-unresolved-reference` | 0 | 3 | 0 |
| `division-by-zero` | 2 | 0 | 0 |
| `invalid-context-manager` | 0 | 0 | 2 |
| `invalid-parameter-default` | 1 | 0 | 0 |
| `not-iterable` | 0 | 0 | 1 |
| `unknown-argument` | 0 | 1 | 0 |
| **Total** | **176** | **120** | **29** |

**[Full report with detailed diff](https://david-implicit-type-aliases-lciu.ecosystem-663.pages.dev/diff)** ([timing results](https://david-implicit-type-aliases-lciu.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-11-06 15:50_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-literal?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21296 will **degrade performances by 5.67%**

<sub>Comparing <code>david/implicit-type-aliases-literal</code> (a614de1) with <code>main</code> (8ba1cfe)</sub>



### Summary

`❌ 1 (👁 1)` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-literal?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 12 s | 12.8 s | -5.67% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Fimplicit-type-aliases-literal?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-06 15:55_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-06 15:55_

---

_Marked ready for review by @sharkdp on 2025-11-06 20:38_

---

_Review requested from @carljm by @sharkdp on 2025-11-06 20:38_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-06 20:38_

---

_Review requested from @dcreager by @sharkdp on 2025-11-06 20:38_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-07 14:28_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-07 14:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:316 on 2025-11-07 14:56_

This case doesn't involve an implicit type alias, and it is testing a behavior which is unchanged by this PR, so it's not clear why it appears in this PR or in this test file?

The new version of this diagnostic added in this PR would I think only trigger if `Literal[int]` appeared in a value expression, but here it's in an annotation.

---

_@sharkdp reviewed on 2025-11-07 14:59_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:9656 on 2025-11-07 14:59_

I think the `TypeList` type is useful, but `Literal` should probably use a simpler wrapper that only contains a single type. `Optional` and `Annotated` also just need a single element. I will change this in a follow-up PR.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:320 on 2025-11-07 15:06_

I have two thoughts about this error message.

1. It's a little weird to talk about "`typing.Literal` instances", because at runtime and in typeshed, `typing.Literal` is not a class and there are no instances of it. It's a special form, and indexing it returns an instance of `typing._LiteralGenericAlias`. I think maybe a term that would hew a bit closer to runtime, and align more with the user's likely intuition that `T = Literal[1]` defines a type alias, would be to call them "`typing.Literal` aliases", maybe?

2) The error message doesn't quite match what I see happening in the code? What you are describing as a "`typing.Literal` instance" (and I suggest calling a "`typing.Literal` alias") is `IntLiteral1` -- and that most certainly is allowed in a type expression! What isn't allowed is to subscript it again. So the error I'd expect to see here would be more like "`typing.Literal` aliases cannot be subscripted." (For reference, pyright gives the somewhat obscure "Expected no type arguments for class int", where I think "class int" results from an implicit promotion from the literal type, and mypy gives "Bad number of arguments for type alias, expected 0, got 1"). Both of these indicate that the mistake is not `IntLiteral1` appearing in a type expression, it's the attempt to subscript it (or pass it type arguments).

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8959 on 2025-11-07 15:49_

I'm not wild about the fact that -- depending on context -- this type can sometimes hold a list of value-expression types (if it's the wrapped data inside a `KnownInstance::UnionType` variant) and sometimes hold a list of type-expression types (if it's the wrapped data inside a `KnownInstance::Literal` variant). That feels a little bug-prone? E.g. it would be incorrect to call the `.to_union()` method on a `TypeList` wrapped inside a `KnownInstance::Union` variant, because you need to first convert them all to type expressions. But it would be incorrect to convert them all to type expressions on the elements in a `TypeList` inside a `KnownInstance::Literal` variant, because those have all already been interpreted as type expressions eagerly when building the type list.

I think I'd prefer to have different structs for the wrapped data inside the two variants, since they have pretty different semantics.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:821 on 2025-11-07 15:51_

If I understand correctly, we only reach this branch for cases like these:

```py
from typing import Literal

X = Literal[1, 2]

def f(a: X[int], b: Literal["foo"][str]):
    ...
```

In which case, I think I'd prefer something a bit closer to the runtime error message, which feels clearer to me:

```pytb
>>> from typing import Literal
>>> Literal[2][int]
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    Literal[2][int]
    ~~~~~~~~~~^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 432, in inner
    return func(*args, **kwds)
  File "/Users/alexw/.pyenv/versions/3.13.1/lib/python3.13/typing.py", line 1473, in __getitem__
    raise TypeError(f"{self} is not a generic class")
TypeError: typing.Literal[2] is not a generic class
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:821 on 2025-11-07 15:51_

Discussed above -- doesn't seem like the right error message for this case

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:9656 on 2025-11-07 15:54_

Because in a case like `Literal[1, 2, 3]`, it's still just a "single" union type?

---

_@carljm approved on 2025-11-07 15:56_

Looks good to me, modulo a couple things about diagnostics. Thank you!

---

_@AlexWaygood approved on 2025-11-07 15:59_

Nice! It could possibly also be worth adding tests that demonstrate that this also now works:

```py
from typing import Union, Literal

X = Literal[2, 3]

def f(x: Union[X, str]):
    reveal_type(x)  # revealed: Literal[2, 3] | str
```

---

_@sharkdp reviewed on 2025-11-07 16:33_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:320 on 2025-11-07 16:33_

Yes, thank you. I changed it and the error message now reads:

> [invalid-type-form] "`Literal[26]` is not a generic class"

---

_@sharkdp reviewed on 2025-11-07 16:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:821 on 2025-11-07 16:34_

See above, the error message now reads:

> [invalid-type-form] "`Literal[26]` is not a generic class"

---

_@sharkdp reviewed on 2025-11-07 16:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:8959 on 2025-11-07 16:34_

I agree. I will change this in major ways in my next PR, and will keep this in mind.

---

_Merged by @sharkdp on 2025-11-07 16:46_

---

_Closed by @sharkdp on 2025-11-07 16:46_

---

_Branch deleted on 2025-11-07 16:46_

---

_@sharkdp reviewed on 2025-11-07 16:48_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:822 on 2025-11-07 16:48_

This looks very strange, but will turn into `ty.to_inner(…).display(…)` soon.

---
