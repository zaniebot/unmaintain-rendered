```yaml
number: 21556
title: "[ty] Fix #1607 (experiment)"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: david/fix-1607
created_at: 2025-11-21T13:14:03Z
updated_at: 2025-11-24T09:19:23Z
url: https://github.com/astral-sh/ruff/pull/21556
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Fix #1607 (experiment)

---

_Pull request opened by @sharkdp on 2025-11-21 13:14_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-11-21 13:14_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-21 13:14_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 13:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-21 13:15:49.974672476 +0000
+++ new-output.txt	2025-11-21 13:15:54.221695030 +0000
@@ -1,25 +1,29 @@
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:465:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_recursive.py`: `infer_definition_types(Id(11539)): execute: too many cycle iterations`
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a885bb4/src/function/execute.rs:465:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(19ea3)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
 _directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
+aliases_explicit.py:41:9: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_explicit.py:52:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_explicit.py:53:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_explicit.py:54:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_explicit.py:56:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_explicit.py:59:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
+aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
+aliases_explicit.py:67:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+aliases_explicit.py:68:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+aliases_explicit.py:69:9: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+aliases_explicit.py:70:9: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+aliases_explicit.py:71:9: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
+aliases_explicit.py:102:5: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+aliases_implicit.py:54:9: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_implicit.py:63:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_implicit.py:64:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_implicit.py:65:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_implicit.py:67:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_implicit.py:70:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
-aliases_implicit.py:71:5: error[type-assertion-failure] Type `list[bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `Unknown`
+aliases_implicit.py:76:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+aliases_implicit.py:77:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+aliases_implicit.py:78:9: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+aliases_implicit.py:79:9: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+aliases_implicit.py:80:9: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
+aliases_implicit.py:81:9: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `str`
 aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown | <class 'int'> | <class 'str'>]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
 aliases_implicit.py:109:9: error[invalid-type-form] Variable of type `list[<class 'int'> | Unknown]` is not allowed in a type expression
@@ -30,6 +34,7 @@
 aliases_implicit.py:118:10: error[invalid-type-form] Variable of type `Literal["int"]` is not allowed in a type expression
 aliases_implicit.py:119:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
 aliases_implicit.py:133:6: error[call-non-callable] Object of type `UnionType` is not callable
+aliases_implicit.py:135:5: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 aliases_newtype.py:11:8: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["user"]`
 aliases_newtype.py:12:14: error[invalid-assignment] Object of type `Literal[42]` is not assignable to `UserId`
 aliases_newtype.py:18:11: error[invalid-assignment] Object of type `<NewType pseudo-class 'UserId'>` is not assignable to `type`
@@ -72,9 +77,6 @@
 aliases_type_statement.py:79:7: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
 aliases_type_statement.py:80:7: error[too-many-positional-arguments] Too many positional arguments: expected 2, got 3
 aliases_type_statement.py:80:37: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
-aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
-aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar, typing.TypeVar]'>` with no `__class_getitem__` method
 annotations_forward_refs.py:47:10: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
 annotations_forward_refs.py:54:11: error[fstring-type-annotation] Type expressions cannot use f-strings
@@ -125,6 +127,12 @@
 callables_annotation.py:58:5: error[invalid-type-form] Special form `typing.Callable` expected exactly two arguments (parameter types and return type)
 callables_annotation.py:58:14: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
 callables_annotation.py:157:20: error[invalid-assignment] Object of type `Proto7` is not assignable to `Proto6`
+callables_annotation.py:171:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+callables_annotation.py:172:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+callables_annotation.py:174:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+callables_annotation.py:175:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+callables_annotation.py:188:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+callables_annotation.py:189:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 callables_kwargs.py:24:5: error[type-assertion-failure] Type `int` does not match asserted type `@Todo(`Unpack[]` special form)`
 callables_kwargs.py:32:9: error[type-assertion-failure] Type `str` does not match asserted type `@Todo(`Unpack[]` special form)`
 callables_kwargs.py:35:5: error[type-assertion-failure] Type `str` does not match asserted type `@Todo(`Unpack[]` special form)`
@@ -176,6 +184,8 @@
 callables_subtyping.py:195:22: error[invalid-assignment] Object of type `StrKwargs6` is not assignable to `IntKwargs6`
 callables_subtyping.py:196:22: error[invalid-assignment] Object of type `IntStrKwargs6` is not assignable to `Standard6`
 callables_subtyping.py:197:22: error[invalid-assignment] Object of type `StrKwargs6` is not assignable to `Standard6`
+callables_subtyping.py:211:40: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+callables_subtyping.py:213:9: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 callables_subtyping.py:236:23: error[invalid-assignment] Object of type `NoDefaultArg8` is not assignable to `DefaultArg8`
 callables_subtyping.py:237:23: error[invalid-assignment] Object of type `NoX8` is not assignable to `DefaultArg8`
 callables_subtyping.py:240:25: error[invalid-assignment] Object of type `NoX8` is not assignable to `NoDefaultArg8`
@@ -446,10 +456,10 @@
 generics_defaults_referential.py:36:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
 generics_defaults_referential.py:37:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
 generics_defaults_referential.py:94:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
