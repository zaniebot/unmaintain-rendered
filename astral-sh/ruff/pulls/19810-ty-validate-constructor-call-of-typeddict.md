```yaml
number: 19810
title: "[ty] validate constructor call of `TypedDict`"
type: pull_request
state: merged
author: PrettyWood
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ty-typeddict-constructor
created_at: 2025-08-07T15:57:49Z
updated_at: 2025-08-25T17:49:13Z
url: https://github.com/astral-sh/ruff/pull/19810
synced_at: 2026-01-12T15:56:47Z
```

# [ty] validate constructor call of `TypedDict`

---

_@PrettyWood_

## Summary
Implement validation for `TypedDict` constructor calls and dictionary literal assignments, including support for `total=False` and proper field management.
Also add support for `Required` and `NotRequired` type qualifiers in `TypedDict` classes, along with proper inheritance behavior and the `total=` parameter.
Support both constructor calls and dict literal syntax

part of https://github.com/astral-sh/ty/issues/154

### Basic Required Field Validation
```py
class Person(TypedDict):
    name: str
    age: int | None

# Error: Missing required field 'name' in TypedDict `Person` constructor
incomplete = Person(age=25)

# Error: Invalid argument to key "name" with declared type `str` on TypedDict `Person`
wrong_type = Person(name=123, age=25)

# Error: Invalid key access on TypedDict `Person`: Unknown key "extra"
extra_field = Person(name="Bob", age=25, extra=True)
```
<img width="773" height="191" alt="Screenshot 2025-08-07 at 17 59 22" src="https://github.com/user-attachments/assets/79076d98-e85f-4495-93d6-a731aa72a5c9" />

### Support for `total=False`
```py
class OptionalPerson(TypedDict, total=False):
    name: str
    age: int | None

# All valid - all fields are optional with total=False
charlie = OptionalPerson()
david = OptionalPerson(name="David")
emily = OptionalPerson(age=30)
frank = OptionalPerson(name="Frank", age=25)

# But type validation and extra fields still apply
invalid_type = OptionalPerson(name=123)  # Error: Invalid argument type
invalid_extra = OptionalPerson(extra=True)  # Error: Invalid key access
```

### Dictionary Literal Validation
```py
# Type checking works for both constructors and dict literals
person: Person = {"name": "Alice", "age": 30}

reveal_type(person["name"])  # revealed: str
reveal_type(person["age"])   # revealed: int | None

# Error: Invalid key access on TypedDict `Person`: Unknown key "non_existing"
reveal_type(person["non_existing"])  # revealed: Unknown
```

### `Required`, `NotRequired`, `total`
```python
from typing import TypedDict
from typing_extensions import Required, NotRequired

class PartialUser(TypedDict, total=False):
    name: Required[str]      # Required despite total=False
    age: int                 # Optional due to total=False
    email: NotRequired[str]  # Explicitly optional (redundant)

class User(TypedDict):
    name: Required[str]      # Explicitly required (redundant)
    age: int                 # Required due to total=True
    bio: NotRequired[str]    # Optional despite total=True

# Valid constructions
partial = PartialUser(name="Alice")  # name required, age optional
full = User(name="Bob", age=25)      # name and age required, bio optional

# Inheritance maintains original field requirements
class Employee(PartialUser):
    department: str                  # Required (new field)
    # name: still Required (inherited)
    # age: still optional (inherited)

emp = Employee(name="Charlie", department="Engineering")  # ✅
Employee(department="Engineering")  # ❌
e: Employee = {"age": 1}  # ❌
```

<img width="898" height="683" alt="Screenshot 2025-08-11 at 22 02 57" src="https://github.com/user-attachments/assets/4c1b18cd-cb2e-493a-a948-51589d121738" />

## Implementation
The implementation reuses existing validation logic done in https://github.com/astral-sh/ruff/pull/19782

### ℹ️ Why I did NOT synthesize an `__init__` for `TypedDict`:

