```yaml
number: 20730
title: "[ty] Truncate type display for long unions in some situations"
type: pull_request
state: merged
author: InvalidPathException
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: invalid/truncate-long-unions
created_at: 2025-10-07T00:57:11Z
updated_at: 2025-12-18T11:12:52Z
url: https://github.com/astral-sh/ruff/pull/20730
synced_at: 2026-01-12T15:57:08Z
```

# [ty] Truncate type display for long unions in some situations

---

_@InvalidPathException_

## Summary

Fixes [astral-sh/ty#1307](https://github.com/astral-sh/ty/issues/1307)

Unions with length <= 5 are unaffected to minimize test churn
Unions with length > 5 will only display the first 3 elements + "... omitted x union elements"
Here "length" is defined as the number of elements after condensation to literals

Edit: we no longer truncate in revel case. 
Before:

> info: Attempted to call union type `(def f1() -> int) | (def f2(name: str) -> int) | (def f3(a: int, b: int) -> int) | (def f4[T](x: T@f4) -> int) | Literal[5] | (Overload[() -> None, (x: str) -> str]) | (Overload[() -> None, (x: str, y: str) -> str]) | PossiblyNotCallable`

After:

> info: Attempted to call union type `(def f1() -> int) | (def f2(name: str) -> int) | (def f3(a: int, b: int) -> int) | ... omitted 5 union elements`

The below comparisons are outdated, but left here as a reference.

Before:
```reveal_type(x)  # revealed: Literal[1, 2] | A | B | C | D | E | F | G```
```reveal_type(x)  # revealed: Result1A | Result1B | Result2A | Result2B | Result3 | Result4```
After:
```reveal_type(x)  # revealed: Literal[1, 2] | A | B | ... omitted 5 union elements```
```reveal_type(x)  # revealed: Result1A | Result1B | Result2A | ... omitted 3 union elements```

This formatting is consistent with `crates/ty_python_semantic/src/types/call/bind.rs` line 2992

## Test Plan

Cosmetic only, covered and verified by changes in mdtest

---

_Review requested from @carljm by @InvalidPathException on 2025-10-07 00:57_

---

_Review requested from @AlexWaygood by @InvalidPathException on 2025-10-07 00:57_

---

_Review requested from @sharkdp by @InvalidPathException on 2025-10-07 00:57_

---

_Review requested from @dcreager by @InvalidPathException on 2025-10-07 00:57_

---

_Label `ty` added by @AlexWaygood on 2025-10-07 06:38_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-07 06:38_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-07 07:16_

---

_Comment by @github-actions[bot] on 2025-10-07 07:18_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-07 07:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `str | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `str | None`, found `Unknown | str | None | ... omitted 3 union elements`
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `int | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `int | None`, found `Unknown | str | None | ... omitted 3 union elements`
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `bool`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `bool`, found `Unknown | str | None | ... omitted 3 union elements`
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `bool`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_mypy` is incorrect: Expected `bool`, found `Unknown | str | None | ... omitted 3 union elements`
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_pyright` is incorrect: Expected `str | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_pyright` is incorrect: Expected `str | None`, found `Unknown | str | None | ... omitted 3 union elements`
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `RustBuildMode`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `RustBuildMode`, found `Unknown | str | None | ... omitted 3 union elements`
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `str | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_ty` is incorrect: Expected `str | None`, found `Unknown | str | None | ... omitted 3 union elements`
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `RustBuildMode`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `RustBuildMode`, found `Unknown | str | None | ... omitted 3 union elements`
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `str | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `str | None`, found `Unknown | str | None | ... omitted 3 union elements`
- mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `Path | None`, found `Unknown | str | None | int | RustBuildMode | Path`
+ mypy_primer/main.py:71:87: error[invalid-argument-type] Argument to function `setup_pyrefly` is incorrect: Expected `Path | None`, found `Unknown | str | None | ... omitted 3 union elements`

kornia (https://github.com/kornia/kornia)
- kornia/feature/loftr/loftr.py:105:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | int | list[Unknown | int] | list[Unknown | str] | str | float`
+ kornia/feature/loftr/loftr.py:105:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | int | list[Unknown | int] | ... omitted 3 union elements`
- kornia/feature/loftr/loftr.py:105:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | int | list[Unknown | int] | list[Unknown | str] | str | float`
+ kornia/feature/loftr/loftr.py:105:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | int | list[Unknown | int] | ... omitted 3 union elements`
- kornia/feature/loftr/loftr.py:107:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | int | dict[Unknown | str, Unknown | int | list[Unknown | int]] | dict[Unknown | str, Unknown | int | list[Unknown | str] | str] | dict[Unknown | str, Unknown | float | int | str]`
- kornia/feature/loftr/loftr.py:108:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | int | dict[Unknown | str, Unknown | int | list[Unknown | int]] | dict[Unknown | str, Unknown | int | list[Unknown | str] | str] | dict[Unknown | str, Unknown | float | int | str]`
- kornia/feature/loftr/loftr.py:110:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | int | dict[Unknown | str, Unknown | int | list[Unknown | int]] | dict[Unknown | str, Unknown | int | list[Unknown | str] | str] | dict[Unknown | str, Unknown | float | int | str]`
+ kornia/feature/loftr/loftr.py:107:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | ... omitted 4 union elements`
+ kornia/feature/loftr/loftr.py:108:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | ... omitted 4 union elements`
+ kornia/feature/loftr/loftr.py:110:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | ... omitted 4 union elements`

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:285:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:286:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:352:52: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | list[Unknown | PendingResult] | bool | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:353:60: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | dict[Unknown | str, Unknown | str] | list[Unknown | GraphQLError] | ... omitted 3 union elements`
- tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[@Todo] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[@Todo] | None`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[CompletedResult] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[CompletedResult] | None`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:463:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[PendingResult] | None`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[@Todo] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[@Todo] | None`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[CompletedResult] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[CompletedResult] | None`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | bool | list[Unknown | PendingResult] | list[Unknown] | list[Unknown | CompletedResult] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_defer.py:464:63: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | bool | list[Unknown | PendingResult] | ... omitted 3 union elements`
- tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:193:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:194:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:229:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Any]`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str | int] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[GraphQLError] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`
- tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | list[Unknown | str | int] | list[Unknown | GraphQLError] | dict[Unknown | str, Unknown | int]`
+ tests/execution/test_stream.py:230:61: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `Any | list[Unknown | str] | str | ... omitted 3 union elements`

paasta (https://github.com/yelp/paasta)
- paasta_tools/frameworks/native_service_config.py:176:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str] | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | list[@Todo]] | list[@Todo]] | dict[Unknown | str, Unknown | str | list[Unknown | dict[Unknown | str, Unknown | str | bool]]] | list[Unknown | dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown]]]]` is not assignable to `TaskInfo`
+ paasta_tools/frameworks/native_service_config.py:176:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | dict[Unknown | str, Unknown | str] | ... omitted 3 union elements]` is not assignable to `TaskInfo`

ignite (https://github.com/pytorch/ignite)
- tests/ignite/distributed/utils/__init__.py:194:12: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/distributed/utils/__init__.py:194:12: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/distributed/utils/__init__.py:195:12: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/distributed/utils/__init__.py:195:12: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/distributed/utils/test_native.py:416:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | int | float | list[int | float] | list[str] | list[Any]`
+ tests/ignite/distributed/utils/test_native.py:416:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | int | float | ... omitted 3 union elements`
- tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:141:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:141:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:142:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:142:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:192:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:192:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:193:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:193:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_davies_bouldin_score.py:141:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_davies_bouldin_score.py:141:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_davies_bouldin_score.py:142:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_davies_bouldin_score.py:142:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_davies_bouldin_score.py:192:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_davies_bouldin_score.py:192:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_davies_bouldin_score.py:193:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_davies_bouldin_score.py:193:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_silhouette_score.py:141:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_silhouette_score.py:141:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_silhouette_score.py:142:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_silhouette_score.py:142:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_silhouette_score.py:192:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_silhouette_score.py:192:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/clustering/test_silhouette_score.py:193:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/clustering/test_silhouette_score.py:193:27: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_canberra_metric.py:112:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_canberra_metric.py:112:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_canberra_metric.py:113:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_canberra_metric.py:113:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_canberra_metric.py:159:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_canberra_metric.py:159:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_canberra_metric.py:160:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_canberra_metric.py:160:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_fractional_absolute_error.py:106:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_fractional_absolute_error.py:106:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_fractional_absolute_error.py:107:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_fractional_absolute_error.py:107:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_fractional_absolute_error.py:156:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_fractional_absolute_error.py:156:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_fractional_absolute_error.py:157:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_fractional_absolute_error.py:157:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_fractional_bias.py:127:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_fractional_bias.py:127:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_fractional_bias.py:128:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_fractional_bias.py:128:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_fractional_bias.py:133:22: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | list[int | float] | list[str] | list[Any]`
+ tests/ignite/metrics/regression/test_fractional_bias.py:133:22: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | ... omitted 3 union elements`
- tests/ignite/metrics/regression/test_fractional_bias.py:179:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_fractional_bias.py:179:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_fractional_bias.py:180:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_fractional_bias.py:180:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_fractional_bias.py:183:22: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | list[int | float] | list[str] | list[Any]`
+ tests/ignite/metrics/regression/test_fractional_bias.py:183:22: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | ... omitted 3 union elements`
- tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:109:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:109:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:110:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:110:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:115:22: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | list[int | float] | list[str] | list[Any]`
+ tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:115:22: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | ... omitted 3 union elements`
- tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:158:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:158:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:159:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:159:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:162:22: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | list[int | float] | list[str] | list[Any]`
+ tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:162:22: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | ... omitted 3 union elements`
- tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:96:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:96:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:97:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:97:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:143:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:143:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:144:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:144:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_kendall_correlation.py:148:20: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_kendall_correlation.py:148:20: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_kendall_correlation.py:149:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_kendall_correlation.py:149:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_kendall_correlation.py:194:20: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_kendall_correlation.py:194:20: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_kendall_correlation.py:195:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_kendall_correlation.py:195:25: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_manhattan_distance.py:104:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_manhattan_distance.py:104:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_manhattan_distance.py:105:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_manhattan_distance.py:105:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_manhattan_distance.py:152:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_manhattan_distance.py:152:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_manhattan_distance.py:153:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_manhattan_distance.py:153:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_maximum_absolute_error.py:96:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_maximum_absolute_error.py:96:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_maximum_absolute_error.py:97:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_maximum_absolute_error.py:97:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_maximum_absolute_error.py:144:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_maximum_absolute_error.py:144:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_maximum_absolute_error.py:145:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_maximum_absolute_error.py:145:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:131:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:131:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:132:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:132:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:137:27: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | list[int | float] | list[str] | list[Any]`
+ tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:137:27: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | ... omitted 3 union elements`
- tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:181:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:181:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:182:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:182:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:185:27: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | list[int | float] | list[str] | list[Any]`
+ tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:185:27: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | int | float | ... omitted 3 union elements`
- tests/ignite/metrics/regression/test_mean_error.py:87:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_error.py:87:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_error.py:88:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_error.py:88:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_error.py:135:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_error.py:135:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_error.py:136:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_error.py:136:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_normalized_bias.py:107:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_normalized_bias.py:107:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_normalized_bias.py:108:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_normalized_bias.py:108:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_normalized_bias.py:157:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_normalized_bias.py:157:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_mean_normalized_bias.py:158:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_mean_normalized_bias.py:158:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_absolute_error.py:117:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_absolute_error.py:117:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_absolute_error.py:118:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_absolute_error.py:118:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_absolute_error.py:164:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_absolute_error.py:164:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_absolute_error.py:165:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_absolute_error.py:165:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:137:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:137:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:138:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:138:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:184:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:184:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:185:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:185:22: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_relative_absolute_error.py:127:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_relative_absolute_error.py:127:21: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ignite/metrics/regression/test_median_relative_absolute_error.py:128:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` may be missing
+ tests/ignite/metrics/regression/test_median_relative_absolute_error.py:128:16: warning[possibly-missing-attribute] Attribute `cpu` on type `Unknown | int | float | ... omitted 3 union elements` may be missing
- tests/ig...*[Comment body truncated]*

