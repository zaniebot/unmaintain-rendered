```yaml
number: 20602
title: "[ty] Introduce `TypeRelation::Redundancy`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/third-type-relation
created_at: 2025-09-27T15:16:06Z
updated_at: 2025-10-06T11:12:48Z
url: https://github.com/astral-sh/ruff/pull/20602
synced_at: 2026-01-12T15:57:05Z
```

# [ty] Introduce `TypeRelation::Redundancy`

---

_@AlexWaygood_

## Summary

The union `T | U` can be validly simplified to `U` iff:
1. `T` is a subtype of `U` OR
2. `T` is equivalent to `U` OR
3. `U` is a union and contains a type that is equivalent to `T` OR
4. `T` is an intersection and contains a type that is equivalent to `U`

(In practice, the only situation in which 2, 3 or 4 would be true when (1) was not true would be if `T` or `U` is a dynamic type.)

Currently we achieve these simplifications in the union builder by doing something along the lines of `t.is_subtype_of(db, u) || t.is_equivalent_to_(db, u) || t.into_intersection().is_some_and(|intersection| intersection.positive(db).contains(&u)) || u.into_union().is_some_and(|union| union.elements(db).contains(&t))`. But this is both slow and misses some cases (it doesn't simplify the union `Any | (Unknown & ~None)` to `Any`, for example). We can improve the consistency and performance of our union simplifications by adding a third type relation that sits in between `TypeRelation::Subtyping` and `TypeRelation::Assignability`: `TypeRelation::UnionSimplification`.

This change leads to simpler, more user-friendly types due to the more consistent simplification. It also lead to a pretty huge performance improvement!

## Test Plan

Existing tests, plus some new ones.


---

_Label `ty` added by @AlexWaygood on 2025-09-27 15:16_

---

_Comment by @github-actions[bot] on 2025-09-27 15:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-27 15:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paasta (https://github.com/yelp/paasta)
- paasta_tools/setup_kubernetes_job.py:313:29: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, HpaOverride].__setitem__(key: str, value: HpaOverride, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown | str, Unknown | (Any & ~AlwaysFalsy)]` on object of type `dict[str, HpaOverride]`
+ paasta_tools/setup_kubernetes_job.py:313:29: error[invalid-assignment] Method `__setitem__` of type `bound method dict[str, HpaOverride].__setitem__(key: str, value: HpaOverride, /) -> None` cannot be called with a key of type `Unknown` and a value of type `dict[Unknown | str, Unknown]` on object of type `dict[str, HpaOverride]`

alerta (https://github.com/alerta/alerta)
- alerta/auth/oidc.py:177:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `(Any & ~AlwaysFalsy) | Unknown | None`
+ alerta/auth/oidc.py:177:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
- alerta/auth/oidc.py:177:70: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `(Any & ~AlwaysFalsy) | Unknown | None`
+ alerta/auth/oidc.py:177:70: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
- alerta/auth/oidc.py:203:43: error[invalid-argument-type] Argument to function `create_token` is incorrect: Expected `str`, found `(Any & ~AlwaysFalsy) | Unknown | None`
+ alerta/auth/oidc.py:203:43: error[invalid-argument-type] Argument to function `create_token` is incorrect: Expected `str`, found `Unknown | None`
- alerta/auth/oidc.py:205:26: error[invalid-argument-type] Argument to function `create_token` is incorrect: Expected `str`, found `(Any & ~AlwaysFalsy) | Unknown | None`
+ alerta/auth/oidc.py:205:26: error[invalid-argument-type] Argument to function `create_token` is incorrect: Expected `str`, found `Unknown | None`
- alerta/auth/oidc.py:205:70: error[invalid-argument-type] Argument to function `create_token` is incorrect: Expected `str`, found `(Any & ~AlwaysFalsy) | Unknown | None`
+ alerta/auth/oidc.py:205:70: error[invalid-argument-type] Argument to function `create_token` is incorrect: Expected `str`, found `Unknown | None`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:1667:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (@Todo & ~None & ~(() -> object) & ~str) | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str) | (@Todo & ~str)`
+ src/_pytest/fixtures.py:1667:24: error[invalid-return-type] Return type does not match returned value: expected `Scope`, found `Scope | (@Todo & ~str) | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str)`
- testing/test_assertrewrite.py:140:28: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `tuple[Literal[3], Literal[0]]` with `Unknown | tuple[Unknown | int, Unknown | int] | tuple[Unknown | int | None, Unknown | int | None]`
+ testing/test_assertrewrite.py:140:28: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `tuple[Literal[3], Literal[0]]` with `Unknown | tuple[Unknown | int | None, Unknown | int | None]`
- testing/test_assertrewrite.py:140:38: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | tuple[Unknown | int, Unknown | int] | tuple[Unknown | int | None, Unknown | int | None]` with `tuple[Literal[6], Literal[3]]`
+ testing/test_assertrewrite.py:140:38: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | tuple[Unknown | int | None, Unknown | int | None]` with `tuple[Literal[6], Literal[3]]`

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/generation/coverage.py:502:52: error[invalid-argument-type] Argument to function `_negative_enum` is incorrect: Expected `list[Unknown]`, found `Unknown | dict[Unknown, Unknown]`
+ src/schemathesis/generation/coverage.py:507:52: error[invalid-argument-type] Argument to function `_negative_type` is incorrect: Expected `str | list[str]`, found `Unknown | dict[Unknown, Unknown]`
+ src/schemathesis/generation/coverage.py:519:55: error[invalid-argument-type] Argument to function `_negative_pattern` is incorrect: Expected `str`, found `Unknown | dict[Unknown, Unknown]`
+ src/schemathesis/generation/coverage.py:521:62: error[invalid-argument-type] Argument to function `_negative_format` is incorrect: Expected `str`, found `Unknown | dict[Unknown, Unknown]`
+ src/schemathesis/generation/coverage.py:523:28: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | dict[Unknown, Unknown]` and `Literal[1]`
+ src/schemathesis/generation/coverage.py:527:28: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | dict[Unknown, Unknown]` and `Literal[1]`
+ src/schemathesis/generation/coverage.py:535:70: error[invalid-argument-type] Argument to function `_negative_multiple_of` is incorrect: Expected `int | float`, found `Unknown | dict[Unknown, Unknown]`
+ src/schemathesis/generation/coverage.py:538:45: error[unsupported-operator] Operator `<` is not supported for types `int` and `dict[Unknown, Unknown]`, in comparing `Literal[0]` with `Unknown | dict[Unknown, Unknown]`
+ src/schemathesis/generation/coverage.py:538:49: error[unsupported-operator] Operator `<` is not supported for types `dict[Unknown, Unknown]` and `int`, in comparing `Unknown | dict[Unknown, Unknown]` with `Literal[32768]`
+ src/schemathesis/generation/coverage.py:550:55: error[unsupported-operator] Operator `-` is unsupported between objects of type `(Unknown & ~Literal[1]) | dict[Unknown, Unknown]` and `Literal[1]`
+ src/schemathesis/generation/coverage.py:550:55: error[unsupported-operator] Operator `-` is unsupported between objects of type `(Unknown & ~Literal[1]) | dict[Unknown, Unknown]` and `Literal[1]`
+ src/schemathesis/generation/coverage.py:568:45: error[unsupported-operator] Operator `<` is not supported for types `dict[Unknown, Unknown]` and `int`, in comparing `Unknown | dict[Unknown, Unknown]` with `Literal[32768]`
+ src/schemathesis/generation/coverage.py:570:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | dict[Unknown, Unknown]` and `Literal[1]`
+ src/schemathesis/generation/coverage.py:570:51: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | dict[Unknown, Unknown]` and `Literal[1]`
+ src/schemathesis/generation/coverage.py:574:32: error[unsupported-operator] Operator `>` is not supported for types `dict[Unknown, Unknown]` and `int`, in comparing `Unknown | dict[Unknown, Unknown]` with `Literal[100]`
+ src/schemathesis/generation/coverage.py:599:66: error[invalid-argument-type] Argument to function `_negative_required` is incorrect: Expected `list[str]`, found `Unknown | dict[Unknown, Unknown]`
- Found 296 diagnostics
+ Found 312 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/audit_logs.py:550:56: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | (@Todo & ~None) | Literal[3, 4, 1, 5, -1]`
+ discord/audit_logs.py:550:56: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | Literal[3, 4, 1, 5, -1]`
- discord/audit_logs.py:551:55: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | (@Todo & ~None) | Literal[3, 4, 1, 5, -1]`
+ discord/audit_logs.py:551:55: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | Literal[3, 4, 1, 5, -1]`
- discord/ext/commands/cog.py:532:17: error[invalid-assignment] Object of type `list[Unknown | (Any & ~AlwaysFalsy) | (str & ~AlwaysFalsy)]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`
+ discord/ext/commands/cog.py:532:17: error[invalid-assignment] Object of type `list[Unknown | (str & ~AlwaysFalsy)]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`

vision (https://github.com/pytorch/vision)
- references/depth/stereo/transforms.py:580:9: error[invalid-assignment] Object of type `tuple[None | Unknown, ...] | tuple[@Todo, @Todo]` is not assignable to `tuple[@Todo, @Todo]`
+ references/depth/stereo/transforms.py:580:9: error[invalid-assignment] Object of type `tuple[None | Unknown, ...]` is not assignable to `tuple[@Todo, @Todo]`
- references/depth/stereo/transforms.py:601:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[tuple[Unknown, Unknown], tuple[@Todo, @Todo], tuple[@Todo, @Todo]]`, found `tuple[tuple[Unknown, Unknown], tuple[(@Todo & None) | Unknown, (@Todo & None) | Unknown], tuple[(@Todo & None) | Unknown, ...]]`
- test/test_transforms_v2.py:6763:40: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((inpt: Unknown) -> Unknown) | ((inpt: Unknown) -> int) | ((pic, mode=None) -> Unknown) | ((inpt: Unknown, displacement: Unknown, interpolation: InterpolationMode | int = InterpolationMode, fill: @Todo = None) -> Unknown) | ((inpt: Unknown, num_output_channels: int = int) -> Unknown)` may be missing
+ test/test_transforms_v2.py:6763:40: warning[possibly-missing-attribute] Attribute `__name__` on type `Unknown | ((inpt: Unknown) -> Unknown) | ((inpt: Unknown) -> int) | ((pic, mode=None) -> Unknown) | ((inpt: Unknown, displacement: Unknown, interpolation: InterpolationMode | int = InterpolationMode, fill: @Todo = None) -> Unknown)` may be missing
- Found 1490 diagnostics
+ Found 1489 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_server_cursor_base.py:217:16: error[unsupported-operator] Operator `+` is unsupported between objects of type `Template` and `(@Todo & Composable & ~Template & ~bytes) | (@Todo & Composable) | SQL`
+ psycopg/psycopg/_server_cursor_base.py:217:16: error[unsupported-operator] Operator `+` is unsupported between objects of type `Template` and `(@Todo & Composable) | SQL`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreter/interpreter.py:3412:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["darwin_versions"], value: tuple[str, str] | None, /) -> None, (key: Literal["soversion"], value: str | None, /) -> None, (key: Literal["version"], value: str | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["dependencies"]` and a value of type `list[Unknown]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3412:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["dependencies"]` and a value of type `list[Unknown]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- mesonbuild/interpreter/interpreter.py:3431:13: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["darwin_versions"], value: tuple[str, str] | None, /) -> None, (key: Literal["soversion"], value: str | None, /) -> None, (key: Literal["version"], value: str | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["d_import_dirs"]` and a value of type `Unknown` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3431:13: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Any, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[@Todo], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[@Todo], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["d_import_dirs"]` and a value of type `Unknown` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- mesonbuild/interpreterbase/decorators.py:526:45: error[invalid-argument-type] Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `(Unknown & tuple[object, ...]) | tuple[@Todo | ContainerTypeInfo, ...] | (ContainerTypeInfo & tuple[object, ...]) | tuple[(Unknown & ~tuple[object, ...]) | (ContainerTypeInfo & ~tuple[object, ...])]`
+ mesonbuild/interpreterbase/decorators.py:526:45: error[invalid-argument-type] Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `(Unknown & tuple[object, ...]) | tuple[@Todo | ContainerTypeInfo, ...] | (ContainerTypeInfo & tuple[object, ...])`
- mesonbuild/interpreterbase/decorators.py:527:54: error[invalid-argument-type] Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `(Unknown & tuple[object, ...]) | tuple[@Todo | ContainerTypeInfo, ...] | (ContainerTypeInfo & tuple[object, ...]) | tuple[(Unknown & ~tuple[object, ...]) | (ContainerTypeInfo & ~tuple[object, ...])]`
+ mesonbuild/interpreterbase/decorators.py:527:54: error[invalid-argument-type] Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `(Unknown & tuple[object, ...]) | tuple[@Todo | ContainerTypeInfo, ...] | (ContainerTypeInfo & tuple[object, ...])`
- mesonbuild/interpreterbase/decorators.py:546:45: error[invalid-argument-type] Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `(Unknown & tuple[object, ...]) | tuple[@Todo | ContainerTypeInfo, ...] | (ContainerTypeInfo & tuple[object, ...]) | tuple[(Unknown & ~tuple[object, ...]) | (ContainerTypeInfo & ~tuple[object, ...])]`
+ mesonbuild/interpreterbase/decorators.py:546:45: error[invalid-argument-type] Argument to function `check_value_type` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `(Unknown & tuple[object, ...]) | tuple[@Todo | ContainerTypeInfo, ...] | (ContainerTypeInfo & tuple[object, ...])`
- mesonbuild/interpreterbase/decorators.py:546:197: error[invalid-argument-type] Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `(Unknown & tuple[object, ...]) | tuple[@Todo | ContainerTypeInfo, ...] | (ContainerTypeInfo & tuple[object, ...]) | tuple[(Unknown & ~tuple[object, ...]) | (ContainerTypeInfo & ~tuple[object, ...])]`
+ mesonbuild/interpreterbase/decorators.py:546:197: error[invalid-argument-type] Argument to function `types_description` is incorrect: Expected `tuple[type | ContainerTypeInfo, ...]`, found `(Unknown & tuple[object, ...]) | tuple[@Todo | ContainerTypeInfo, ...] | (ContainerTypeInfo & tuple[object, ...])`

xarray (https://github.com/pydata/xarray)
- xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo | None | (@Todo & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo | None | dict[Unknown, Unknown]`
- xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo | None | (@Todo & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo | None | dict[Unknown, Unknown]`
- xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `Literal["coordinates", "all"] | bool | None`, found `@Todo | None | (@Todo & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `Literal["coordinates", "all"] | bool | None`, found `@Todo | None | dict[Unknown, Unknown]`
- xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo | None | (@Todo & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo | None | dict[Unknown, Unknown]`
- xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo | None | (@Todo & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo | None | dict[Unknown, Unknown]`
- xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `str | None`, found `@Todo | None | (@Todo & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1624:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `str | None`, found `@Todo | None | dict[Unknown, Unknown]`
- xarray/core/dataarray.py:450:21: error[invalid-assignment] Object of type `list[Unknown | (Any & ~DataArray & ~Top[Series[Any]] & ~DataFrame) | (ReprObject & ~Top[Series[Any]] & ~DataFrame)]` is not assignable to `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, Unknown]] | Mapping[Unknown, Unknown] | None`
+ xarray/core/dataarray.py:450:21: error[invalid-assignment] Object of type `list[Unknown | (ReprObject & ~Top[Series[Any]] & ~DataFrame)]` is not assignable to `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, Unknown]] | Mapping[Unknown, Unknown] | None`
- xarray/plot/utils.py:301:49: error[unsupported-operator] Operator `-` is unsupported between objects of type `(Unknown & ~Iterable[object] & ~None & ~Literal[1]) | list[Unknown] | (Unknown & ~None & ~Literal[1])` and `Literal[1]`
+ xarray/plot/utils.py:301:49: error[unsupported-operator] Operator `-` is unsupported between objects of type `(Unknown & ~None & ~Literal[1]) | list[Unknown]` and `Literal[1]`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/json_util.py:1015:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, (Any, JSONOptions, /) -> Any].__setitem__(key: int, value: (Any, JSONOptions, /) -> Any, /) -> None` cannot be called with a key of type `object` and a value of type `((Any, JSONOptions, /) -> Any) | ((obj: Any, dummy0: Any) -> Any) | ((obj: bytes, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: datetime, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Any, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: int | float, json_options: JSONOptions) -> Any) | ((obj: int, json_options: JSONOptions) -> Any) | ((obj: UUID, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Binary, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Int64, json_options: JSONOptions) -> Any) | ((obj: Code, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: DBRef, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((dummy0: Any, dummy1: Any) -> dict[Unknown, Unknown]) | ((obj: ObjectId, dummy0: Any) -> dict[Unknown, Unknown]) | ((obj: Timestamp, dummy0: Any) -> dict[Unknown, Unknown])` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`
+ bson/json_util.py:1015:9: error[invalid-assignment] Method `__setitem__` of type `bound method dict[int, (Any, JSONOptions, /) -> Any].__setitem__(key: int, value: (Any, JSONOptions, /) -> Any, /) -> None` cannot be called with a key of type `object` and a value of type `((Any, JSONOptions, /) -> Any) | ((obj: Any, dummy0: Any) -> Any) | ((obj: Binary, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: datetime, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Any, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Int64, json_options: JSONOptions) -> Any) | ((obj: UUID, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: Code, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((obj: DBRef, json_options: JSONOptions) -> dict[Unknown, Unknown]) | ((dummy0: Any, dummy1: Any) -> dict[Unknown, Unknown]) | ((obj: ObjectId, dummy0: Any) -> dict[Unknown, Unknown]) | ((obj: Timestamp, dummy0: Any) -> dict[Unknown, Unknown])` on object of type `dict[int, (Any, JSONOptions, /) -> Any]`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/flows.py:288:34: warning[possibly-missing-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & (() -> object) & ~classmethod & ~staticmethod) | (@Todo & (() -> object) & ~classmethod & ~staticmethod) | (((...) -> R@__init__) & (() -> object) & ~staticmethod) | (((...) -> R@__init__) & (() -> object))` may be missing
+ src/prefect/flows.py:288:34: warning[possibly-missing-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & (() -> object)) | (@Todo & (() -> object) & ~classmethod & ~staticmethod)` may be missing
- src/prefect/flows.py:405:68: warning[possibly-missing-attribute] ...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-27 15:21_

---

_Comment by @github-actions[bot] on 2025-09-27 15:26_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 12 | 0 | 32 |
| `unsupported-operator` | 10 | 0 | 7 |
| `possibly-missing-attribute` | 0 | 0 | 14 |
| `invalid-assignment` | 0 | 0 | 12 |
| `invalid-return-type` | 0 | 1 | 5 |
| `no-matching-overload` | 0 | 3 | 0 |
| `possibly-missing-implicit-call` | 0 | 0 | 1 |
| **Total** | **22** | **4** | **71** |

**[Full report with detailed diff](https://alex-third-type-relation.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-third-type-relation.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-09-27 15:28_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fthird-type-relation?runnerMode=WallTime)

### Merging #20602 will **improve performances by 18.36%**

<sub>Comparing <code>alex/third-type-relation</code> (3f54ca7) with <code>main</code> (f9688bd)</sub>



### Summary

`⚡ 2` improvements  
`✅ 6` untouched  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` medium[colour-science] `` | 10.7 s | 9.1 s | +18.36% |
| ⚡ | `` small[freqtrade] `` | 5.4 s | 4.8 s | +13.01% |


---

_Comment by @codspeed-hq[bot] on 2025-09-27 15:29_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fthird-type-relation?runnerMode=Instrumentation)

### Merging #20602 will **improve performances by 25.82%**

<sub>Comparing <code>alex/third-type-relation</code> (3f54ca7) with <code>main</code> (f9688bd)</sub>



### Summary

`⚡ 3` improvements  
`✅ 10` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` attrs `` | 430.1 ms | 387 ms | +11.14% |
| ⚡ | `` DateType `` | 234.3 ms | 186.2 ms | +25.82% |
| ⚡ | `` hydra-zen `` | 849.8 ms | 760.8 ms | +11.69% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Fthird-type-relation?runnerMode=Instrumentation&sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Label `performance` added by @AlexWaygood on 2025-09-27 15:33_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-27 15:41_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-27 15:41_

---

_Comment by @AlexWaygood on 2025-09-27 16:33_

# Ecosystem analysis

This all looks good to me.

Here's a slightly more minimal example of what's going on with the new schemathesis diagnostics. On `main` we have this behaviour:

```py
from typing import Any

def f(x: dict[Any, Any] | dict[Any | str, Any | dict[Any, Any]]):
    reveal_type(x)            # dict[Any, Any] | dict[Any | str, Any | dict[Any, Any]]
    reveal_type(x.items())    # dict_items[Any, Any] | dict_items[Any | str, Any | dict[Any, Any]]

    for a, b in x.items():
        reveal_type(a)        # Any
        reveal_type(b)        # Any
```

And on this PR we have this behaviour:

```py
from typing import Any

def f(x: dict[Any, Any] | dict[Any | str, Any | dict[Any, Any]]):
    reveal_type(x)            # dict[Any, Any] | dict[Any | str, Any | dict[Any, Any]]
    reveal_type(x.items())    # dict_items[Any | str, Any | dict[Any, Any]]

    for a, b in x.items():
        reveal_type(a)        # Any | str
        reveal_type(b)        # Any | dict[Any, Any]
```

`dict_items[Any, Any] | dict_items[Any | str, Any | dict[Any, Any]]` is recognised as being simplifiable to `dict_items[Any | str, Any | dict[Any, Any]]` on this PR branch because `dict_items` is covariant in both its type parameters. The simplification of the union to a non-union type means that we avoid a known bug when iterating over unions (https://github.com/astral-sh/ty/issues/1089, which will be fixed by https://github.com/astral-sh/ruff/pull/20368), so in this case actually means that we preserve a stricter type at the end of the day.

The new diagnostics on `xarray` and `zulip` seem to have a similar cause.

---

_Marked ready for review by @AlexWaygood on 2025-09-27 16:33_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-27 16:33_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-27 16:33_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-27 16:33_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-27 16:41_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-27 16:41_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1519 on 2025-09-29 14:58_

Maybe this is the key bit? Can this be expressed in terms of the top/bottom materializations of the two types?

---

_@dcreager reviewed on 2025-09-29 14:58_

The code simplification and performance wins are huge. My main holdup is from a type theoretic viewpoint. Right now we can define the two existing relations succinctly, both in English, and in terms of materializations:

- subtyping: Every materialization of `S` is a subtype of every materialization of `T`. To prove that, we only have to show that `S+ ≤ T-`, because of transitivity.

- assignability: There is _some_ materialization of `T` that _every_ materialization of `S` is a subtype of. To prove that, we show that `S+ ≤ T+`.

Is there a similar definition that we can make for union simplification?

---

_Comment by @carljm on 2025-09-29 17:21_

I suspect that the relation implemented here isn't theoretically consistent, but it's nonetheless practically useful, and so we should probably document the theoretical inconsistency and do it anyway.

I think this goes back to the discussion on https://github.com/astral-sh/ruff/pull/18799 (and specifically [this comment](https://github.com/astral-sh/ruff/pull/18799#issuecomment-2993917848)). There is a more generalized definition of subtyping for gradual types (proposed by @JelleZijlstra in that PR discussion) where the rule is that `S <: T iff S- <: T- && S+ <: T+` -- in other words, every materialization of S must be a subtype of some materialization of T, and every materialization of T must be a supertype of some materialization of S.

Unfortunately we can't fully use that definition, because it breaks down (and introduces non-transitivity) in the interaction between nominal and non-fully-static structural types. It introduces cases where a nominal type X is a (nominal) subtype of a nominal type Y, but overrides a gradual attribute of Y with a more precise type, in such a way that now Y is a subtype of some structural type P, but X is not. Which gives that `X <: Y` and `Y <: P`, but `X </: P`, breaking transitivity. (One theoretical solution to this is to treat all types as structural, with nominal subtyping becoming an additional structural "tag", such that X subclassing Y doesn't necessarily imply `X <: Y`, but that's probably too big a departure from the status quo to be workable for us.)

If we _could_ use that definition of subtyping, it would remove the need to check for both equivalence and subtyping in union simplification, because under that definition of gradual subtyping, any two equivalent types (even gradual ones) are subtypes of each other (that is, it generalizes reflexivity of subtyping.)

I believe that the addition of the `t.into_intersection().is_some_and(|intersection| intersection.positive(db).contains(&u)) || u.into_union().is_some_and(|union| union.elements(db).contains(&t))` clauses to `UnionBuilder` simplification, and now the formalization of that into the new `UnionSimplification` relation, is implementing a partial / ad-hoc version of the more generalized definition of gradual subtyping, but by applying it only to unions and intersections (and not to subtyping of structural types) it avoids the non-transitivity problem.

---

_Comment by @AlexWaygood on 2025-09-30 14:30_

Okay -- I've had a go at thinking through and writing up some theoretical foundations for this! LMK what you think.

---

_@AlexWaygood reviewed on 2025-09-30 14:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1519 on 2025-09-30 14:33_

I think so, yes: although `Any & T` is not a subtype of `Any`, for any type `T` it is redundant to add `Any & T` to a union that already contains `Any` as the bottom materialization of `(Any & T) | Any` will always be equivalent to the bottom materialization of `Any`, and the top materialization of `(Any & T) | Any` will always be equivalent to the top materialization of `Any`.

I wrote this up in full as the doc-comment to `TypeRelation::UnionSimplification`.

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-30 14:33_

---

_@JelleZijlstra reviewed on 2025-09-30 14:49_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types.rs`:9038 on 2025-09-30 14:49_

You can also express this as `Bottom[D] <: Top[C]`.

---

_@JelleZijlstra reviewed on 2025-09-30 14:54_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-09-30 14:54_

I think the name is a bit unfortunate because it's hard to formulate other terms from it. I'll say that `D` is union-simplifiable with regards to `C` if the relation holds between D and C.

Then, I think the formal formulation can be that `D union-simplifiable C` if `C` is equivalent to `C | D`. Equivalence, as you probably discuss somewhere, for fully static types means that the sets of values described by both types are the same; for dynamic types it means that the top and bottom materializations of both types are equivalent.

---

_@JelleZijlstra reviewed on 2025-09-30 14:55_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-09-30 14:55_

I looked at the code a bit more and maybe "redundant with" is a good name. `C` is redundant with `D` if the union `C | D` can be safely simplified to `D`.

---

_@AlexWaygood reviewed on 2025-09-30 14:58_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-09-30 14:58_

> I think the name is a bit unfortunate because it's hard to formulate other terms from it

Agreed... but I'm struggling to think of anything better!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9065 on 2025-09-30 14:58_

```suggestion
    /// For a pair of [fully static] types `A` and `B`, the union simplification relation
```

---

_@AlexWaygood reviewed on 2025-09-30 14:58_

---

_@AlexWaygood reviewed on 2025-09-30 14:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-09-30 14:59_

> I looked at the code a bit more and maybe "redundant with" is a good name. `C` is redundant with `D` if the union `C | D` can be safely simplified to `D`.

Hmm, maybe `TypeRelation::UnionRedundancy`? And we can say `C` is union-redundant with `D` if the union `C | D` can be safely simplified to `D`

---

_@JelleZijlstra reviewed on 2025-09-30 15:01_

---

_Review comment by @JelleZijlstra on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-09-30 15:01_

Doesn't it also apply to intersections? That's why I wanted to avoid using the term "union".

---

_@AlexWaygood reviewed on 2025-09-30 15:03_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-09-30 15:03_

it does, yes, fair point

---

_@JelleZijlstra reviewed on 2025-09-30 15:10_

This looks correct and useful to me, besides my suggestions about the name and phrasing.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:9038 on 2025-09-30 15:23_

nit: I would use "some" here instead of "any" to minimize the chance of confusing it for "all"

(also a spelling nit)

```suggestion
    /// can be said to be assignable to `C` if *some* possible fully static [materialization]
    /// of `D` is a subtype of *some* possible fully static materialization of `C`.
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:9075 on 2025-09-30 15:29_

I think (1) is redundant :drum: with (2), and you can simplify the whole thing to "`C | D` is gradually equivalent to `D`, and `C & D` is gradually equivalent to `C`". That even nicely maps with the intuitive English definition. (You might still keep (1) as a separate check in the implementation, as an optimization, but that's not needed here in the theoretical definition)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-09-30 15:34_

Though I think the relation is reversed for intersection, right? If we use `≤R` for the new redundancy relation, and `=G` for gradual equivalence, then its

```
C ≤U D <=> (C | D) =G D
C ≤U D <=> (C & D) =G C
```

Mnemonics: the smaller thing is on the left of the relation; union makes things bigger; intersection makes things smaller

(That doesn't change Jelle's point that we don't need to include "union" in the name of the relation since it's relevant to intersections too)

---

_@dcreager reviewed on 2025-09-30 15:36_

---

_Comment by @AlexWaygood on 2025-09-30 16:56_

This is now stacked on top of https://github.com/astral-sh/ruff/pull/20650 and https://github.com/astral-sh/ruff/pull/20651, which I _think_ are uncontroversial bugfixes that should probably be landed first

---

_@AlexWaygood reviewed on 2025-10-03 11:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-10-03 11:27_

@dcreager, are you able to think of any test cases I could add for intersections (that don't already pass with the logic we have on `main`)? I'm struggling to come up with any, and I can't tell if this is because my head is going fuzzy with all the set theory or because our DNF representation of unions and intersections actually makes the question redundant (🥁) when it comes to intersections.

If it's the latter, then `UnionRedundancy` might not be such a bad name after all, and it might make the concept easier to reason about if we stated that it only applied to union simplification (not intersection simplification)

---

_@AlexWaygood reviewed on 2025-10-03 11:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9075 on 2025-10-03 11:34_

Oh, great point

---

_@AlexWaygood reviewed on 2025-10-03 11:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-10-03 11:47_

but... just because we can't think of any new test cases doesn't mean that it's wrong to replace the existing `if x.is_subtype_of(y) || x.is_equivalent_to(y)` branches in our intersection simplification logic with the new `x.is_redundant_with(y)` method. So it does seem like it's useful (purely from a performance perspective, if nothing else) to apply it to both unions and intersections, even if (unlike with unions) this doesn't lead to any semantic improvement over what we have on `main`.

---

_Comment by @AlexWaygood on 2025-10-03 11:59_

> This is now stacked on top of #20650 and #20651, which I _think_ are uncontroversial bugfixes that should probably be landed first

Seems like #20651 wasn't so uncontroversial 😆 and my head is going fuzzy with all the type theory there.

On further reflection, I _think_ it should be okay to land this without #20651, so I've rebased this on top of `main` again as a standalone change.

---

_Renamed from "[ty] Introduce `TypeRelation::UnionSimplification`" to "[ty] Introduce `TypeRelation::Redundancy`" by @AlexWaygood on 2025-10-03 12:00_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-03 12:01_

---

_Comment by @AlexWaygood on 2025-10-03 12:02_

I think I addressed all review comments. This should be ready for another round of review.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-10-03 12:02_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-03 12:02_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/union_types.md`:371 on 2025-10-03 17:17_

Incidentally, it would be nice to have a version of `reveal_type` that takes in a type-form parameter, so that we don't have to jump through these hoops to get something we can pass in to `typing_extensions.reveal_type`

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:9068 on 2025-10-03 17:24_

> re you able to think of any test cases I could add for intersections (that don't already pass with the logic we have on `main`)?

I can't think of any either. We can add those as a follow-on if we encounter any.

> So it does seem like it's useful (purely from a performance perspective, if nothing else) to apply it to both unions and intersections, even if (unlike with unions) this doesn't lead to any semantic improvement over what we have on `main`.

Agreed

---

_@dcreager approved on 2025-10-03 17:24_

---

_Merged by @AlexWaygood on 2025-10-03 17:35_

---

_Closed by @AlexWaygood on 2025-10-03 17:35_

---

_Branch deleted on 2025-10-03 17:35_

---

_@sharkdp reviewed on 2025-10-06 09:31_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9077 on 2025-10-06 09:31_

Another way of saying this would be: `D` can be said to be redundant in a union with `C` if `C | D` is gradually equivalent to `C`.

The top and bottom materializations of two gradual types are the same if and only if they are gradually equivalent.

And the two conditions in the end could also be reformulated as: `Top[D] <: Top[C] AND Bottom[D] <: Bottom[C]`, which would show that "redundancy" is equivalent to the "other subtyping" relation that @carljm experimented with in https://github.com/astral-sh/ruff/pull/18799 as well:

| Relation      | Conditions                                    |
|---------------|-----------------------------------------------|
| Subtyping     | `Top[D] <: Bottom[C]`                         |
| Redundancy    | `Top[D] <: Top[C] AND Bottom[D] <: Bottom[C]` |
| Assignability | `Bottom[D] <: Top[C]`                         |

This also shows that subtyping always implies redundancy, and redundancy implies assignability.


---

_@AlexWaygood reviewed on 2025-10-06 09:43_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:9077 on 2025-10-06 09:43_

> Another way of saying this would be: `D` can be said to be redundant in a union with `C` if `C | D` is gradually equivalent to `C`.

Correct (though gradual equivalence could, I think, be defined in terms of "mutual redundancy", and if we defined redundancy using gradual equivalence, that would be a little cyclic 😆).

> This also shows that subtyping always implies redundancy, and redundancy implies assignability.

Yes, I was plannning on adding property tests to this effect

---

_@sharkdp reviewed on 2025-10-06 09:45_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9077 on 2025-10-06 09:45_

Reversing this logic also implies that gradual equivalence between `C` and `D` is equivalent to mutual redundancy between `C` and `D`. Which could hint at another strategy to re-implement gradual equivalence in ty.

---

_@sharkdp reviewed on 2025-10-06 09:46_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9077 on 2025-10-06 09:46_

Oh, I just now saw your comment. Glad we both came up with the term "mutual redundancy" :smile: 

---

_@sharkdp reviewed on 2025-10-06 11:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:9077 on 2025-10-06 11:12_

A quick summary of my post-merge review and discussion with Alex: Given that the redundancy operation *described* in this PR seems to be equivalent to the "S+ <: T+ && S- <: T-" subtyping from https://github.com/astral-sh/ruff/pull/18799, we should probably try to document why the same problem mentioned in that PR with transitivity of subtyping is not relevant here (probably because what has been *implemented* here does not fully match the defined redundancy relation).

---