`TypedDict` inherits `dict.__init__(self, *args, **kwargs)` that accepts all arguments.
The type resolution system finds this inherited signature **before** looking for synthesized members.
So `own_synthesized_member()` is never called because a signature already exists.

To force synthesis, you'd have to override Python’s inheritance mechanism,  which would break compatibility with the existing ecosystem.

This is why I went with ad-hoc validation. IMO it's the only viable approach that respects Python’s  
inheritance semantics while providing the required validation.

### Refacto of `Field`

**Before:**
```rust
struct Field<'db> {
    declared_ty: Type<'db>,
    default_ty: Option<Type<'db>>,     // NamedTuple and dataclass only
    init_only: bool,                   // dataclass only  
    init: bool,                        // dataclass only
    is_required: Option<bool>,         // TypedDict only
}
```

**After:**
```rust
struct Field<'db> {
    declared_ty: Type<'db>,
    kind: FieldKind<'db>,
}

enum FieldKind<'db> {
    NamedTuple { default_ty: Option<Type<'db>> },
    Dataclass { default_ty: Option<Type<'db>>, init_only: bool, init: bool },
    TypedDict { is_required: bool },
}
```

## Test Plan
Updated Markdown tests

---

_Review requested from @carljm by @PrettyWood on 2025-08-07 15:57_

---

_Review requested from @AlexWaygood by @PrettyWood on 2025-08-07 15:57_

---

_Review requested from @sharkdp by @PrettyWood on 2025-08-07 15:57_

---

_Review requested from @dcreager by @PrettyWood on 2025-08-07 15:57_

---

_Renamed from "[ty] validate constructor call of TypedDict" to "[ty] validate constructor call of `TypedDict`" by @PrettyWood on 2025-08-07 15:58_

---

