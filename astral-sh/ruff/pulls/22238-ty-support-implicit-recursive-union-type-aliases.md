```yaml
number: 22238
title: "[ty] support implicit recursive union type aliases"
type: pull_request
state: open
author: mtshiba
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: implicit-recursive-union
created_at: 2025-12-29T05:10:16Z
updated_at: 2026-01-07T09:42:07Z
url: https://github.com/astral-sh/ruff/pull/22238
synced_at: 2026-01-10T16:30:32Z
```

# [ty] support implicit recursive union type aliases

---

_Pull request opened by @mtshiba on 2025-12-29 05:10_

## Summary

Part of https://github.com/astral-sh/ty/issues/1738

This PR allows implicit (or PEP-613) union type aliases to be self-referential.
To achieve this, two new Rust structs are added: `LazyUnionTypeInstance` and `ImplicitTypeAliasType`.
As a result, the previous `UnionTypeInstance` is now called `EagerUnionTypeInstance`. Also, an `Implicit(ImplicitTypeAliasType)` variant is added to `TypeAliasType`.

First, a union type instance is created as `EagerUnionTypeInstance`. Then, when registering a binding, it is checked whether it can be promoted to `LazyUnionTypeInstance`. The promotion criteria include whether it is a valid union type and whether it is a non-generic type (this should be relaxed in the future, but is marked as TODO for this PR).

```python
JsonValue = Union[int, float, bool, str, None, dict[str, "JsonValue"], list["JsonValue"]]
# ↑ Lazy      ↑ Eager                                     ↑ Lazy             ↑ Lazy
typing.cast(Union[int, str], x)
#             ↑ Eager
```

`LazyUnionTypeInstance` is converted to `ImplicitTypeAliasType` in a type context. `ImplicitTypeAliasType` behaves almost identically to PEP-695 type alias (except that it doesn't have a generic context yet).
There are other `KnownInstance`s that can be converted to `ImplicitTypeAliasType` in a type context, but in this PR we will focus on union type instances.

Also, this change means that implicit type aliases are now shown with their name rather than their expanded type.
This is preferable, assuming there is reasonable intent behind the naming of type aliases.

Fixes https://github.com/astral-sh/ty/issues/1883 (with https://github.com/astral-sh/ruff/pull/22241)
Fixes https://github.com/astral-sh/ty/issues/1998

### Performance analysis

https://github.com/astral-sh/ruff/pull/22238#issuecomment-3698301314

### Ecosystem analysis

The new false positive errors appears to be due to astral-sh/ty#2015. Type aliases should basically behave the same as value types, but as in astral-sh/ty#2015, inconsistencies in behavior have been observed in implementations, which may also affect performance.

## Test Plan

New corpus test
mdtest updated

---

_Label `ty` added by @mtshiba on 2025-12-29 05:10_

---

_Label `ecosystem-analyzer` added by @mtshiba on 2025-12-29 05:10_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 05:12_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2026-01-07 09:32:44.627070962 +0000
+++ new-output.txt	2026-01-07 09:32:44.967072566 +0000
@@ -5,7 +5,7 @@
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
 aliases_explicit.py:67:9: error[not-subscriptable] Cannot subscript non-generic type
-aliases_explicit.py:68:9: error[not-subscriptable] Cannot subscript non-generic type: `<class 'list[int | None]'>` is already specialized
+aliases_explicit.py:68:9: error[not-subscriptable] Cannot subscript non-generic type: `<class 'list[GoodTypeAlias2]'>` is already specialized
 aliases_explicit.py:69:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_explicit.py:70:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_explicit.py:71:24: error[invalid-type-arguments] Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`
@@ -13,7 +13,7 @@
 aliases_explicit.py:102:5: error[not-subscriptable] Cannot subscript non-generic type
 aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
 aliases_implicit.py:76:9: error[not-subscriptable] Cannot subscript non-generic type
-aliases_implicit.py:77:9: error[not-subscriptable] Cannot subscript non-generic type: `<class 'list[int | None]'>` is already specialized
+aliases_implicit.py:77:9: error[not-subscriptable] Cannot subscript non-generic type: `<class 'list[GoodTypeAlias2]'>` is already specialized
 aliases_implicit.py:78:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_implicit.py:79:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_implicit.py:80:24: error[invalid-type-arguments] Type argument for `ParamSpec` must be either a list of types, `ParamSpec`, `Concatenate`, or `...`
@@ -32,7 +32,7 @@
 aliases_newtype.py:11:8: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["user"]`
 aliases_newtype.py:12:14: error[invalid-assignment] Object of type `Literal[42]` is not assignable to `UserId`
 aliases_newtype.py:18:11: error[invalid-assignment] Object of type `<NewType pseudo-class 'UserId'>` is not assignable to `type`
-aliases_newtype.py:23:16: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `<NewType pseudo-class 'UserId'>`
+aliases_newtype.py:23:16: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `_ClassInfo`, found `<NewType pseudo-class 'UserId'>`
 aliases_newtype.py:26:21: error[invalid-base] Cannot subclass an instance of NewType
 aliases_newtype.py:35:1: error[invalid-newtype] The name of a `NewType` (`BadName`) must match the name of the variable it is assigned to (`GoodName`)
 aliases_newtype.py:41:6: error[invalid-type-form] `GoodNewType1` is a `NewType` and cannot be specialized
@@ -42,12 +42,19 @@
 aliases_newtype.py:61:38: error[invalid-newtype] invalid base for `typing.NewType`: type `TD1`
 aliases_newtype.py:63:15: error[invalid-newtype] Wrong number of arguments in `NewType` creation, expected 2, found 3
 aliases_newtype.py:65:38: error[invalid-newtype] invalid base for `typing.NewType`: type `Any`
+aliases_recursive.py:19:12: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | int | float | complex]` is not assignable to `Json`
+aliases_recursive.py:20:12: error[invalid-assignment] Object of type `list[Unknown | int | float | complex]` is not assignable to `Json`
+aliases_recursive.py:38:22: error[invalid-assignment] Object of type `tuple[Literal[1], tuple[Literal["1"], Literal[1]], tuple[Literal[1], tuple[Literal[1], list[Unknown | int]]]]` is not assignable to `RecursiveTuple`
+aliases_recursive.py:39:22: error[invalid-assignment] Object of type `tuple[Literal[1], list[Unknown | int]]` is not assignable to `RecursiveTuple`
+aliases_recursive.py:50:24: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | list[Unknown | int]]` is not assignable to `RecursiveMapping`
+aliases_recursive.py:51:24: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | int | list[Unknown | int]]` is not assignable to `RecursiveMapping`
+aliases_recursive.py:52:24: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | int | dict[Unknown | str, Unknown | str | int | list[Unknown | int]]]` is not assignable to `RecursiveMapping`
 aliases_type_statement.py:10:52: error[invalid-type-arguments] Too many type arguments: expected 2, got 3
 aliases_type_statement.py:17:1: error[unresolved-attribute] Object of type `TypeAliasType` has no attribute `bit_count`
 aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
 aliases_type_statement.py:23:7: error[unresolved-attribute] Object of type `TypeAliasType` has no attribute `other_attrib`
 aliases_type_statement.py:26:18: error[invalid-base] Invalid class base with type `TypeAliasType`