---

_Comment by @github-actions[bot] on 2025-10-07 07:54_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 112 | 0 | 181 |
| `possibly-missing-attribute` | 0 | 0 | 164 |
| `invalid-assignment` | 0 | 0 | 22 |
| `unsupported-operator` | 0 | 0 | 20 |
| `possibly-missing-implicit-call` | 0 | 0 | 6 |
| `invalid-return-type` | 0 | 0 | 4 |
| `invalid-type-form` | 0 | 0 | 3 |
| `invalid-parameter-default` | 0 | 0 | 1 |
| `no-matching-overload` | 0 | 1 | 0 |
| **Total** | **112** | **1** | **401** |


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/exception/control_flow.md`:525 on 2025-10-07 12:11_

So, I actually think we should probably never truncate unions for `reveal_type` calls. A `reveal_type` call is the user explicitly asking us to tell them what the type of something is; it would be annoying if we only then displayed part of the type.

I think it's probably good for the default to be to truncate unions (as you already have it), but I think we need to still have an option where we can force unions to be displayed in full, without truncation. And we need to invoke that option for specific diagnostics such as `reveal_type`. We should be able to do this by extending our existing `DisplaySettings` struct, I think?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/union_call.md_-_Calling_a_union_of_f…_-_Try_to_cover_all_pos…_-_Cover_non-keyword_re…_(707b284610419a54).snap`:89 on 2025-10-07 12:23_