_Comment by @github-actions[bot] on 2025-08-07 16:02_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-25 12:37:55.634200073 +0000
+++ new-output.txt	2025-08-25 12:37:58.166200560 +0000
@@ -847,8 +847,16 @@
 tuples_type_form.py:15:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""]]` is not assignable to `tuple[int, int]`
 tuples_type_form.py:25:1: error[invalid-assignment] Object of type `tuple[Literal[1]]` is not assignable to `tuple[()]`
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
+typeddicts_operations.py:37:5: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
+typeddicts_operations.py:62:1: error[unresolved-attribute] Type `MovieOptional` has no attribute `clear`
+typeddicts_readonly_inheritance.py:65:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `RequiredName` constructor
+typeddicts_type_consistency.py:69:21: error[invalid-key] Invalid key access on TypedDict `A3`: Unknown key "y"
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
+typeddicts_usage.py:23:7: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "director"
+typeddicts_usage.py:24:17: error[invalid-assignment] Invalid assignment to key "year" with declared type `int` on TypedDict `Movie`: value of type `Literal["1982"]`
+typeddicts_usage.py:28:1: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
+typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 851 diagnostics
+Found 859 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-07 16:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
aioredis (https://github.com/aio-libs/aioredis)
- aioredis/connection.py:1201:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/connection.py:1205:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 17 diagnostics
+ Found 15 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- chess/engine.py:1778:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- chess/engine.py:1799:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 16 diagnostics
+ Found 14 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/types.py:3090:52: warning[possibly-unbound-attribute] Attribute `get` on type `(Unknown & ~tuple[Unknown, ...]) | (tuple[Unknown, str] & ~tuple[Unknown, ...]) | @Todo(Type::Intersection.call())` is possibly unbound
+ pydantic/v1/networks.py:232:13: error[invalid-key] Invalid key access on TypedDict `Parts`: Unknown key "host" - did you mean "port"?
- Found 767 diagnostics
+ Found 769 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:24:5: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "name"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:25:5: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "title"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:26:5: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "description"
+ tests/integration_tests/assets/dropins/cc_custom_module_24_1.py:30:5: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "examples"
+ tests/unittests/config/test_modules.py:73:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "name"
+ tests/unittests/config/test_modules.py:75:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "title"
+ tests/unittests/config/test_modules.py:76:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "description"
+ tests/unittests/config/test_modules.py:78:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "examples"
+ tests/unittests/config/test_modules.py:113:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "name"
+ tests/unittests/config/test_modules.py:115:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "title"
+ tests/unittests/config/test_modules.py:116:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "description"
+ tests/unittests/config/test_modules.py:118:13: error[invalid-key] Invalid key access on TypedDict `MetaSchema`: Unknown key "examples"
- Found 619 diagnostics
+ Found 631 diagnostics

AutoSplit (https://github.com/Toufool/AutoSplit)
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'split_hotkey' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'reset_hotkey' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'undo_split_hotkey' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'skip_split_hotkey' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'pause_hotkey' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'screenshot_hotkey' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'toggle_auto_reset_image_hotkey' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'fps_limit' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'live_capture_region' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'capture_method' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'capture_device_id' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'capture_device_name' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'default_comparison_method' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'default_similarity_threshold' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'default_delay_time' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'default_pause_time' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'loop_splits' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'start_also_resets' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'enable_auto_reset' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'split_image_directory' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'screenshot_directory' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'open_screenshot' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'screenshot_on' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'captured_window_title' in TypedDict `UserProfileDict` constructor
+ src/user_profile.py:144:35: error[missing-typed-dict-key] Missing required key 'capture_region' in TypedDict `UserProfileDict` constructor
- Found 110 diagnostics
+ Found 135 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/freqtradebot.py:2406:13: error[missing-typed-dict-key] Missing required key 'id' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2406:13: error[missing-typed-dict-key] Missing required key 'pair' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2406:13: error[missing-typed-dict-key] Missing required key 'lock_time' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2406:13: error[missing-typed-dict-key] Missing required key 'lock_timestamp' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2406:13: error[missing-typed-dict-key] Missing required key 'lock_end_time' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2406:13: error[missing-typed-dict-key] Missing required key 'lock_end_timestamp' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2406:13: error[missing-typed-dict-key] Missing required key 'reason' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2406:13: error[missing-typed-dict-key] Missing required key 'side' in TypedDict `RPCProtectionMsg` constructor
+ freqtrade/freqtradebot.py:2406:13: error[missing-typed-dict-key] Missing required key 'active' in TypedDict `RPCProtectionMsg` constructor
- Found 375 diagnostics
+ Found 384 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/primary_guild.py:86:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 541 diagnostics
+ Found 540 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/interpreter/interpreter.py:861:32: error[invalid-argument-type] Invalid argument to key "default_options" with declared type `list[str] | dict[str, @Todo(Support for `typing.TypeAlias`)] | str` on TypedDict `DoSubproject`: value of type `dict[OptionKey, @Todo(Support for `typing.TypeAlias`)]`
- mesonbuild/interpreter/interpreter.py:3408:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["darwin_versions"], value: tuple[str, str] | None, /) -> None, (key: Literal["soversion"], value: str | None, /) -> None, (key: Literal["version"], value: str | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["dependencies"]` and a value of type `list[Unknown]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3408:9: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["darwin_versions"], value: tuple[str, str] | None, /) -> None, (key: Literal["soversion"], value: str | None, /) -> None, (key: Literal["version"], value: str | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["dependencies"]` and a value of type `list[Unknown]` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- mesonbuild/interpreter/interpreter.py:3427:13: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["darwin_versions"], value: tuple[str, str] | None, /) -> None, (key: Literal["soversion"], value: str | None, /) -> None, (key: Literal["version"], value: str | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: @Todo(`NotRequired[]` type qualifier), /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["d_import_dirs"]` and a value of type `Unknown` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
+ mesonbuild/interpreter/interpreter.py:3427:13: error[invalid-assignment] Method `__setitem__` of type `(Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["export_dynamic"], value: bool | None, /) -> None, (key: Literal["gui_app"], value: bool | None, /) -> None, (key: Literal["implib"], value: str | bool | None, /) -> None, (key: Literal["pie"], value: bool | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["win_subsystem"], value: str | None, /) -> None, (key: Literal["android_exe_type"], value: Literal["application", "executable"] | None, /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["prelink"], value: bool, /) -> None, (key: Literal["pic"], value: bool | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["darwin_versions"], value: tuple[str, str] | None, /) -> None, (key: Literal["soversion"], value: str | None, /) -> None, (key: Literal["version"], value: str | None, /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None]) | (Overload[(key: Literal["rust_abi"], value: Literal["c", "rust"] | None, /) -> None, (key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["d_debug"], value: list[str | int], /) -> None, (key: Literal["d_import_dirs"], value: list[str | IncludeDirs], /) -> None, (key: Literal["d_module_versions"], value: list[str | int], /) -> None, (key: Literal["d_unittest"], value: bool, /) -> None, (key: Literal["rust_dependency_map"], value: dict[str, str], /) -> None, (key: Literal["swift_interoperability_mode"], value: Literal["c", "cpp"], /) -> None, (key: Literal["swift_module_name"], value: str, /) -> None, (key: Literal["sources"], value: Unknown, /) -> None, (key: Literal["c_args"], value: list[str], /) -> None, (key: Literal["cpp_args"], value: list[str], /) -> None, (key: Literal["cuda_args"], value: list[str], /) -> None, (key: Literal["fortran_args"], value: list[str], /) -> None, (key: Literal["d_args"], value: list[str], /) -> None, (key: Literal["objc_args"], value: list[str], /) -> None, (key: Literal["objcpp_args"], value: list[str], /) -> None, (key: Literal["rust_args"], value: list[str], /) -> None, (key: Literal["vala_args"], value: list[str | File], /) -> None, (key: Literal["cs_args"], value: list[str], /) -> None, (key: Literal["swift_args"], value: list[str], /) -> None, (key: Literal["cython_args"], value: list[str], /) -> None, (key: Literal["nasm_args"], value: list[str], /) -> None, (key: Literal["masm_args"], value: list[str], /) -> None, (key: Literal["vs_module_defs"], value: str | File | CustomTarget | CustomTargetIndex | None, /) -> None]) | (Overload[(key: Literal["build_by_default"], value: bool, /) -> None, (key: Literal["build_rpath"], value: str, /) -> None, (key: Literal["extra_files"], value: list[Unknown], /) -> None, (key: Literal["gnu_symbol_visibility"], value: str, /) -> None, (key: Literal["install"], value: bool, /) -> None, (key: Literal["install_mode"], value: FileMode, /) -> None, (key: Literal["install_rpath"], value: str, /) -> None, (key: Literal["implicit_include_directories"], value: bool, /) -> None, (key: Literal["link_depends"], value: list[str | File | CustomTarget | CustomTargetIndex | BuildTarget], /) -> None, (key: Literal["link_language"], value: str | None, /) -> None, (key: Literal["name_prefix"], value: str | None, /) -> None, (key: Literal["name_suffix"], value: str | None, /) -> None, (key: Literal["native"], value: MachineChoice, /) -> None, (key: Literal["objects"], value: list[Unknown], /) -> None, (key: Literal["override_options"], value: dict[OptionKey, @Todo(Support for `typing.TypeAlias`)], /) -> None, (key: Literal["depend_files"], value: list[File], /) -> None, (key: Literal["resources"], value: list[str], /) -> None, (key: Literal["main_class"], value: str, /) -> None, (key: Literal["java_resources"], value: StructuredSources | None, /) -> None, (key: Literal["sources"], value: str | File | CustomTarget | CustomTargetIndex | GeneratedList | ExtractedObjects | BuildTarget, /) -> None, (key: Literal["java_args"], value: list[str], /) -> None])` cannot be called with a key of type `Literal["d_import_dirs"]` and a value of type `Unknown` on object of type `Executable | StaticLibrary | SharedLibrary | SharedModule | Jar`
- Found 807 diagnostics
+ Found 808 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/runner.py:324:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2949 diagnostics
+ Found 2948 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/llmobs/_experiment.py:172:9: error[missing-typed-dict-key] Missing required key 'input_data' in TypedDict `DatasetRecord` constructor
+ ddtrace/llmobs/_experiment.py:172:9: error[missing-typed-dict-key] Missing required key 'expected_output' in TypedDict `DatasetRecord` constructor
+ ddtrace/llmobs/_experiment.py:172:9: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ ddtrace/llmobs/_llmobs.py:1646:43: error[invalid-key] Invalid key access on TypedDict `LLMObsEvaluationMetricEvent`: Unknown key "metadata"
+ ddtrace/llmobs/_llmobs.py:1784:43: error[invalid-key] Invalid key access on TypedDict `LLMObsEvaluationMetricEvent`: Unknown key "metadata"
+ tests/llmobs/test_experiments.py:73:9: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:73:9: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:339:13: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:339:13: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:340:13: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:340:13: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:351:9: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:351:9: error[missing-typed-dict-key] Missing required key 'expected_output' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:351:9: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:421:7: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:421:7: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:428:9: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:428:9: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:458:7: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:458:7: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:473:7: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:473:7: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:509:13: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:549:7: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:549:7: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:553:9: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:553:9: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:583:7: error[missing-typed-dict-key] Missing required key 'metadata' in TypedDict `DatasetRecord` constructor
+ tests/llmobs/test_experiments.py:583:7: error[missing-typed-dict-key] Missing required key 'record_id' in TypedDict `Datas...*[Comment body truncated]*

