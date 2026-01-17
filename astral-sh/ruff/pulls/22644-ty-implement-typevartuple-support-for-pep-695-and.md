```yaml
number: 22644
title: "[ty] Implement TypeVarTuple support for PEP 695 and legacy syntax"
type: pull_request
state: open
author: bxff
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: feat/typevartuple-support
created_at: 2026-01-17T09:17:10Z
updated_at: 2026-01-17T14:35:20Z
url: https://github.com/astral-sh/ruff/pull/22644
synced_at: 2026-01-17T15:13:10Z
```

# [ty] Implement TypeVarTuple support for PEP 695 and legacy syntax

---

_@bxff_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements comprehensive support for `TypeVarTuple`, closing **[astral-sh/ty#156](https://github.com/astral-sh/ty/issues/156)**. The implementation enables variadic generic types in both modern PEP 695 syntax (`class Array[*Shape]: ...`) and legacy pre-3.12 syntax (`Ts = TypeVarTuple("Ts"); class Array(Generic[Unpack[Ts]]): ...`).

### What is TypeVarTuple?

`TypeVarTuple` introduces variadic type variables that capture a variable number of type parameters, enabling patterns like generic tensor classes with arbitrary dimension counts:

```python
# Modern syntax (PEP 695)
class Array[*Shape]: ...

# Legacy syntax
from typing_extensions import TypeVarTuple, Unpack, Generic
Shape = TypeVarTuple("Shape")
class Array(Generic[Unpack[Shape]]): ...
```

### Key Accomplishments

**1. Core Type System Foundation**
- Added `TypeVarKind::TypeVarTuple` and `TypeVarKind::Pep695TypeVarTuple` variants to distinguish definition styles
- Implemented `is_typevartuple()` helpers throughout the type system
- Proper `TypeVarInstance` creation for PEP 695 definitions with full default expression support
- Correct translation of AST `TypeParam::TypeVarTuple` nodes into type instances

**2. Legacy Constructor Support (`TypeVarTuple("Ts")`)**
- Full validation of legacy constructor calls with proper name matching and parameter handling
- Support for the `default=` parameter (Python 3.13+) with appropriate version gating
- Diagnostic emission via `INVALID_LEGACY_TYPE_VARIABLE` for malformed definitions
- Special handling to prevent "not-iterable" errors when unpacking in generic bases

**3. `Unpack[Ts]` Resolution Everywhere**
- Fixed `Unpack[Ts]` in type expressions to resolve directly to the underlying `TypeVarTuple`
- Corrected subscript handling to return the type variable instead of placeholder types
- Resolved critical issue where base class contexts returned `@Todo(typing.Unpack)` instead of the actual type variable

**4. Legacy TypeVar Collection**
- Enhanced `find_legacy_typevars` to discover `TypeVarTuple` instances in `Generic[Unpack[Ts]]` constructs
- Proper extraction from `KnownInstanceType::TypeVar` variants with correct binding contexts
- Complete traversal of generic base classes and protocols

**5. Display & Developer Experience**
- Added `*` prefix for variadic type variables (and `**` for ParamSpecs) in both normal and full display modes
- Clear, consistent representation in error messages and `reveal_type()` output

**Files Modified**
- `crates/ty_python_semantic/src/types.rs` - Core type definitions and legacy collection
- `crates/ty_python_semantic/src/types/infer/builder.rs` - Definition inference and constructor handling
- `crates/ty_python_semantic/src/types/generics.rs` - AST translation
- `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs` - Type expression resolution
- `crates/ty_python_semantic/src/types/display.rs` - Visual representation

## Test Plan

### Automated Testing
Ran the full `mdtest` suite with **313 passed** cases. Updated test expectations to reflect correct behavior:

```diff
- GenericContext[]
+ GenericContext[*Ts@SingleTypeVarTuple]

- GenericContext[T@...]
+ GenericContext[T@..., *Ts@...]

- None  # Legacy TypeVarTuple classes
+ GenericContext[*Ts@...]

- P@...  # ParamSpec display
+ **P@...
```

### Manual Validation
Verified complex inheritance patterns with `ty check`:

```python
# PEP 695 syntax
class Array[*Shape]: ...
reveal_type(generic_context(Array))  # GenericContext[*Shape@Array]

# Legacy syntax with Unpack
from typing_extensions import TypeVarTuple, Unpack, Generic
Ts = TypeVarTuple("Ts")
class Tensor(Generic[Unpack[Ts]]): ...
reveal_type(generic_context(Tensor))  # GenericContext[*Ts@Tensor]
```

### Known Limitations

1. **Starred syntax in base classes**: `class Stars(Generic[*Ts])` currently reveals `None` due to a different code path in `legacy_generic_class_context`. This is flagged for follow-up work.

2. **typing_extensions default parameter**: Cannot reliably detect module origin during definition inference without causing assertion failures. May emit false positive diagnostics for `typing_extensions.TypeVarTuple(default=...)` on Python < 3.13.

These limitations are documented in code comments and test files, and do not impact the primary use cases for either PEP 695 or legacy `Unpack[Ts]` syntax.

---

_Review requested from @carljm by @bxff on 2026-01-17 09:17_

---

_Review requested from @AlexWaygood by @bxff on 2026-01-17 09:17_

---

_Review requested from @sharkdp by @bxff on 2026-01-17 09:17_

---

_Review requested from @dcreager by @bxff on 2026-01-17 09:17_

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 09:19_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

The percentage of diagnostics emitted that were expected errors held steady at 70.24%. The percentage of expected errors that received a diagnostic held steady at 60.04%.

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 649 | 649 | +0 |  |
| False Positives | 275 | 275 | +0 |  |
| False Negatives | 432 | 432 | +0 |  |
| Total Diagnostics | 924 | 924 | +0 |  |
| Precision | 70.24% | 70.24% | +0.00% |  |
| Recall | 60.04% | 60.04% | +0.00% |  |



### False positives removed

<details>

| Location | Name | Message |
|----------|------|---------|
| [aliases_type_statement.py:10:52](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_type_statement.py#L10) | invalid-type-arguments | Too many type arguments: expected 2, got 3 |
| [aliases_typealiastype.py:23:5](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/aliases_typealiastype.py#L23) | invalid-argument-type | Argument to class `TypeAliasType` is incorrect: Expected `tuple[TypeVar \| ParamSpec \| typing_extensions.TypeVarTuple, ...]`, found `tuple[TypeVar, TypeVar, ParamSpec, typing.TypeVarTuple]` |


</details>

### False positives added

<details>

| Location | Name | Message |
|----------|------|---------|
| [generics_defaults.py:88:39](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/generics_defaults.py#L88) | invalid-legacy-type-variable | The `default` parameter of `typing.TypeVarTuple` was added in Python 3.13 |
| [generics_defaults.py:99:53](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/generics_defaults.py#L99) | invalid-legacy-type-variable | The `default` parameter of `typing.TypeVarTuple` was added in Python 3.13 |


</details>



---

_Comment by @astral-sh-bot[bot] on 2026-01-17 09:20_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
+ python/egglog/deconstruct.py:24:25: error[invalid-legacy-type-variable] The `default` parameter of `typing.TypeVarTuple` was added in Python 3.13
- python/egglog/thunk.py:50:31: error[invalid-argument-type] Argument is incorrect: Expected `(...) -> TypeVar`, found `(...) -> T@fn`
+ python/egglog/thunk.py:50:31: error[invalid-argument-type] Argument is incorrect: Expected `(TypeVar, /) -> TypeVar`, found `(...) -> T@fn`
- Found 1485 diagnostics
+ Found 1486 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/axis_map.py:146:39: error[invalid-type-arguments] Too many type arguments to class `IndexHierarchy`: expected between 0 and 1, got 2
- static_frame/core/batch.py:774:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocCompound[Batch]`, found `InterGetItemLocCompound[Batch | Bottom[Series[Any, Any]]]`
- static_frame/core/batch.py:778:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocCompound[Batch]`, found `InterGetItemILocCompound[Batch | Bottom[Series[Any, Any]]]`
- static_frame/core/batch.py:782:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceGetItemBLoc[Batch]`, found `InterfaceGetItemBLoc[Batch | Bottom[Series[Any, Any]]]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Frame[Any, Any, object]] | ... omitted 8 union elements, object_]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Frame[Any, Any, object]] | ... omitted 8 union elements, object_ | Self@iloc]`
- static_frame/core/container_util.py:750:23: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:750:23: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:750:40: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:750:40: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:752:36: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:752:36: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:752:71: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:752:71: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:762:24: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:762:24: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:770:24: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:770:24: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:771:25: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:771:25: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:774:24: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:774:24: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:775:25: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:775:25: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:776:25: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:776:25: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:780:21: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:780:21: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:785:25: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:785:25: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:790:23: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:790:23: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:790:42: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:790:42: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:792:36: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:792:36: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:792:73: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:792:73: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:800:24: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:800:24: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:802:25: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:802:25: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:812:24: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:812:24: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:813:25: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:813:25: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:814:25: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:814:25: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:819:24: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:819:24: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:820:25: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:820:25: warning[possibly-missing-attribute] Attribute `reindex` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:821:25: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:821:25: warning[possibly-missing-attribute] Attribute `_index` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:822:27: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:822:27: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:826:21: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:826:21: warning[possibly-missing-attribute] Attribute `values` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
- static_frame/core/container_util.py:838:27: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Frame) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:838:27: warning[possibly-missing-attribute] Attribute `_columns` may be missing on object of type `(Any & ndarray[tuple[object, ...], dtype[object]]) | (Any & Top[Series[Any, Any]]) | (Any & Top[Frame[Any, Any, object]]) | ndarray[tuple[Any, ...], dtype[Unknown]]`
+ static_frame/core/container_util.py:1032:22: error[call-non-callable] Object of type `object` is not callable
+ static_frame/core/container_util.py:1088:16: error[call-non-callable] Object of type `object` is not callable
+ static_frame/core/container_util.py:1326:49: error[invalid-argument-type] Argument to bound method `values_at_depth` is incorrect: Expected `int | list[int]`, found `int | (list[int] & Top[integer[Any]]) | (ndarray[Any, dtype[signedinteger[_64Bit]]] & Top[integer[Any]])`
+ static_frame/core/frame.py:246:40: error[invalid-legacy-type-variable] The `default` parameter of `typing.TypeVarTuple` was added in Python 3.13
- static_frame/core/frame.py:2688:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Iterable[Any]] | TypeBlocks | Frame | Series[Any, Any]`, found `object`
+ static_frame/core/frame.py:2688:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Iterable[Any]] | TypeBlocks | Frame[Any, Any, @Todo] | Series[Any, Any]`, found `object`
- static_frame/core/frame.py:3628:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/frame.py:3707:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceGetItemBLoc[Series[Any, Any]]`, found `InterfaceGetItemBLoc[Series[Any, Any] | Bottom[Frame[Any, Any, object]]]`
- static_frame/core/frame.py:3712:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> Self@drop) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/frame.py:3712:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/frame.py:3713:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_loc(key: Hashable) -> Self@drop) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3713:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3714:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_getitem(key: Hashable) -> Self@drop) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3714:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_getitem(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3720:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_iloc_mask(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> Frame[Any, Any, @Todo]` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/frame.py:3721:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_loc_mask(key: Hashable) -> Frame[Any, Any, @Todo]` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3722:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_getitem_mask(key: Hashable) -> Frame[Any, Any, @Todo]` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3728:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@masked_array._extract_iloc_masked_array(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> MaskedArray[Any, Any]) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/frame.py:3728:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@masked_array._extract_iloc_masked_array(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> MaskedArray[Any, Any]` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/frame.py:3729:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@masked_array._extract_loc_masked_array(key: Hashable) -> MaskedArray[Any, Any]) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3729:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@masked_array._extract_loc_masked_array(key: Hashable) -> MaskedArray[Any, Any]` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3730:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@masked_array._extract_getitem_masked_array(key: Hashable) -> MaskedArray[Any, Any]) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3730:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@masked_array._extract_getitem_masked_array(key: Hashable) -> MaskedArray[Any, Any]` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3737:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@assign._extract_iloc_assign(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> FrameAssignILoc) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/frame.py:3737:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@assign._extract_iloc_assign(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> FrameAssignILoc` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/frame.py:3738:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@assign._extract_loc_assign(key: Hashable) -> FrameAssignILoc) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3738:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@assign._extract_loc_assign(key: Hashable) -> FrameAssignILoc` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3739:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@assign._extract_getitem_assign(key: Hashable) -> FrameAssignILoc) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3739:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@assign._extract_getitem_assign(key: Hashable) -> FrameAssignILoc` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3799:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[Frame[Any, Any, @Todo]]`, found `InterfaceString[Frame[Any, Any, @Todo] | Bottom[Series[Any, Any]]]`
+ static_frame/core/frame.py:3825:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[Frame[Any, Any, @Todo]]`, found `InterfaceDatetime[Frame[Any, Any, @Todo] | Bottom[Series[Any, Any]]]`
+ static_frame/core/frame.py:3873:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[Frame[Any, Any, @Todo]]`, found `InterfaceRe[Frame[Any, Any, @Todo] | Bottom[Series[Any, Any]]]`
+ static_frame/core/frame.py:6773:21: error[call-non-callable] Object of type `object` is not callable
- static_frame/core/frame.py:8288:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:9327:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:9337:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/frame.py:10249:38: error[invalid-type-arguments] Too many type arguments to class `FrameHE`: expected between 0 and 2, got 3
+ static_frame/core/frame.py:10257:38: error[invalid-type-arguments] Too many type arguments to class `FrameHE`: expected between 0 and 2, got 3
+ static_frame/core/frame.py:10455:24: warning[possibly-missing-attribute] Attribute `_blocks` may be missing on object of type `Unknown | Series[Any, Any] | Frame[Any, Any, @Todo]`
- static_frame/core/frame.py:10537:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:10618:83: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/frame.py:10635:39: error[invalid-type-arguments] Too many type arguments to class `FrameHE`: expected between 0 and 2, got 3
+ static_frame/core/generic_aliases.py:21:53: error[invalid-type-arguments] Too many type arguments to class `FrameHE`: expected between 0 and 2, got 3
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Index[Any]] | Any | Bottom[Series[Any, Any]] | ... omitted 9 union elements, Any]`
+ static_frame/core/index.py:601:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `Self@iter_label` does not satisfy constraints (`Frame[Any, Any, @Todo]`, `Series[Any, Any]`, `Bus[Any]`, `Quilt`, `Yarn[Any]`) of type variable `TContainerAny`
+ static_frame/core/index.py:601:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Frame[Any, Any, @Todo]`, found `Self@iter_label`
- static_frame/core/index.py:763:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceString[ndarray[Any, Any]]`, found `InterfaceString[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:781:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceDatetime[ndarray[Any, Any]]`, found `InterfaceDatetime[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
- static_frame/core/index.py:801:16: error[invalid-return-type] Return type does not match returned value: expected `InterfaceRe[ndarray[Any, Any]]`, found `InterfaceRe[ndarray[Any, Any] | Bottom[Series[Any, Any]]]`
+ static_frame/core/index_hierarchy.py:245:42: error[invalid-legacy-type-variable] The `default` parameter of `typing.TypeVarTuple` was added in Python 3.13
+ static_frame/core/index_hierarchy.py:1322:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[IndexHierarchy[@Todo], Any]`, found `InterGetItemILocReduces[Any | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 9 union elements, Any]`
+ static_frame/core/index_hierarchy.py:1352:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `Self@iter_label` does not satisfy constraints (`Frame[Any, Any, @Todo]`, `Series[Any, Any]`, `Bus[Any]`, `Quilt`, `Yarn[Any]`) of type variable `TContainerAny`
+ static_frame/core/index_hierarchy.py:1352:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Frame[Any, Any, @Todo]`, found `Self@iter_label`
- static_frame/core/index_hierarchy.py:3336:82: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/join.py:302:91: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/join.py:307:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/node_fill_value.py:107:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Frame[Any, Any, @Todo]`, found `TVContainer_co@InterfaceFillValue`
+ static_frame/core/node_fill_value.py:107:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy constraints (`Frame[Any, Any, @Todo]`, `IndexHierarchy[@Todo]`) of type variable `TVContainer_co`
- static_frame/core/node_fill_value.py:154:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/node_fill_value.py:269:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:269:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:277:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:277:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:285:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:285:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:301:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:301:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:309:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:309:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:317:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:317:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:325:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:325:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:333:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:333:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:341:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:341:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:349:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:349:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:357:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:357:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:365:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:365:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:373:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:373:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:381:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:381:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:389:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:389:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:397:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:397:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:405:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:405:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:413:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:413:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:422:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:422:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:430:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:430:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:438:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Frame[TVIndex@Frame, TVColumns@Frame, TVDtypes@Frame]` of type variable `Self`
+ static_frame/core/node_fill_value.py:438:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@InterfaceFillValue` does not satisfy upper bound `Series[TVIndex@Series, TVDtype@Series]` of type variable `Self`
+ static_frame/core/node_fill_value.py:454:16: error[invalid-argument-type] Argument to bound method `_ufunc_binary_operator` is incorrect: Argument type `TVContainer_co@Interfac

... (truncated 360 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Review requested from @MichaReiser by @bxff on 2026-01-17 09:40_

---

_Review requested from @Gankra by @bxff on 2026-01-17 09:40_

---

_Label `ty` added by @AlexWaygood on 2026-01-17 11:35_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-17 11:37_

---

_Comment by @astral-sh-bot[bot] on 2026-01-17 11:42_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 118 | 2 | 54 |
| `invalid-type-arguments` | 63 | 0 | 0 |
| `not-subscriptable` | 0 | 52 | 0 |
| `unused-ignore-comment` | 2 | 47 | 0 |
| `possibly-missing-attribute` | 9 | 2 | 35 |
| `invalid-return-type` | 12 | 7 | 13 |
| `invalid-assignment` | 0 | 2 | 6 |
| `invalid-parameter-default` | 0 | 0 | 7 |
| `unsupported-operator` | 6 | 0 | 1 |
| `unresolved-attribute` | 3 | 1 | 2 |
| `call-non-callable` | 3 | 0 | 0 |
| `invalid-legacy-type-variable` | 3 | 0 | 0 |
| `invalid-await` | 2 | 0 | 0 |
| `type-assertion-failure` | 0 | 2 | 0 |
| **Total** | **221** | **115** | **118** |


**[Full report with detailed diff](https://2196a745.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://2196a745.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @codspeed-hq[bot] on 2026-01-17 14:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/bxff%3Afeat%2Ftypevartuple-support?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 4.37%**

<sub>Comparing <code>bxff:feat/typevartuple-support</code> (08cc8fe) with <code>main</code> (ca57b25)</sub>



### Summary

`❌ 1` regressed benchmark  
`✅ 52` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/bxff%3Afeat%2Ftypevartuple-support?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/bxff%3Afeat%2Ftypevartuple-support?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 22 s | 23 s | -4.37% |


---