the problem here is that only two elements of this union are not callable (`Literal[5]` and `PossiblyNotCallable`), and they are both truncated away, so it's no longer clear what the problem is. I suppose the ideal display here would be something like

```suggestion
info: Attempted to call union type `(def f1() -> int) | <...3 union elements omitted...> | Literal[5] | <...3 union elements omitted...>`
```

Which indicates that we possibly need to have some way of filtering out union elements that do not satisfy a certain predicate...

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:1223 on 2025-10-07 12:24_

I'm not sure this is possible? If a union has 0 elements, we should eagerly normalize the union into `Type::Never` when constructing the type

```suggestion
        assert_ne!(total_entries, 0);
```

---

_@AlexWaygood requested changes on 2025-10-07 12:24_

Thanks very much! I think there's some subtleties here that we need to think through a bit

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/union_call.md_-_Calling_a_union_of_f…_-_Try_to_cover_all_pos…_-_Cover_non-keyword_re…_(707b284610419a54).snap`:89 on 2025-10-07 15:50_

The previous line does specifically mention `Literal[5]`, so I'm not sure it's critical that the union display be that smart. In general I think when there's an error that applies to only some elements of the union, the diagnostic itself should call out the specific problematic union element(s), we shouldn't rely on union display for that. Showing the union is just additional context.

---

_@carljm reviewed on 2025-10-07 15:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/union_call.md_-_Calling_a_union_of_f…_-_Try_to_cover_all_pos…_-_Cover_non-keyword_re…_(707b284610419a54).snap`:89 on 2025-10-07 15:52_