-aliases_type_statement.py:31:22: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `TypeAliasType`
+aliases_type_statement.py:31:22: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `_ClassInfo`, found `TypeAliasType`
 aliases_type_statement.py:37:22: error[invalid-type-form] Function calls are not allowed in type expressions
 aliases_type_statement.py:38:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_type_statement.py:39:22: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
@@ -923,12 +930,12 @@
 tuples_type_compat.py:33:10: error[invalid-assignment] Object of type `tuple[int, ...]` is not assignable to `tuple[int]`
 tuples_type_compat.py:43:22: error[invalid-assignment] Object of type `tuple[int, ...]` is not assignable to `tuple[int]`
 tuples_type_compat.py:62:26: error[invalid-assignment] Object of type `tuple[int, ...]` is not assignable to `tuple[int, int]`
-tuples_type_compat.py:75:9: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
-tuples_type_compat.py:80:9: error[type-assertion-failure] Type `tuple[str, str] | tuple[int, int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
-tuples_type_compat.py:85:9: error[type-assertion-failure] Type `tuple[int, str, int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
-tuples_type_compat.py:101:13: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
-tuples_type_compat.py:106:13: error[type-assertion-failure] Type `tuple[str, str] | tuple[int, int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
-tuples_type_compat.py:111:13: error[type-assertion-failure] Type `tuple[int, str, int]` does not match asserted type `tuple[int] | tuple[str, str] | tuple[int, *tuple[str, ...], int]`
+tuples_type_compat.py:75:9: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `Func5Input`
+tuples_type_compat.py:80:9: error[type-assertion-failure] Type `tuple[str, str] | tuple[int, int]` does not match asserted type `Func5Input`
+tuples_type_compat.py:85:9: error[type-assertion-failure] Type `tuple[int, str, int]` does not match asserted type `Func5Input`
+tuples_type_compat.py:101:13: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `Func6Input`
+tuples_type_compat.py:106:13: error[type-assertion-failure] Type `tuple[str, str] | tuple[int, int]` does not match asserted type `Func6Input`
+tuples_type_compat.py:111:13: error[type-assertion-failure] Type `tuple[int, str, int]` does not match asserted type `Func6Input`
 tuples_type_compat.py:126:13: error[type-assertion-failure] Type `tuple[int | str, str]` does not match asserted type `tuple[int | str, int | str]`
 tuples_type_compat.py:129:13: error[type-assertion-failure] Type `tuple[int | str, int]` does not match asserted type `tuple[int | str, int | str]`
 tuples_type_compat.py:157:6: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[""], Literal[""]]` is not assignable to `tuple[int, str]`
@@ -1023,4 +1030,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1025 diagnostics
+Found 1032 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-29 07:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/recipes.py:1098:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], mod: None = None) -> int | float | complex, (base: _SupportsPow2[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3NoneOnly[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3[_E_contra@pow, _M_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: _M_contra@pow) -> _T_co@pow, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float, mod: None = None) -> Any, (base: _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], exp: int | float | complex, mod: None = None) -> int | float | complex]`
+ more_itertools/recipes.py:1098:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `(int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], int | float | complex | _SupportsPow2[Any, Any] | _SupportsPow3[Any, Any, Any], /) -> int | float | complex | ... omitted 3 union elements`, found `Overload[(base: int, exp: int, mod: int) -> int, (base: int, exp: Literal[0], mod: None = None) -> Literal[1], (base: int, exp: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], mod: None = None) -> int, (base: int, exp: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], mod: None = None) -> int | float, (base: int, exp: int, mod: None = None) -> Any, (base: Literal[1, 2, 3, 4, 5, ... omitted 20 literals], exp: int | float, mod: None = None) -> int | float, (base: Literal[-1, -2, -3, -4, -5, ... omitted 15 literals], exp: int | float, mod: None = None) -> int | float | complex, (base: int | float, exp: int, mod: None = None) -> int | float, (base: int | float, exp: int | float | complex | _SupportsSomeKindOfPow, mod: None = None) -> Any, (base: int | float | complex, exp: int | float | complex | _SupportsSomeKindOfPow, mod: None = None) -> int | float | complex, (base: _SupportsPow2[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3NoneOnly[_E_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: None = None) -> _T_co@pow, (base: _SupportsPow3[_E_contra@pow, _M_contra@pow, _T_co@pow], exp: _E_contra@pow, mod: _M_contra@pow) -> _T_co@pow, (base: _SupportsSomeKindOfPow, exp: int | float, mod: None = None) -> Any, (base: _SupportsSomeKindOfPow, exp: int | float | complex, mod: None = None) -> int | float | complex]`

attrs (https://github.com/python-attrs/attrs)
- src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ src/attr/validators.py:175:24: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- tests/test_converters.py:239:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:239:16: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_converters.py:240:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:240:16: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_converters.py:309:24: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:309:24: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_converters.py:319:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:319:16: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_converters.py:320:16: error[unresolved-attribute] Object of type `((Any, /) -> Any) | Converter[Any, Any]` has no attribute `converter`
+ tests/test_converters.py:320:16: error[unresolved-attribute] Object of type `_ConverterType` has no attribute `converter`
- tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
+ tests/test_validators.py:185:68: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None]`
- tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
+ tests/test_validators.py:219:56: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None]`
- typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None]`
+ typing-examples/mypy.py:250:64: error[invalid-argument-type] Argument to function `matches_re` is incorrect: Expected `((str | bytes, str | bytes, int, /) -> Match[str | bytes] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None]`