-generics_defaults_referential.py:95:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar[int, list[int]]'>`
-generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, typing.TypeVar]`
-generics_defaults_specialization.py:27:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
+generics_defaults_referential.py:95:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar[int, list[Z1]]'>`
+generics_defaults_referential.py:96:1: error[type-assertion-failure] Type `Bar[int, list[int]]` does not match asserted type `Bar[int, list[Z1]]`
+generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, DefaultStrT]`
+generics_defaults_specialization.py:30:1: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 generics_defaults_specialization.py:45:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
 generics_paramspec_basic.py:10:1: error[invalid-paramspec] The name of a `ParamSpec` (`NotIt`) must match the name of the variable it is assigned to (`WrongName`)
 generics_paramspec_basic.py:23:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `P@func1`
@@ -594,27 +604,42 @@
 generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Type `tuple[int, bool, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_overloads.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:50:23: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:50:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:45:14: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
+generics_typevartuple_specialization.py:45:40: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:59:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:92:14: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
+generics_typevartuple_specialization.py:92:42: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `Unknown`
 generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Type `tuple[Any, *tuple[Any, ...]]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_specialization.py:102:20: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
+generics_typevartuple_specialization.py:103:21: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+generics_typevartuple_specialization.py:127:5: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+generics_typevartuple_specialization.py:130:14: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 3
 generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func7, T2@func7]`
+generics_typevartuple_specialization.py:134:14: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
+generics_typevartuple_specialization.py:134:33: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 3
+generics_typevartuple_specialization.py:134:59: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 4
 generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Type `tuple[tuple[str], bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Type `tuple[tuple[str, bool], int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
+generics_typevartuple_specialization.py:143:14: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 4
 generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func9, T2@func9, T3@func9]`
+generics_typevartuple_specialization.py:147:15: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 3
+generics_typevartuple_specialization.py:147:41: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 4
 generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
 generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Type `tuple[tuple[bool], str, int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
-generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `@Todo(Support for `typing.GenericAlias` instances in type expressions)`
-generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:153:8: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
+generics_typevartuple_specialization.py:156:24: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
+generics_typevartuple_specialization.py:156:55: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
+generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `Unknown`
+generics_typevartuple_specialization.py:163:8: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
 generics_typevartuple_unpack.py:17:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_upper_bound.py:37:1: error[type-assertion-failure] Type `list[int]` does not match asserted type `list[Unknown | int]`
 generics_upper_bound.py:38:1: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[Unknown | int]`
@@ -625,20 +650,6 @@
 generics_variance.py:14:6: error[invalid-legacy-type-variable] A `TypeVar` cannot be both covariant and contravariant
 generics_variance.py:26:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T_co@ImmutableList]`
 generics_variance.py:57:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `B_co@func`
-generics_variance.py:175:25: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:175:35: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:179:29: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:179:39: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:183:21: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:183:27: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:187:25: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:187:31: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:191:33: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:191:43: error[non-subscriptable] Cannot subscript object of type `<class 'Co[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:191:49: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:196:5: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:196:15: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
-generics_variance.py:196:25: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
 generics_variance_inference.py:19:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T3@ClassA`
 generics_variance_inference.py:24:33: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int, int, int]`
 generics_variance_inference.py:25:37: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int | float, int | float, int]`
@@ -894,8 +905,6 @@
 specialtypes_type.py:139:5: error[type-assertion-failure] Type `type[Any]` does not match asserted type `type`
 specialtypes_type.py:143:1: error[unresolved-attribute] Object of type `typing.Type` has no attribute `unknown`
 specialtypes_type.py:145:1: error[unresolved-attribute] Class `type` has no attribute `unknown`
-specialtypes_type.py:152:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
-specialtypes_type.py:156:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
 specialtypes_type.py:160:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 specialtypes_type.py:161:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 specialtypes_type.py:169:21: error[invalid-assignment] Object of type `type` is not assignable to `type[int]`
@@ -1001,5 +1010,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1003 diagnostics
+Found 1012 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-21 13:22_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

**Failing projects**:

| Project | Old Status | New Status | Old Return Code | New Return Code |
|---------|------------|------------|-----------------|------------------|
| `xarray` | success | abnormal exit | `1` | `101` |
| `prefect` | success | abnormal exit | `1` | `101` |
| `arviz` | success | abnormal exit | `1` | `101` |

**Diagnostic changes:**

| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 438 | 531 | 1,405 |
| `invalid-argument-type` | 1,542 | 375 | 124 |
| `too-many-positional-arguments` | 578 | 0 | 0 |
| `no-matching-overload` | 348 | 3 | 0 |
| `unused-ignore-comment` | 12 | 307 | 0 |
| `invalid-return-type` | 254 | 3 | 9 |
| `possibly-missing-attribute` | 150 | 2 | 84 |
| `unresolved-attribute` | 59 | 71 | 30 |
| `unsupported-operator` | 82 | 0 | 21 |
| `invalid-assignment` | 59 | 9 | 16 |
| `non-subscriptable` | 16 | 27 | 8 |
| `invalid-context-manager` | 0 | 0 | 32 |
| `not-iterable` | 22 | 0 | 7 |
| `invalid-await` | 22 | 2 | 3 |
| `invalid-type-form` | 1 | 13 | 11 |
| `invalid-parameter-default` | 2 | 0 | 3 |
| `call-non-callable` | 1 | 2 | 0 |
| `redundant-cast` | 3 | 0 | 0 |
| `unknown-argument` | 2 | 0 | 0 |
| `parameter-already-assigned` | 0 | 1 | 0 |
| **Total** | **3,591** | **1,346** | **1,753** |

**[Full report with detailed diff](https://david-fix-1607.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1607.ecosystem-663.pages.dev/timing))




---

_Closed by @sharkdp on 2025-11-24 09:19_

---