---

_Label `ty` added by @AlexWaygood on 2025-08-07 16:04_

---

_Label `diagnostics` added by @dhruvmanila on 2025-08-08 07:04_

---

_Review requested from @MichaReiser by @PrettyWood on 2025-08-10 20:36_

---

_Comment by @carljm on 2025-08-15 01:06_

This is very cool. Sorry I haven't gotten a chance to review it yet; will try to soon, or may leave it at this point for @sharkdp to look at next week when he's back from vacation.

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-18 13:05_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:226 on 2025-08-18 13:10_

Maybe add a TODO comment here that we might consider emitting a diagnostic? For example, pyright does:
```
  /home/shark/playground/test.py:15:13 - error: Could not access item in TypedDict
    "name" is not a required key in "OptionalPerson", so access may result in runtime exception (reportTypedDictNotRequiredAccess)
  /home/shark/playground/test.py:15:13 - information: Type of "charlie["name"]" is "str"
```

---

_Comment by @github-actions[bot] on 2025-08-18 13:14_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `missing-typed-dict-required-field` | 86 | 0 | 0 |
| `invalid-key` | 15 | 0 | 0 |
| `unused-ignore-comment` | 0 | 7 | 0 |
| `invalid-argument-type` | 2 | 0 | 0 |
| `invalid-assignment` | 0 | 0 | 2 |
| `possibly-unbound-attribute` | 1 | 0 | 0 |
| **Total** | **104** | **7** | **2** |