That's fair

---

_@AlexWaygood reviewed on 2025-10-07 15:52_

---

_@InvalidPathException reviewed on 2025-10-07 18:51_

---

_Review comment by @InvalidPathException on `crates/ty_python_semantic/src/types/display.rs`:1223 on 2025-10-07 18:51_

resolved

---

_@AlexWaygood approved on 2025-10-07 18:55_

Looks excellent, thank you!! Will wait until tomorrow before merging in case anybody else has thoughts

---

_Merged by @AlexWaygood on 2025-10-08 10:21_

---

_Closed by @AlexWaygood on 2025-10-08 10:21_

---

_Comment by @AlexWaygood on 2025-10-08 13:07_

And this had exactly the effect on https://github.com/astral-sh/ruff/pull/20368 that I was hoping for. Some of the new diagnostics on that PR branch are so much better now -- thank you!

<img width="3766" height="656" alt="image" src="https://github.com/user-attachments/assets/7d118782-d176-4b0a-88df-2a754a3f06a3" />


---

_Comment by @flying-sheep on 2025-12-18 11:06_

How do I disable this via config or CLI?

---

_Comment by @AlexWaygood on 2025-12-18 11:12_

> How do I disable this via config or CLI?

There's no way currently, but note that we always display unions without truncation if you call `reveal_type` on an object. Note also that `reveal_type` returns the object passed in, so you can use it in nested expressions: `reveal_type(x)[reveal_type(y)]` etc.

---