packaging (https://github.com/pypa/packaging)
+ src/packaging/markers.py:139:46: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Variable | Value | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any]`
+ src/packaging/markers.py:142:46: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Variable | Value | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any]`
+ src/packaging/markers.py:144:12: error[invalid-return-type] Return type does not match returned value: expected `list[Divergent] | MarkerAtom | str`, found `tuple[Variable | Value | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any], Op | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any], Variable | Value | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any]]`
+ src/packaging/markers.py:178:26: warning[possibly-missing-attribute] Attribute `serialize` may be missing on object of type `MarkerVar | Op | tuple[MarkerVar, Op, MarkerVar] | Sequence[Any]`
- Found 23 diagnostics
+ Found 27 diagnostics

janus (https://github.com/aio-libs/janus)
- janus/__init__.py:714:18: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ janus/__init__.py:714:18: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- janus/__init__.py:714:36: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ janus/__init__.py:714:36: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- janus/__init__.py:717:24: error[invalid-argument-type] Argument to function `heappop` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ janus/__init__.py:717:24: error[invalid-argument-type] Argument to function `heappop` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`

anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:360:34: error[invalid-argument-type] Argument to function `getcoroutinestate` is incorrect: Expected `Coroutine[Any, Any, Any]`, found `Generator[Future[object] | None, None, Unknown] | Coroutine[Any, Any, Unknown]`
+ src/anyio/_backends/_asyncio.py:360:34: error[invalid-argument-type] Argument to function `getcoroutinestate` is incorrect: Expected `Coroutine[Any, Any, Any]`, found `Generator[_TaskYieldType, None, Unknown] | Coroutine[Any, Any, Unknown]`
+ src/anyio/_backends/_trio.py:1090:28: error[no-matching-overload] No overload of function `fspath` matches arguments
- src/anyio/_backends/_trio.py:1098:30: error[invalid-argument-type] Argument to function `convert_item` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `str | bytes | (Sequence[str | bytes | PathLike[str] | PathLike[bytes]] & PathLike[object]) | PathLike[str] | PathLike[bytes]`
+ src/anyio/_backends/_trio.py:1098:30: error[invalid-argument-type] Argument to function `convert_item` is incorrect: Expected `StrOrBytesPath`, found `str | bytes | PathLike[str] | PathLike[bytes] | (Sequence[StrOrBytesPath] & PathLike[object])`
- Found 93 diagnostics
+ Found 94 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/client.py:1596:29: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:1921:37: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2160:60: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2173:60: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2608:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2616:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2621:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2629:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2666:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:2674:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:3176:36: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
+ aioredis/client.py:3190:48: error[invalid-argument-type] Argument to function `list_or_args` is incorrect: Argument type `KeysT` does not satisfy upper bound `_StringLikeT` of type variable `_KeyT`
- aioredis/client.py:3430:34: error[invalid-assignment] Object of type `KeysView[object]` is not assignable to `Sequence[bytes | str | memoryview[int]] | AbstractSet[AnyKeyT@_zaggregate]`
+ aioredis/client.py:3430:34: error[invalid-assignment] Object of type `KeysView[object]` is not assignable to `Sequence[_StringLikeT] | AbstractSet[AnyKeyT@_zaggregate]`
- aioredis/client.py:4114:55: error[invalid-assignment] Object of type `dict[bytes | str | memoryview[int], Any | None]` is not assignable to `dict[bytes | str | memoryview[int], (dict[str, str], /) -> Awaitable[None]]`
+ aioredis/client.py:4114:55: error[invalid-assignment] Object of type `dict[bytes | str | memoryview[int], Any | None]` is not assignable to `dict[_StringLikeT, (dict[str, str], /) -> Awaitable[None]]`
- aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[int] | str | ... omitted 4 union elements`, found `(@Todo & ~bytes) | int | list[bytes | memoryview[int] | str | ... omitted 5 union elements] | ... omitted 4 union elements`
+ aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `EncodableT | ResponseError | None`, found `(@Todo & ~bytes) | int | list[bytes | memoryview[int] | str | ... omitted 5 union elements] | ... omitted 4 union elements`
- Found 29 diagnostics
+ Found 41 diagnostics

nionutils (https://github.com/nion-software/nionutils)
+ nion/utils/Geometry.py:328:29: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int | float, int | float], (index: Literal[1]) -> tuple[int | float, int | float]]) | Overload[(index: SupportsIndex, /) -> tuple[int | float, int | float], (index: slice[Any, Any, Any], /) -> tuple[tuple[int | float, int | float], ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatRectTuple`
+ nion/utils/Geometry.py:328:29: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int, int], (index: Literal[1]) -> tuple[int, int]]) | Overload[(index: SupportsIndex, /) -> tuple[int, int], (index: slice[Any, Any, Any], /) -> tuple[tuple[int, int], ...]]` cannot be called with key of type `Literal[0]` on object of type `IntRectTuple`
+ nion/utils/Geometry.py:328:42: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int | float, int | float], (index: Literal[1]) -> tuple[int | float, int | float]]) | Overload[(index: SupportsIndex, /) -> tuple[int | float, int | float], (index: slice[Any, Any, Any], /) -> tuple[tuple[int | float, int | float], ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatRectTuple`
+ nion/utils/Geometry.py:328:42: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int, int], (index: Literal[1]) -> tuple[int, int]]) | Overload[(index: SupportsIndex, /) -> tuple[int, int], (index: slice[Any, Any, Any], /) -> tuple[tuple[int, int], ...]]` cannot be called with key of type `Literal[0]` on object of type `IntRectTuple`
+ nion/utils/Geometry.py:328:57: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int | float, int | float], (index: Literal[1]) -> tuple[int | float, int | float]]) | Overload[(index: SupportsIndex, /) -> tuple[int | float, int | float], (index: slice[Any, Any, Any], /) -> tuple[tuple[int | float, int | float], ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatRectTuple`
+ nion/utils/Geometry.py:328:57: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int, int], (index: Literal[1]) -> tuple[int, int]]) | Overload[(index: SupportsIndex, /) -> tuple[int, int], (index: slice[Any, Any, Any], /) -> tuple[tuple[int, int], ...]]` cannot be called with key of type `Literal[1]` on object of type `IntRectTuple`
+ nion/utils/Geometry.py:328:70: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int | float, int | float], (index: Literal[1]) -> tuple[int | float, int | float]]) | Overload[(index: SupportsIndex, /) -> tuple[int | float, int | float], (index: slice[Any, Any, Any], /) -> tuple[tuple[int | float, int | float], ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatRectTuple`
+ nion/utils/Geometry.py:328:70: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int, int], (index: Literal[1]) -> tuple[int, int]]) | Overload[(index: SupportsIndex, /) -> tuple[int, int], (index: slice[Any, Any, Any], /) -> tuple[tuple[int, int], ...]]` cannot be called with key of type `Literal[1]` on object of type `IntRectTuple`
+ nion/utils/Geometry.py:343:44: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatSizeTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatSizeTuple`
+ nion/utils/Geometry.py:343:44: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntSizeTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[1]` on object of type `IntSizeTuple`
+ nion/utils/Geometry.py:343:65: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatSizeTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatSizeTuple`
+ nion/utils/Geometry.py:343:65: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntSizeTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[0]` on object of type `IntSizeTuple`
+ nion/utils/Geometry.py:348:14: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int | float, int | float], (index: Literal[1]) -> tuple[int | float, int | float]]) | Overload[(index: SupportsIndex, /) -> tuple[int | float, int | float], (index: slice[Any, Any, Any], /) -> tuple[tuple[int | float, int | float], ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatRectTuple`
+ nion/utils/Geometry.py:348:35: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int | float, int | float], (index: Literal[1]) -> tuple[int | float, int | float]]) | Overload[(index: SupportsIndex, /) -> tuple[int | float, int | float], (index: slice[Any, Any, Any], /) -> tuple[tuple[int | float, int | float], ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatRectTuple`
+ nion/utils/Geometry.py:348:58: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int | float, int | float], (index: Literal[1]) -> tuple[int | float, int | float]]) | Overload[(index: SupportsIndex, /) -> tuple[int | float, int | float], (index: slice[Any, Any, Any], /) -> tuple[tuple[int | float, int | float], ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatRectTuple`
+ nion/utils/Geometry.py:348:83: error[invalid-argument-type] Method `__getitem__` of type `(Overload[(index: Literal[0]) -> tuple[int | float, int | float], (index: Literal[1]) -> tuple[int | float, int | float]]) | Overload[(index: SupportsIndex, /) -> tuple[int | float, int | float], (index: slice[Any, Any, Any], /) -> tuple[tuple[int | float, int | float], ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatRectTuple`
+ nion/utils/Geometry.py:353:26: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:353:35: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:353:52: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:353:61: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:358:30: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:358:39: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:358:55: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:358:64: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:538:31: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntSizeTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[0]` on object of type `IntSizeTuple`
+ nion/utils/Geometry.py:538:31: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntPointTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[0]` on object of type `IntPointTuple`
+ nion/utils/Geometry.py:538:41: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntSizeTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[1]` on object of type `IntSizeTuple`
+ nion/utils/Geometry.py:538:41: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntPointTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[1]` on object of type `IntPointTuple`
+ nion/utils/Geometry.py:542:31: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntSizeTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[0]` on object of type `IntSizeTuple`
+ nion/utils/Geometry.py:542:31: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntPointTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[0]` on object of type `IntPointTuple`
+ nion/utils/Geometry.py:542:41: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntSizeTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[1]` on object of type `IntSizeTuple`
+ nion/utils/Geometry.py:542:41: error[invalid-argument-type] Method `__getitem__` of type `(bound method IntPointTuple.__getitem__(index: int) -> int) | Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal[1]` on object of type `IntPointTuple`
+ nion/utils/Geometry.py:981:33: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatSizeTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatSizeTuple`
+ nion/utils/Geometry.py:981:33: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:981:43: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatSizeTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatSizeTuple`
+ nion/utils/Geometry.py:981:43: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:985:33: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatSizeTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatSizeTuple`
+ nion/utils/Geometry.py:985:33: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[0]` on object of type `FloatPointTuple`
+ nion/utils/Geometry.py:985:43: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatSizeTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatSizeTuple`
+ nion/utils/Geometry.py:985:43: error[invalid-argument-type] Method `__getitem__` of type `(bound method FloatPointTuple.__getitem__(index: int) -> int | float) | Overload[(index: SupportsIndex, /) -> int | float, (index: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` cannot be called with key of type `Literal[1]` on object of type `FloatPointTuple`
- Found 23 diagnostics
+ Found 63 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/binary_distribution.py:2075:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `Sized`
+ lib/spack/spack/binary_distribution.py:2075:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `StrPath`, found `Sized`
- lib/spack/spack/cmd/blame.py:158:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `object`
+ lib/spack/spack/cmd/blame.py:158:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `StrPath`, found `object`
- lib/spack/spack/cmd/blame.py:168:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `object`
+ lib/spack/spack/cmd/blame.py:168:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `StrPath`, found `object`
- lib/spack/spack/cmd/commands.py:563:34: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/cmd/commands.py:563:34: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `object` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/cmd/providers.py:59:40: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/cmd/providers.py:59:40: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/config.py:1153:35: error[invalid-argument-type] Argument to function `isfile` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `object`
+ lib/spack/spack/config.py:1153:35: error[invalid-argument-type] Argument to function `isfile` is incorrect: Expected `FileDescriptorOrPath`, found `object`
- lib/spack/spack/config.py:1155:34: error[invalid-argument-type] Argument to function `isdir` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `object`
+ lib/spack/spack/config.py:1155:34: error[invalid-argument-type] Argument to function `isdir` is incorrect: Expected `FileDescriptorOrPath`, found `object`
- lib/spack/spack/directives.py:298:15: error[invalid-assignment] Object of type `list[Unknown | ~str]` is not assignable to `((type[PackageBase] | Dependency, /) -> None) | str | list[((type[PackageBase] | Dependency, /) -> None) | str] | None`
+ lib/spack/spack/directives.py:298:15: error[invalid-assignment] Object of type `list[Unknown | ~str]` is not assignable to `PatchesType | None`
+ lib/spack/spack/directives.py:299:37: error[not-iterable] Object of type `PatchesType | None` is not iterable
- lib/spack/spack/directives.py:299:37: error[not-iterable] Object of type `((type[PackageBase] | Dependency, /) -> None) | str | list[((type[PackageBase] | Dependency, /) -> None) | str] | None` may not be iterable
- lib/spack/spack/directives.py:323:26: error[not-iterable] Object of type `((type[PackageBase] | Dependency, /) -> None) | str | list[((type[PackageBase] | Dependency, /) -> None) | str] | None` may not be iterable
- lib/spack/spack/directives.py:324:9: error[call-non-callable] Object of type `str` is not callable
+ lib/spack/spack/directives.py:323:26: error[not-iterable] Object of type `PatchesType | None` is not iterable
- lib/spack/spack/installer.py:1309:27: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ lib/spack/spack/installer.py:1309:27: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `StrOrBytesPath`, found `Unknown | None | str`
- lib/spack/spack/installer.py:1322:23: error[invalid-argument-type] Argument to function `rename` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | None | str`
+ lib/spack/spack/installer.py:1322:23: error[invalid-argument-type] Argument to function `rename` is incorrect: Expected `StrOrBytesPath`, found `Unknown | None | str`
- lib/spack/spack/llnl/util/filesystem.py:168:5: error[invalid-assignment] Object of type `def copystat(src, dst, follow_symlinks=True) -> Unknown` is not assignable to attribute `copystat` of type `def copystat(src: str | bytes | PathLike[str] | PathLike[bytes], dst: str | bytes | PathLike[str] | PathLike[bytes], *, follow_symlinks: bool = True) -> None`
+ lib/spack/spack/llnl/util/filesystem.py:168:5: error[invalid-assignment] Object of type `def copystat(src, dst, follow_symlinks=True) -> Unknown` is not assignable to attribute `copystat` of type `def copystat(src: StrOrBytesPath, dst: StrOrBytesPath, *, follow_symlinks: bool = True) -> None`
- lib/spack/spack/llnl/util/filesystem.py:1668:35: error[invalid-argument-type] Argument to function `exists` is incorrect: Expected `int | str | bytes | PathLike[str] | PathLike[bytes]`, found `Unknown | Sized`
+ lib/spack/spack/llnl/util/filesystem.py:1668:35: error[invalid-argument-type] Argument to function `exists` is incorrect: Expected `FileDescriptorOrPath`, found `Unknown | Sized`
- lib/spack/spack/llnl/util/filesystem.py:1674:25: error[invalid-argument-type] Argument to function `move` is incorrect: Expected `str | PathLike[str]`, found `Unknown | Sized`
+ lib/spack/spack/llnl/util/filesystem.py:1674:25: error[invalid-argument-type] Argument to function `move` is incorrect: Expected `StrPath`, found `Unknown | Sized`
+ lib/spack/spack/llnl/util/filesystem.py:1830:14: error[no-matching-overload] No overload of function `abspath` matches arguments
- lib/spack/spack/package_base.py:1531:33: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/package_base.py:1531:33: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/package_base.py:1539:84: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/package_base.py:1539:84: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/solver/asp.py:2650:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `(Unknown & ~None) | ConcreteVersion` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
+ lib/spack/spack/solver/asp.py:2650:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitOrStandardVersion, list[Provenance]].__getitem__(key: GitOrStandardVersion, /) -> list[Provenance]` cannot be called with key of type `(Unknown & ~None) | ConcreteVersion` on object of type `dict[GitOrStandardVersion, list[Provenance]]`
- lib/spack/spack/solver/asp.py:3363:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `(Unknown & ~AlwaysFalsy) | (ConcreteVersion & ~AlwaysFalsy)` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
+ lib/spack/spack/solver/asp.py:3363:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitOrStandardVersion, list[Provenance]].__getitem__(key: GitOrStandardVersion, /) -> list[Provenance]` cannot be called with key of type `(Unknown & ~AlwaysFalsy) | (ConcreteVersion & ~AlwaysFalsy)` on object of type `dict[GitOrStandardVersion, list[Provenance]]`
- lib/spack/spack/test/cmd/deprecate.py:89:45: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/test/cmd/deprecate.py:89:45: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/test/cmd/deprecate.py:90:41: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/test/cmd/deprecate.py:90:41: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Unknown | Spec` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/test/directory_layout.py:116:30: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | str | list[str] | tuple[str, ...]`, found `SpecHashDescriptor`
+ lib/spack/spack/test/directory_layout.py:116:30: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | DepTypes`, found `SpecHashDescriptor`
- lib/spack/spack/test/directory_layout.py:129:37: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | str | list[str] | tuple[str, ...]`, found `SpecHashDescriptor`
+ lib/spack/spack/test/directory_layout.py:129:37: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | DepTypes`, found `SpecHashDescriptor`
- lib/spack/spack/test/directory_layout.py:134:70: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | str | list[str] | tuple[str, ...]`, found `SpecHashDescriptor`
+ lib/spack/spack/test/directory_layout.py:134:70: error[invalid-argument-type] Argument to bound method `copy` is incorrect: Expected `int | DepTypes`, found `SpecHashDescriptor`
- lib/spack/spack/test/llnl/util/lock.py:192:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `str | bytes | PathLike[str] | PathLike[bytes]`, found `None | Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:192:23: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `StrOrBytesPath`, found `None | Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1091:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1093:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1102:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1108:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1112:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1121:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/llnl/util/lock.py:1125:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `(() -> bool) | None | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1091:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1093:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1102:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1108:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1112:44: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1121:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
+ lib/spack/spack/test/llnl/util/lock.py:1125:48: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `ReleaseFnType | ContextManager[Unknown]`, found `def write(t, v, tb) -> Unknown`
- lib/spack/spack/test/spec_dag.py:814:32: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `str | list[str] | tuple[str, ...]`, found `def all(iterable: Iterable[object], /) -> bool`
+ lib/spack/spack/test/spec_dag.py:814:32: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `DepTypes`, found `def all(iterable: Iterable[object], /) -> bool`
- lib/spack/spack/test/spec_dag.py:818:29: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `str | list[str] | tuple[str, ...]`, found `None`
+ lib/spack/spack/test/spec_dag.py:818:29: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `DepTypes`, found `None`
- lib/spack/spack/test/spec_dag.py:820:29: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `str | list[str] | tuple[str, ...]`, found `list[str | None]`
+ lib/spack/spack/test/spec_dag.py:820:29: error[invalid-argument-type] Argument to function `canonicalize` is incorrect: Expected `DepTypes`, found `list[Unknown | None]`
- lib/spack/spack/test/spec_semantics.py:2106:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec | Unknown` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/test/spec_semantics.py:2106:19: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec | Unknown` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
- lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: int = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: int = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/vendor/attr/validators.py:186:25: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `(Overload[(pattern: str | Pattern[str], string: str, flags: _FlagsType = 0) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: Buffer, flags: _FlagsType = 0) -> Match[bytes] | None] & ~AlwaysTruthy & ~AlwaysFalsy) | (str & ~AlwaysFalsy)` does not satisfy upper bound `SupportsRichComparison` of type variable `SupportsRichComparisonT`
+ var/spack/test_repos/spack_repo/builtin_mock/build_systems/compiler.py:139:20: error[no-matching-overload] No overload of function `basename` matches arguments
- Found 4296 diagnostics
+ Found 4297 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_internal/build_env.py:58:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `str | None`
+ src/pip/_internal/build_env.py:58:27: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `StrPath`, found `str | None`
- src/pip/_internal/metadata/pkg_resources.py:128:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IResourceProvider | None`, found `InMemoryMetadata`
+ src/pip/_internal/metadata/pkg_resources.py:128:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MetadataType`, found `InMemoryMetadata`
- src/pip/_internal/metadata/pkg_resources.py:149:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IResourceProvider | None`, found `InMemoryMetadata`
+ src/pip/_internal/metadata/pkg_resources.py:149:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_MetadataType`, found `InMemoryMetadata`
+ src/pip/_internal/utils/logging.py:214:39: error[invalid-argument-type] Argument to function `_is_broken_pipe_error` is incorrect: Expected `type[BaseException]`, found `(type[BaseException] & ~AlwaysFalsy) | (BaseException & ~AlwaysFalsy) | TracebackType`
+ src/pip/_internal/utils/logging.py:214:50: error[invalid-argument-type] Argument to function `_is_broken_pipe_error` is incorrect: Expected `BaseException`, found `(type[BaseException] & ~AlwaysFalsy) | (BaseException & ~AlwaysFalsy) | TracebackType`
+ src/pip/_vendor/distlib/compat.py:1048:17: error[invalid-assignment] Object of type `type[BaseException] | BaseException | TracebackType | None` is not assignable to attribute `__cause__

... (truncated 8832 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 07:03_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-key` | 284 | 2,279 | 0 |
| `invalid-argument-type` | 319 | 23 | 2,164 |
| `invalid-assignment` | 24 | 5 | 165 |
| `no-matching-overload` | 168 | 0 | 0 |
| `type-assertion-failure` | 9 | 0 | 111 |
| `possibly-missing-attribute` | 14 | 1 | 91 |
| `invalid-return-type` | 7 | 0 | 88 |
| `unresolved-attribute` | 1 | 1 | 50 |
| `not-subscriptable` | 1 | 39 | 10 |
| `unused-ignore-comment` | 10 | 28 | 0 |
| `unsupported-operator` | 10 | 0 | 19 |
| `not-iterable` | 0 | 3 | 15 |
| `invalid-context-manager` | 1 | 0 | 10 |
| `invalid-parameter-default` | 0 | 0 | 9 |
| `invalid-type-form` | 0 | 0 | 9 |
| `redundant-cast` | 0 | 4 | 1 |
| `invalid-await` | 2 | 0 | 1 |
| `invalid-type-arguments` | 0 | 0 | 3 |
| `missing-typed-dict-key` | 0 | 2 | 0 |
| `call-non-callable` | 0 | 1 | 0 |
| **Total** | **850** | **2,386** | **2,746** |


**[Full report with detailed diff](https://daa420d1.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://daa420d1.ty-ecosystem-ext.pages.dev/timing))



---

_Comment by @codspeed-hq[bot] on 2025-12-29 07:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 7.42%**

<sub>Comparing <code>mtshiba:implicit-recursive-union</code> (882c15a) with <code>main</code> (3b61da0)</sub>



### Summary

`⚡ 2` improved benchmarks  
`❌ 2` regressed benchmarks  
`✅ 49` untouched benchmarks  
`🆕 1` new benchmark  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.9 s | 5.2 s | -5.29% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.9 s | 8.5 s | -7.42% |
| 🆕 | Simulation | [`` ty_micro[recursive_union_type_alias_and_protocol] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Amicro%3A%3Abenchmark_recursive_union_type_alias_and_protocol%3A%3Aty_micro%5Brecursive_union_type_alias_and_protocol%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | N/A | 70 ms | N/A |
| ⚡ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 6.1 ms | 5.8 ms | +5.85% |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3Aimplicit-recursive-union?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.2 s | 1 s | +17.77% |


---

_Comment by @mtshiba on 2025-12-30 04:42_

The codspeed (walltime) results seem to have improved in some cases and worsened in others.
Since instrumented tests showed almost no regressions, it is likely that the changes are due to higher-level compounding effects. For example, whether type alias expansion is eager or lazy may improve the inspection speed of some code while slowing down the inspection speed of other code. Or paths that were previously disabled by `Divergent` may be executed.
I think it would be difficult to improve everything.

---

_Comment by @mtshiba on 2025-12-30 04:57_

mypy and pyright seem to expand type aliases as much as possible, regardless of whether they are implicit or PEP-695 style (in the case of recursive type aliases, mypy seems to omit the recursive part and display it such as `int | list[...]`).
pyrefly does not expand both aliases whenever possible.

---

_Marked ready for review by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @carljm by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @AlexWaygood by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @sharkdp by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @dcreager by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @MichaReiser by @mtshiba on 2026-01-05 15:09_

---

_Review requested from @Gankra by @mtshiba on 2026-01-07 09:08_

---

_Converted to draft by @mtshiba on 2026-01-07 09:28_

---