---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:1728 on 2025-08-18 13:29_

I'm wondering if the rule name should mention `TypedDict` somehow? Or do we anticipate other use cases for this rule that are not `TypedDict`-specific?

For comparison, mypy uses `typeddict-item`, and pyrefly uses `typed-dict-key-error`.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1162 on 2025-08-18 13:30_

```suggestion
    /// `NamedTuple` field metadata
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1593 on 2025-08-18 13:32_

```suggestion
    /// This does **not** consider Python default values on parameters — only whether
    /// a type annotation specifies them as required or not.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1596 on 2025-08-18 13:32_

```suggestion
    /// - `NamedTuple` / dataclass: Always `true` (all fields are required).
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1600 on 2025-08-18 13:33_

I think I prefer removing this:
```suggestion
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1590 on 2025-08-18 13:34_

I the term "type-required" an official one? If not, I think I'd just go with "required" (throughout this doc-comment and in the method name). Or maybe just `is_total_typed_dict`? Or reverse the logic and call it `is_non_total_typed_dict`? This way, the doc-comment could also be simplified a lot.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1597 on 2025-08-18 13:36_

Should this be:
```suggestion
    /// - `TypedDict`: Defaults to `true` unless `total=False`.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1609 on 2025-08-18 13:38_

Maybe
```suggestion
        // Look for `total=False` in class keyword arguments. Note that it is fine to only check for
        // Boolean literals here (https://typing.python.org/en/latest/spec/typeddict.html#totality)
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:4703 on 2025-08-18 14:04_

This leads to some immediate benefits (see updated "alice" tests), but it contradicts our usual splitting of declared type and inferred type, where the inferred type can be more precise than the declared type.

Admittedly, I'm struggling to find real-world examples where this might lead to observable differences (let alone actual problems). The best I came up with is the following exampel, which makes use of PEP-728 [`extra_items`](https://peps.python.org/pep-0728/#the-extra-items-class-parameter):
```py
from typing import TypedDict

class OnlyA(TypedDict, extra_items=bool):
    a: int

class Both(TypedDict):
    a: int
    b: bool

only_a: OnlyA = {"a": 1, "b": True}
both: Both = only_a
```
In this constructed example, we would lose the information that `"b"` is a key of the dict literal because the inferred type for `only_a` would be `OnlyA` instead of the type of the dict literal (which we can't really represent right now, but that's beside the point). This would lead to an error on the `both: Both = only_a` assignment, which some might consider correct, but that's how ty works in other cases as well:
```py
a: int = 1
b: Literal[1] = a  # okay!
```

So I guess the benefits far outweigh the drawbacks? But it would be good to get some feedback from others on the team.

---

_@sharkdp reviewed on 2025-08-18 14:08_

Thank you very much — this looks great.

I fixed some merge conflicts on your branch to see the update ecosystem impact.

It's unfortunate that this causes a lot of new false positives, since `NotRequired` is not supported yet (on this branch). I think we should probably rebase https://github.com/astral-sh/ruff/pull/19865 on top of this and consider merging it into this branch before we merge this to `main` (thank you for splitting them in the first place though!). I think you should be able to change the merge base of https://github.com/astral-sh/ruff/pull/19865 to the branch of this PR, so it's easier to review? Or does that only work for branches inside the ruff repo? If so, I could also open an integration branch that we could merge both of these into.

---

_@PrettyWood reviewed on 2025-08-18 14:36_

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/src/types/diagnostic.rs`:1728 on 2025-08-18 14:36_

Yes in my head Namedtuples, dataclasses and typeddict have a lot in common and IMO it made sense to have common errors. But if other libs split into specific errors, I guess it's best to do as well and see later if things should be merged

---

_Comment by @PrettyWood on 2025-08-18 14:45_

Thank you very much @sharkdp for the merge conflict + review! (btw huge fan of your work, I use daily `bat` and `fd`! <3)
I rebased the 6 commits to support `Required` and `NotRequired` and added a last commit with your feedbacks (most of them didn't apply because they were on `are_fields_type_required()`, which I replaced.

---

_@PrettyWood reviewed on 2025-08-18 14:47_

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/src/types/infer.rs`:4703 on 2025-08-18 14:47_

Yes I was wondering if this was the expected way (I guess not haha)
But it made the implementation way easier for my little mind. Happy to change that if needed

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:846 on 2025-08-19 01:06_

nit: let's put all this TypedDict-specific stuff in its own submodule (separate-file submodule, this file is already massive)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:4703 on 2025-08-19 01:23_

Yes, I think we should go ahead with this; I think it's the behavior we want in this case. It may be that the implementation is subsumed in future by a more general implementation of bidirectional checking where we instead pass in the `TypedDict` context to the initial type inference of the dict literal, but it's OK if this particular implementation turns out to be temporary; it makes `TypedDict` much more useful immediately.

---

_@carljm reviewed on 2025-08-19 01:23_

---

_@PrettyWood reviewed on 2025-08-19 07:53_

---

_Review comment by @PrettyWood on `crates/ty_python_semantic/src/types/infer.rs`:846 on 2025-08-19 07:53_

Done in [1983227](https://github.com/astral-sh/ruff/pull/19810/commits/1983227abfa45a56484160d32f9f4dce844a182d)

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-19 08:27_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-19 08:27_

---

_@sharkdp requested changes on 2025-08-19 09:57_

Thank you for merging the `Required`/`NonRequired` changes into this.

I was now able to take a look at the full ecosystem impact. I noticed two problems:

* There are some true positives which users tried to `# type: ignore`. It seems that the way the ignore-comments are placed is not compatible with the diagnostic ranges that we create. It would be good to look into a few of these examples and to create some tests. Maybe we can adapt the diagnostic range to get rid of some of these new diagnostics.
* The errors on `strawberry` are false positives. The code there constructs a TypedDict using `MyTypedDict({"my_field": 1})`, i.e. there is an additional `{…}` layer. This does not seem to be supported yet. I added some tests to demonstrate the current behavior.

While writing these tests, I also noticed that this approach here has some severe limitations. For example, we don't verify a function call like `accepts_person({"name": "Alice"})` where `accepts_person` takes a `Person` `TypedDict`. Also, `TypedDict` construction does not seem to be validated if the left hand side is a non-name target (`house.person = {"name": "Alice"}`). I think we should try to understand these cases better. It might be fine to start with an initial version here that has some false negatives. But maybe this indicates that we should go for a completely different solution here where we infer a new `Type::DictLiteral` type for `{…}` dict literals, that we can then use in normal assignability checks.

---

_Comment by @JelleZijlstra on 2025-08-19 22:16_

> The code there constructs a TypedDict using MyTypedDict({"my_field": 1}), i.e. there is an additional {…} layer.

This is technically not allowed by the spec, but it probably should be. I'll open a discussion.

---

_Comment by @JelleZijlstra on 2025-08-19 22:26_

Refer to https://discuss.python.org/t/allow-typeddict-construction-with-typename/102974 . To be clear, though the spec doesn't currently discuss this syntax, I'd recommend ty add support for it, because it's used in practice and we'll likely add it to the spec.

---

_Comment by @sharkdp on 2025-08-20 08:14_

@carljm and I talked about this PR yesterday. The main conclusion is that we would like to go ahead with what you have. To extend / clarify / correct my comments from above:

* The `# type: ignore` thing => this is not the most important part, but I think we should at least write some tests. It seems to be quite common to suppress type checker errors on `TypedDict` construction.
* The `MyTypedDict({"my_field": 1})` syntax => see Jelle's comment (thank you!). We don't have to support *checking* these constructions, but it would be good not to emit N_keys diagnostics for every such construction. In general, I'm wondering if we should summarize multiple missing keys into a single diagnostic message.
* Concerning the limitation of your approach: we think this is a valuable step anyway. We currently anticipate solving the remaining cases using a variant of https://github.com/astral-sh/ty/issues/168. Your change here is basically a "light" version of adding contextual information that only works for annotated assignments. So we think that large parts of the implementation can be reused when we implement bidirectional type checking.

---

_Comment by @PrettyWood on 2025-08-20 14:58_

Thank you for the valuable feedback. I'll work on the type ignore and construction via dict tonight or tomorrow.
The goal is to make this PR mergeable without raising false negatives.

I already started to work on a new `DictLiteral` type yesterday. But maybe you had something in mind to implement bidirectional checking. You'll tell me!

---

_Comment by @sharkdp on 2025-08-20 15:06_

> I already started to work on a new `DictLiteral` type yesterday. But maybe you had something in mind to implement bidirectional checking. You'll tell me!

Sorry for my misleading comment earlier. I think we basically decided yesterday that bidirectional type inference is a more promising and more general solution here (compared to the `DictLiteral` type), mostly because we're going to need it for other use cases anyway. So it's not clear if adding a `DictLiteral` type would be beneficial long-term. We might not need it anymore once we have bidirectional type inference. And therefore, it's probably not very helpful to invest time into it right now.

---

_Comment by @PrettyWood on 2025-08-20 15:17_

Perfect! No problem! Thanks

---

_Comment by @PrettyWood on 2025-08-20 21:52_

@sharkdp @carljm Update done! I'll update with `main` once I have your approval

---

_Comment by @carljm on 2025-08-20 21:54_

I'll let @sharkdp take the lead on review of this one. We won't be able to fully evaluate the PR until it is updated with main (so we can get ecosystem and conformance suite results from it), but it should be possible to give feedback on the implementation.

---

_@sharkdp reviewed on 2025-08-22 15:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:226 on 2025-08-22 15:39_

This has been marked as resolved, but I don't see a comment?

---

_Comment by @sharkdp on 2025-08-22 15:43_

Thank you very much for the detailed updates! I am planning to do a full review on Monday. Seeing the updated ecosystem results would be great, but I can also fix the conflicts myself, if the branch hasn't been updated by then.

---

_Comment by @PrettyWood on 2025-08-25 09:15_

@sharkdp I just pushed the merge conflict. I won't be there later today. Feel free to change directly whatever you want.
Thank you

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-25 09:20_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-25 09:21_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-25 09:21_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:1696 on 2025-08-25 09:39_

Minor: I think I would prefer to strip the `compute_` prefix from this function name. Same for `compute_typed_dict_params_from_class_def`.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:2525 on 2025-08-25 09:42_

Maybe
```suggestion
                                .expect("TypedDictParams should be available for CodeGeneratorKind::TypedDict")
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/diagnostic.rs`:1769 on 2025-08-25 09:45_

I think I would prefer something slightly shorter. I think we can maybe leave "required" out of the rule name? And use "key" instead of "field"?
```suggestion
    pub(crate) static MISSING_TYPED_DICT_KEY = {
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:18 on 2025-08-25 09:47_

```suggestion
    /// Keeps track of the keyword arguments that were passed-in during class definition.
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:26 on 2025-08-25 09:52_

It looks like `extra_items` will not be a simple flag, so maybe let's just remove this for now?
```suggestion
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/typed_dict.rs`:111 on 2025-08-25 09:52_

```suggestion
    /// For constructor arguments like `MyTypedDict(key=value)`
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:37 on 2025-08-25 09:59_

This seems to be in the wrong position?
```suggestion
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:556 on 2025-08-25 11:48_

Minor
```suggestion
When inheriting from a `TypedDict` with a different `total` setting, inherited fields maintain
their original requirement status, while new fields follow the child class's `total` setting:
```

---

_@sharkdp approved on 2025-08-25 12:03_

Thank you very much. This is great.

The typing conformance diff shows that there is a problem with annotations like `y: ReadOnly[NotRequired[str]]`, where we currently emit an error ("Type qualifier `typing.NotRequired` is not allowed in type expressions"). I think that can be fixed by recognizing `ReadOnly` as a special form / type qualifier as well, making sure that the argument is inferred as a [type annotation, not as a type expression](https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions). I can look into this (and the remaining minor comments).

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-08-25 12:30_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-08-25 12:30_

---

_Merged by @sharkdp on 2025-08-25 12:45_

---

_Closed by @sharkdp on 2025-08-25 12:45_

---

_Branch deleted on 2025-08-25 17:49_

---
