```yaml
number: 21553
title: "[ty] Generic types aliases (implicit and PEP 613)"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/generic-implicit-aliases
created_at: 2025-11-21T09:29:24Z
updated_at: 2025-11-28T19:38:25Z
url: https://github.com/astral-sh/ruff/pull/21553
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Generic types aliases (implicit and PEP 613)

---

_Pull request opened by @sharkdp on 2025-11-21 09:29_

## Summary

Add support for generic PEP 613 type aliases and generic implicit type aliases:
```py
from typing import TypeVar

T = TypeVar("T")
ListOrSet = list[T] | set[T]

def _(xs: ListOrSet[int]):
    reveal_type(xs)  # list[int] | set[int]
```

closes https://github.com/astral-sh/ty/issues/1643
closes https://github.com/astral-sh/ty/issues/1629
closes https://github.com/astral-sh/ty/issues/1596
closes https://github.com/astral-sh/ty/issues/573
closes https://github.com/astral-sh/ty/issues/221

## Typing conformance

```diff
-aliases_explicit.py:52:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_explicit.py:53:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_explicit.py:54:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_explicit.py:56:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_explicit.py:59:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
```

New true negatives :heavy_check_mark: 

```diff
+aliases_explicit.py:41:36: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
-aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
+aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `(...) -> Unknown`
```

These require `ParamSpec`

```diff
+aliases_explicit.py:67:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_explicit.py:68:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_explicit.py:69:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_explicit.py:70:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_explicit.py:71:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_explicit.py:102:20: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
```

New true positives :heavy_check_mark: 

```diff
-aliases_implicit.py:63:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_implicit.py:64:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_implicit.py:65:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_implicit.py:67:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_implicit.py:70:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
-aliases_implicit.py:71:5: error[type-assertion-failure] Type `list[bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
```

New true negatives :heavy_check_mark: 

```diff
+aliases_implicit.py:54:36: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
-aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
+aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `(...) -> Unknown`
```

These require `ParamSpec`

```diff
+aliases_implicit.py:76:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_implicit.py:77:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_implicit.py:78:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_implicit.py:79:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_implicit.py:80:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_implicit.py:81:25: error[invalid-type-arguments] Type `str` is not assignable to upper bound `int | float` of type variable `TFloat@GoodTypeAlias12`
+aliases_implicit.py:135:20: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
```

New true positives :heavy_check_mark: 

```diff
+callables_annotation.py:172:19: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+callables_annotation.py:175:19: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+callables_annotation.py:188:25: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+callables_annotation.py:189:25: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
```

These require `ParamSpec` and `Concatenate`.

```diff
-generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, typing.TypeVar]`
+generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, DefaultStrT]`
```

Favorable diagnostic change :heavy_check_mark: 

```diff
-generics_defaults_specialization.py:27:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
```

New true negative :heavy_check_mark: 

```diff
-generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
+generics_defaults_specialization.py:30:15: error[invalid-type-arguments] Too many type arguments: expected between 0 and 1, got 2
```

Correct new diagnostic :heavy_check_mark: 


```diff
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
```

One of these should apparently be an error, but not of this kind, so this is good :heavy_check_mark: 

```diff
-specialtypes_type.py:152:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
-specialtypes_type.py:156:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
```

Good, those were false positives. :heavy_check_mark: 

I skipped the analysis for everything involving `TypeVarTuple`.

## Ecosystem impact

**[Full report with detailed diff](https://david-generic-implicit-alias.ecosystem-663.pages.dev/diff)**

Previous iterations of this PR showed all kinds of problems. In it's current state, I do not see any large systematic problems, but it is hard to tell with 5k diagnostic changes.

## Performance

* There is a huge 4x regression in `colour-science/colour`, related to [this large file](https://github.com/colour-science/colour/blob/develop/colour/io/luts/tests/test_lut.py) with [many assignments of hard-coded arrays (lists of lists) to `np.NDArray` types](https://github.com/colour-science/colour/blob/83e754c8b6932532f246b9fe1727d14f3600cd83/colour/io/luts/tests/test_lut.py#L701-L781) that we now understand. We now take ~2 seconds to check this file, so definitely not great, but maybe acceptable for now.

## Test Plan

Updated and new Markdown tests

---

_Label `ty` added by @sharkdp on 2025-11-21 09:29_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-21 09:29_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 09:31_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-28 19:17:48.980568399 +0000
+++ new-output.txt	2025-11-28 19:17:52.680561701 +0000
@@ -4,21 +4,26 @@
 _directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_explicit.py:52:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_explicit.py:53:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_explicit.py:54:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_explicit.py:56:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_explicit.py:59:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
+aliases_explicit.py:41:36: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `(...) -> Unknown`
+aliases_explicit.py:60:5: error[type-assertion-failure] Type `(...) -> None` does not match asserted type `@Todo(Callable[..] specialized with ParamSpec)`
+aliases_explicit.py:67:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_explicit.py:68:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_explicit.py:69:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_explicit.py:70:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_explicit.py:71:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
+aliases_explicit.py:102:20: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_implicit.py:63:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_implicit.py:64:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_implicit.py:65:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_implicit.py:67:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_implicit.py:70:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
-aliases_implicit.py:71:5: error[type-assertion-failure] Type `list[bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+aliases_implicit.py:54:36: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `(...) -> Unknown`
+aliases_implicit.py:72:5: error[type-assertion-failure] Type `(...) -> None` does not match asserted type `@Todo(Callable[..] specialized with ParamSpec)`
+aliases_implicit.py:76:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_implicit.py:77:24: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+aliases_implicit.py:78:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_implicit.py:79:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_implicit.py:80:29: error[invalid-type-arguments] Too many type arguments: expected 1, got 2
+aliases_implicit.py:81:25: error[invalid-type-arguments] Type `str` is not assignable to upper bound `int | float` of type variable `TFloat@GoodTypeAlias12`
 aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown | <class 'int'> | <class 'str'>]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
 aliases_implicit.py:109:9: error[invalid-type-form] Variable of type `list[<class 'int'> | Unknown]` is not allowed in a type expression
@@ -29,6 +34,7 @@
 aliases_implicit.py:118:10: error[invalid-type-form] Variable of type `Literal["int"]` is not allowed in a type expression
 aliases_implicit.py:119:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
 aliases_implicit.py:133:6: error[call-non-callable] Object of type `UnionType` is not callable
+aliases_implicit.py:135:20: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 aliases_newtype.py:11:8: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["user"]`
 aliases_newtype.py:12:14: error[invalid-assignment] Object of type `Literal[42]` is not assignable to `UserId`
 aliases_newtype.py:18:11: error[invalid-assignment] Object of type `<NewType pseudo-class 'UserId'>` is not assignable to `type`
@@ -75,7 +81,6 @@
 aliases_type_statement.py:88:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias6`
 aliases_type_statement.py:89:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias7`
 aliases_typealiastype.py:32:7: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `other_attrib`
-aliases_typealiastype.py:39:26: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_typealiastype.py:52:40: error[invalid-type-form] Function calls are not allowed in type expressions
 aliases_typealiastype.py:53:40: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_typealiastype.py:54:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
@@ -92,9 +97,6 @@
 aliases_typealiastype.py:63:42: error[invalid-type-form] Boolean operations are not allowed in type expressions
 aliases_typealiastype.py:64:42: error[invalid-type-form] F-strings are not allowed in type expressions
 aliases_typealiastype.py:66:47: error[unresolved-reference] Name `BadAlias21` used when not defined
-aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
-aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
-aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar, typing.TypeVar]'>` with no `__class_getitem__` method
 annotations_forward_refs.py:47:10: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
 annotations_forward_refs.py:54:11: error[fstring-type-annotation] Type expressions cannot use f-strings
@@ -139,6 +141,10 @@
 callables_annotation.py:58:5: error[invalid-type-form] Special form `typing.Callable` expected exactly two arguments (parameter types and return type)
 callables_annotation.py:58:14: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
 callables_annotation.py:157:20: error[invalid-assignment] Object of type `Proto7` is not assignable to `Proto6`
+callables_annotation.py:172:19: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+callables_annotation.py:175:19: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+callables_annotation.py:188:25: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+callables_annotation.py:189:25: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 callables_kwargs.py:24:5: error[type-assertion-failure] Type `int` does not match asserted type `@Todo(`Unpack[]` special form)`
 callables_kwargs.py:32:9: error[type-assertion-failure] Type `str` does not match asserted type `@Todo(`Unpack[]` special form)`
 callables_kwargs.py:35:5: error[type-assertion-failure] Type `str` does not match asserted type `@Todo(`Unpack[]` special form)`
@@ -467,9 +473,8 @@
 generics_defaults_referential.py:37:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
 generics_defaults_referential.py:94:1: error[type-assertion-failure] Type `type[Bar[Any, list[Any]]]` does not match asserted type `<class 'Bar'>`
 generics_defaults_referential.py:95:1: error[type-assertion-failure] Type `type[Bar[int, list[int]]]` does not match asserted type `<class 'Bar[int, list[int]]'>`
-generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, typing.TypeVar]`
-generics_defaults_specialization.py:27:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
+generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, DefaultStrT]`
+generics_defaults_specialization.py:30:15: error[invalid-type-arguments] Too many type arguments: expected between 0 and 1, got 2
 generics_defaults_specialization.py:45:1: error[type-assertion-failure] Type `type[Bar[str]]` does not match asserted type `<class 'Bar'>`
 generics_paramspec_basic.py:10:1: error[invalid-paramspec] The name of a `ParamSpec` (`NotIt`) must match the name of the variable it is assigned to (`WrongName`)
 generics_paramspec_basic.py:23:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `P@func1`
@@ -617,27 +622,41 @@
 generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Type `tuple[int, bool, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_overloads.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:50:23: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:50:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:45:23: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
+generics_typevartuple_specialization.py:45:51: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:59:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:92:28: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
+generics_typevartuple_specialization.py:92:56: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Type `tuple[Any, *tuple[Any, ...]]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_specialization.py:102:32: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
+generics_typevartuple_specialization.py:103:33: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+generics_typevartuple_specialization.py:127:9: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+generics_typevartuple_specialization.py:130:18: error[invalid-type-arguments] Too many type arguments: expected 0, got 3
 generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func7, T2@func7]`
+generics_typevartuple_specialization.py:134:18: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
+generics_typevartuple_specialization.py:134:37: error[invalid-type-arguments] Too many type arguments: expected 0, got 3
+generics_typevartuple_specialization.py:134:63: error[invalid-type-arguments] Too many type arguments: expected 0, got 4
 generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Type `tuple[tuple[str], bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Type `tuple[tuple[str, bool], int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
+generics_typevartuple_specialization.py:143:18: error[invalid-type-arguments] Too many type arguments: expected 0, got 4
 generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func9, T2@func9, T3@func9]`
+generics_typevartuple_specialization.py:147:19: error[invalid-type-arguments] Too many type arguments: expected 0, got 3
+generics_typevartuple_specialization.py:147:45: error[invalid-type-arguments] Too many type arguments: expected 0, got 4
 generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
 generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Type `tuple[tuple[bool], str, int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
-generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `@Todo(Support for `typing.GenericAlias` instances in type expressions)`
-generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:153:12: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
+generics_typevartuple_specialization.py:156:28: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
+generics_typevartuple_specialization.py:156:59: error[invalid-type-arguments] Too many type arguments: expected 0, got 2
+generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
+generics_typevartuple_specialization.py:163:13: error[invalid-type-arguments] Too many type arguments: expected 0, got 1
 generics_typevartuple_unpack.py:17:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_upper_bound.py:37:1: error[type-assertion-failure] Type `list[int]` does not match asserted type `list[Unknown | int]`
 generics_upper_bound.py:38:1: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[Unknown | int]`
@@ -648,20 +667,6 @@
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
@@ -930,10 +935,6 @@
 specialtypes_type.py:139:5: error[type-assertion-failure] Type `type[Any]` does not match asserted type `type`
 specialtypes_type.py:143:1: error[unresolved-attribute] Object of type `typing.Type` has no attribute `unknown`
 specialtypes_type.py:145:1: error[unresolved-attribute] Class `type` has no attribute `unknown`
-specialtypes_type.py:152:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
-specialtypes_type.py:156:16: error[invalid-type-form] `typing.TypeVar` is not a generic class
-specialtypes_type.py:160:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
-specialtypes_type.py:161:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 specialtypes_type.py:169:21: error[invalid-assignment] Object of type `type` is not assignable to `type[int]`
 specialtypes_type.py:175:16: error[invalid-return-type] Return type does not match returned value: expected `type[T@ClassA]`, found `type`
 tuples_type_compat.py:15:27: error[invalid-assignment] Object of type `tuple[int | float, int | float | complex]` is not assignable to `tuple[int, int]`
@@ -1038,4 +1039,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1040 diagnostics
+Found 1041 diagnostics

```

</details>




---

_@sharkdp reviewed on 2025-11-21 09:32_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/async.md`:82 on 2025-11-21 09:32_

We now understand the [`_FutureLike`](https://github.com/python/typeshed/blob/3e1b90bb2d772781c2b6f318e0f44ef791cf5160/stdlib/asyncio/tasks.pyi#L78-L81) type alias.

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 09:38_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 416 | 510 | 1,474 |
| `invalid-argument-type` | 818 | 347 | 153 |
| `invalid-type-arguments` | 628 | 55 | 4 |
| `no-matching-overload` | 637 | 13 | 0 |
| `unsupported-operator` | 376 | 10 | 19 |
| `unused-ignore-comment` | 11 | 393 | 0 |
| `possibly-missing-attribute` | 156 | 51 | 96 |
| `invalid-return-type` | 254 | 10 | 13 |
| `unresolved-attribute` | 85 | 90 | 37 |
| `invalid-assignment` | 109 | 12 | 23 |
| `non-subscriptable` | 32 | 29 | 8 |
| `invalid-type-form` | 1 | 35 | 2 |
| `invalid-context-manager` | 0 | 0 | 32 |
| `not-iterable` | 22 | 0 | 6 |
| `invalid-await` | 22 | 2 | 3 |
| `invalid-method-override` | 18 | 0 | 2 |
| `invalid-parameter-default` | 9 | 0 | 3 |
| `call-non-callable` | 1 | 7 | 1 |
| `redundant-cast` | 3 | 0 | 0 |
| `invalid-raise` | 2 | 0 | 0 |
| `too-many-positional-arguments` | 0 | 2 | 0 |
| `unresolved-reference` | 0 | 2 | 0 |
| `unsupported-base` | 0 | 2 | 0 |
| `parameter-already-assigned` | 0 | 1 | 0 |
| **Total** | **3,600** | **1,571** | **1,876** |

**[Full report with detailed diff](https://david-generic-implicit-alias.ecosystem-663.pages.dev/diff)** ([timing results](https://david-generic-implicit-alias.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-24 10:29_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-24 10:29_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-24 11:12_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-24 11:13_

---

_Comment by @codspeed-hq[bot] on 2025-11-24 11:31_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fgeneric-implicit-aliases?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21553 will **degrade performances by 77.43%**

<sub>Comparing <code>david/generic-implicit-aliases</code> (db39fa1) with <code>main</code> (594b7b0)</sub>



### Summary

`❌ 7 (👁 7)` regressions  
`✅ 45` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fgeneric-implicit-aliases?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 20.2 s | 89.3 s | -77.43% |
| 👁 | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fgeneric-implicit-aliases?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 56.1 s | 67 s | -16.3% |
| 👁 | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fgeneric-implicit-aliases?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 16.7 s | 19.4 s | -13.95% |
| 👁 | WallTime | [`` multithreaded[altair] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fgeneric-implicit-aliases?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amultithreaded%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.3 s | 1.6 s | -18.13% |
| 👁 | WallTime | [`` small[altair] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fgeneric-implicit-aliases?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Baltair%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 4.4 s | 5.3 s | -17.12% |
| 👁 | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fgeneric-implicit-aliases?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.1 s | 8.1 s | -12.51% |
| 👁 | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Fgeneric-implicit-aliases?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.5 s | 2.8 s | -10.18% |


---

_Renamed from "[ty] Generic implicit types aliases" to "[ty] Generic types aliases (implicit and PEP 613)" by @sharkdp on 2025-11-24 12:15_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-24 12:15_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-24 12:16_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-24 12:48_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-24 12:48_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 12:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_base.py:468:48: error[parameter-already-assigned] Multiple values provided for parameter `unwrites` of bound method `_write`
- Found 15 diagnostics
+ Found 14 diagnostics

attrs (https://github.com/python-attrs/attrs)
- src/attrs/__init__.pyi:67:21: error[unsupported-operator] Operator `|` is unsupported between objects of type `GenericAlias` and `<class 'Sequence[@Todo]'>`
+ tests/test_dunders.py:914:24: error[no-matching-overload] No overload of function `attrib` matches arguments
+ tests/test_functional.py:730:17: error[no-matching-overload] No overload of function `attrib` matches arguments
+ tests/test_make.py:102:13: error[no-matching-overload] No overload of function `attrib` matches arguments
+ tests/test_validators.py:134:24: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["42"]`
+ tests/test_validators.py:298:18: error[no-matching-overload] No overload of function `attrib` matches arguments
+ tests/test_validators.py:634:43: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `member_validator`
+ tests/test_validators.py:635:45: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `iterable_validator`
+ tests/test_validators.py:786:40: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `key_validator`
+ tests/test_validators.py:787:42: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `value_validator`
+ tests/test_validators.py:788:44: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `mapping_validator`
+ tests/test_validators.py:1143:64: error[unresolved-attribute] Object of type `(Any, Attribute[Unknown], Unknown, /) -> Any` has no attribute `exc_types`
- Found 596 diagnostics
+ Found 606 diagnostics

anyio (https://github.com/agronholm/anyio)
+ src/anyio/_backends/_asyncio.py:365:34: error[invalid-argument-type] Argument to function `getcoroutinestate` is incorrect: Expected `Coroutine[Any, Any, Any]`, found `Generator[Future[object] | None, None, Unknown] | Coroutine[Any, Any, Unknown]`
- Found 88 diagnostics
+ Found 89 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
+ src/async_utils/bg_loop.py:115:31: error[invalid-argument-type] Argument to bound method `set_task_factory` is incorrect: Expected `_TaskFactory | None`, found `def eager_task_factory[_T_co](loop: AbstractEventLoop | None, coro: Coroutine[Any, Any, _T_co@eager_task_factory], *, name: str | None = None, context: Context | None = None) -> Task[_T_co@eager_task_factory]`
- Found 29 diagnostics
+ Found 30 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/environment/environment.py:590:24: error[unresolved-attribute] Object of type `Path` has no attribute `startswith`
- lib/spack/spack/package_base.py:897:48: error[invalid-type-form] Variable of type `def all(iterable: Iterable[object], /) -> bool` is not allowed in a type expression
- lib/spack/spack/package_base.py:897:55: error[unresolved-reference] Name `specific` used when not defined
- lib/spack/spack/package_base.py:897:67: error[unresolved-reference] Name `none` used when not defined
- lib/spack/spack/spec.py:2090:24: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/spec.py:2104:24: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/spec.py:2143:24: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/spec.py:2157:24: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/traverse.py:457:20: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/traverse.py:472:20: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/traverse.py:570:20: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/traverse.py:585:20: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/util/executable.py:354:80: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/util/executable.py:419:80: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- lib/spack/spack/util/git.py:31:27: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
- Found 7965 diagnostics
+ Found 7952 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/resolvelib/structs.py:203:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `() -> Iterable[Unknown]`, found `(Iterable[CT@build_iter_view] & (() -> object)) | (() -> Iterable[CT@build_iter_view])`
- Found 600 diagnostics
+ Found 601 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_decor/decorcache.py:74:16: error[invalid-return-type] Return type does not match returned value: expected `typing.TypeVar | ((typing.TypeVar, /) -> typing.TypeVar)`, found `BeartypeableT@beartype`
+ beartype/_decor/decorcache.py:74:16: error[invalid-return-type] Return type does not match returned value: expected `BeartypeableT@BeartypeReturn | ((BeartypeableT@BeartypeConfedDecorator, /) -> BeartypeableT@BeartypeConfedDecorator)`, found `BeartypeableT@beartype | BeartypeableT@BeartypeReturn | ((BeartypeableT@BeartypeConfedDecorator, /) -> BeartypeableT@BeartypeConfedDecorator)`
- beartype/_decor/decorcache.py:130:5: error[invalid-assignment] Invalid subscript assignment with key of type `BeartypeConf` and value of type `def _beartype_confed(obj: BeartypeableT@beartype) -> BeartypeableT@beartype` on object of type `dict[BeartypeConf, (typing.TypeVar, /) -> typing.TypeVar]`
+ beartype/_decor/decorcache.py:130:5: error[invalid-assignment] Invalid subscript assignment with key of type `BeartypeConf` and value of type `def _beartype_confed(obj: BeartypeableT@beartype) -> BeartypeableT@beartype` on object of type `dict[BeartypeConf, (BeartypeableT@BeartypeConfedDecorator, /) -> BeartypeableT@BeartypeConfedDecorator]`
- beartype/_decor/decorcache.py:133:12: error[invalid-return-type] Return type does not match returned value: expected `typing.TypeVar | ((typing.TypeVar, /) -> typing.TypeVar)`, found `def _beartype_confed(obj: BeartypeableT@beartype) -> BeartypeableT@beartype`
+ beartype/_decor/decorcache.py:133:12: error[invalid-return-type] Return type does not match returned value: expected `BeartypeableT@BeartypeReturn | ((BeartypeableT@BeartypeConfedDecorator, /) -> BeartypeableT@BeartypeConfedDecorator)`, found `def _beartype_confed(obj: BeartypeableT@beartype) -> BeartypeableT@beartype`
- beartype/_decor/decormain.py:93:20: error[invalid-return-type] Return type does not match returned value: expected `typing.TypeVar | ((typing.TypeVar, /) -> typing.TypeVar)`, found `BeartypeableT@beartype`
+ beartype/_decor/decormain.py:93:20: error[invalid-return-type] Return type does not match returned value: expected `BeartypeableT@BeartypeReturn | ((BeartypeableT@BeartypeConfedDecorator, /) -> BeartypeableT@BeartypeConfedDecorator)`, found `BeartypeableT@beartype`
- beartype/_decor/decormain.py:102:16: error[invalid-return-type] Return type does not match returned value: expected `typing.TypeVar | ((typing.TypeVar, /) -> typing.TypeVar)`, found `def _beartype_optimized[BeartypeableT](obj: BeartypeableT@_beartype_optimized) -> BeartypeableT@_beartype_optimized`
- beartype/bite/kind/infercallable.py:562:18: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
+ beartype/bite/kind/infercallable.py:562:9: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 500 diagnostics
+ Found 499 diagnostics

black (https://github.com/psf/black)
- src/black/rusty.py:28:27: error[invalid-type-arguments] Type `typing.TypeVar` is not assignable to upper bound `Exception` of type variable `E@Err`
+ src/black/trans.py:297:20: error[invalid-raise] Cannot use object of type `object` as an exception cause
+ src/black/trans.py:306:24: error[invalid-raise] Cannot use object of type `object` as an exception cause
+ src/black/trans.py:466:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
+ src/black/trans.py:481:47: error[invalid-argument-type] Argument to bound method `_merge_string_group` is incorrect: Expected `Line`, found `object`
+ src/black/trans.py:493:13: error[unresolved-attribute] Unresolved attribute `__cause__` on type `object`.
+ src/black/trans.py:494:13: error[invalid-assignment] Object of type `object` is not assignable to attribute `__cause__` of type `BaseException | None`
+ src/black/trans.py:980:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
+ src/black/trans.py:1107:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `(Ok[None] & Top[Err[Unknown]]) | Err[CannotTransform]`
- Found 61 diagnostics
+ Found 68 diagnostics

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/utils.py:492:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 215 diagnostics
+ Found 214 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/execution/async_iterables.py:41:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/execution/execute.py:500:38: error[invalid-await] `@Todo | UndefinedType` is not awaitable
+ src/graphql/execution/execute.py:427:42: error[invalid-await] `Awaitable[GraphQLWrappedResult[dict[str, Any]]] | GraphQLWrappedResult[dict[str, Any]]` is not awaitable
+ src/graphql/execution/execute.py:500:38: error[invalid-await] `Awaitable[GraphQLWrappedResult[Any]] | GraphQLWrappedResult[Any] | UndefinedType` is not awaitable
- src/graphql/execution/execute.py:556:54: error[invalid-argument-type] Argument to function `resolve` is incorrect: Expected `Awaitable[GraphQLWrappedResult[dict[str, Any]]]`, found `@Todo | UndefinedType`
+ src/graphql/execution/execute.py:556:54: error[invalid-argument-type] Argument to function `resolve` is incorrect: Expected `Awaitable[GraphQLWrappedResult[dict[str, Any]]]`, found `Awaitable[GraphQLWrappedResult[Any]] | GraphQLWrappedResult[Any] | UndefinedType`
+ src/graphql/execution/execute.py:655:38: error[invalid-await] `Awaitable[GraphQLWrappedResult[Any]] | GraphQLWrappedResult[Any]` is not awaitable
- src/graphql/execution/execute.py:878:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/graphql/execution/execute.py:868:35: error[invalid-await] `Awaitable[GraphQLWrappedResult[Any]] | GraphQLWrappedResult[Any]` is not awaitable
+ src/graphql/execution/execute.py:1286:35: error[invalid-await] `Awaitable[GraphQLWrappedResult[Any]] | GraphQLWrappedResult[Any]` is not awaitable
+ src/graphql/execution/execute.py:1355:34: error[invalid-await] `Awaitable[GraphQLWrappedResult[dict[str, Any]]] | GraphQLWrappedResult[dict[str, Any]]` is not awaitable
- src/graphql/execution/execute.py:1674:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/execution/execute.py:1746:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/graphql/execution/execute.py:1469:38: error[invalid-await] `Awaitable[GraphQLWrappedResult[dict[str, Any]]] | GraphQLWrappedResult[dict[str, Any]]` is not awaitable
+ src/graphql/execution/execute.py:1673:42: error[invalid-await] `Awaitable[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult` is not awaitable
+ src/graphql/execution/execute.py:1676:21: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult]]` is not assignable to attribute `result` of type `BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult] | (() -> BoxedAwaitableOrValue[ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult])`
+ src/graphql/execution/execute.py:1726:44: error[invalid-await] `Awaitable[GraphQLWrappedResult[dict[str, Any]]] | GraphQLWrappedResult[dict[str, Any]]` is not awaitable
+ src/graphql/execution/execute.py:1828:32: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `BoxedAwaitableOrValue[StreamItemResult] | (() -> BoxedAwaitableOrValue[StreamItemResult])`, found `BoxedAwaitableOrValue[CoroutineType[Any, Any, StreamItemResult]]`
- src/graphql/execution/execute.py:1984:46: error[invalid-await] `@Todo | GraphQLWrappedResult[None]` is not awaitable
+ src/graphql/execution/execute.py:1917:26: error[invalid-await] `Awaitable[StreamItemResult] | StreamItemResult` is not awaitable
- src/graphql/execution/execute.py:2005:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/graphql/execution/execute.py:1984:46: error[invalid-await] `Awaitable[GraphQLWrappedResult[Any]] | GraphQLWrappedResult[Any] | GraphQLWrappedResult[None]` is not awaitable
+ src/graphql/execution/execute.py:2018:34: error[invalid-await] `Awaitable[GraphQLWrappedResult[dict[str, Any]]] | GraphQLWrappedResult[dict[str, Any]]` is not awaitable
- src/graphql/execution/execute.py:2562:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/graphql/execution/execute.py:2552:46: error[invalid-await] `Awaitable[AsyncIterable[Any] | ExecutionResult] | AsyncIterable[Any] | ExecutionResult` is not awaitable
+ src/graphql/execution/execute.py:2638:30: error[invalid-await] `Awaitable[AsyncIterable[Any]] | AsyncIterable[Any]` is not awaitable
+ src/graphql/execution/incremental_graph.py:201:29: error[invalid-argument-type] Argument to bound method `_enqueue` is incorrect: Expected `ReconcilableDeferredGroupedFieldSetResult | NonReconcilableDeferredGroupedFieldSetResult | StreamItemsResult`, found `~Top[Future[Any]]`
+ src/graphql/execution/incremental_graph.py:367:16: error[unresolved-attribute] Object of type `object` has no attribute `item`
+ src/graphql/execution/incremental_graph.py:377:66: error[unresolved-attribute] Object of type `object` has no attribute `errors`
+ src/graphql/execution/incremental_graph.py:380:25: error[unresolved-attribute] Object of type `object` has no attribute `item`
+ src/graphql/execution/incremental_graph.py:381:16: error[unresolved-attribute] Object of type `object` has no attribute `errors`
+ src/graphql/execution/incremental_graph.py:382:31: error[unresolved-attribute] Object of type `object` has no attribute `errors`
+ src/graphql/execution/incremental_graph.py:383:16: error[unresolved-attribute] Object of type `object` has no attribute `incremental_data_records`
+ src/graphql/execution/incremental_graph.py:384:49: error[unresolved-attribute] Object of type `object` has no attribute `incremental_data_records`
+ src/graphql/graphql.py:159:9: error[no-matching-overload] No overload of function `ensure_future` matches arguments
- src/graphql/pyutils/async_reduce.py:47:74: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/pyutils/gather_with_cancel.py:24:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/graphql/type/definition.py:1094:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/benchmarks/test_async_iterable.py:20:18: error[invalid-await] `Awaitable[ExecutionResult] | ExecutionResult` is not awaitable
- tests/execution/test_abstract.py:47:24: error[invalid-await] `ExecutionResult | @Todo` is not awaitable
+ tests/execution/test_abstract.py:47:24: error[invalid-await] `ExecutionResult | Awaitable[ExecutionResult]` is not awaitable
+ tests/execution/test_defer.py:183:24: error[invalid-await] `Awaitable[ExecutionResult | ExperimentalIncrementalExecutionResults] | ExecutionResult | ExperimentalIncrementalExecutionResults` is not awaitable
- tests/execution/test_defer.py:2575:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_defer.py:2599:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/execution/test_lists.py:138:22: error[invalid-await] `Awaitable[ExecutionResult] | ExecutionResult` is not awaitable
+ tests/execution/test_lists.py:175:22: error[invalid-await] `Awaitable[ExecutionResult] | ExecutionResult` is not awaitable
+ tests/execution/test_middleware.py:264:20: error[unresolved-attribute] Object of type `object` has no attribute `data`
+ tests/execution/test_middleware.py:266:20: error[unresolved-attribute] Object of type `object` has no attribute `data`
- tests/execution/test_middleware.py:31:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_middleware.py:59:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_middleware.py:91:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_middleware.py:149:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_middleware.py:189:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_middleware.py:195:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_middleware.py:283:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_middleware.py:298:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_middleware.py:349:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_mutations.py:230:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_mutations.py:271:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_stream.py:2274:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_stream.py:2350:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_stream.py:2425:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_stream.py:2486:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_stream.py:2531:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/execution/test_subscribe.py:288:30: error[invalid-await] `Awaitable[AsyncIterator[ExecutionResult] | ExecutionResult] | AsyncIterator[ExecutionResult] | ExecutionResult` is not awaitable
- tests/execution/test_subscribe.py:223:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_subscribe.py:247:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_subscribe.py:266:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/execution/test_subscribe.py:399:22: error[invalid-await] `Awaitable[ExecutionResult | AsyncIterable[Any]] | ExecutionResult | AsyncIterable[Any]` is not awaitable
+ tests/execution/test_subscribe.py:434:22: error[invalid-await] `Awaitable[ExecutionResult | AsyncIterable[Any]] | ExecutionResult | AsyncIterable[Any]` is not awaitable
+ tests/execution/test_subscribe.py:443:22: error[invalid-await] `Awaitable[ExecutionResult | AsyncIterable[Any]] | ExecutionResult | AsyncIterable[Any]` is not awaitable
+ tests/execution/test_sync.py:64:22: error[invalid-await] `Awaitable[ExecutionResult] | ExecutionResult` is not awaitable
- tests/execution/test_subscribe.py:327:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_subscribe.py:555:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_subscribe.py:617:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_subscribe.py:693:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_subscribe.py:773:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_subscribe.py:866:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_subscribe.py:918:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/execution/test_subscribe.py:1165:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/pyutils/test_async_reduce.py:50:22: error[invalid-await] `Awaitable[Literal["foo"]] | Literal["foo"]` is not awaitable
- tests/type/test_definition.py:340:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_definition.py:547:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_definition.py:664:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_definition.py:939:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_definition.py:946:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_definition.py:1044:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/type/test_validation.py:752:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 438 diagnostics
+ Found 428 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/contrib/check_orphans.py:200:61: error[invalid-argument-type] Argument to function `read_nerve_files` is incorrect: Expected `dict[str, str | None]`, found `dict[str, str]`
- paasta_tools/instance/kubernetes.py:668:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/instance/kubernetes.py:680:30: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/instance/kubernetes.py:701:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/instance/kubernetes.py:703:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/instance/kubernetes.py:723:93: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/instance/kubernetes.py:727:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/instance/kubernetes.py:743:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/instance/kubernetes.py:747:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/metrics/metastatus_lib.py:591:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `(Sequence[typing.TypeVar], /) -> Sequence[typing.TypeVar]`
+ paasta_tools/metrics/metastatus_lib.py:591:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `(Sequence[_GenericNodeT@_GenericNodeSortFunctionT], /) -> Sequence[_GenericNodeT@_GenericNodeSortFunctionT]`
- paasta_tools/metrics/metastatus_lib.py:606:25: error[invalid-assignment] Object of type `Sequence[typing.TypeVar]` is not assignable to `Sequence[_GenericNodeT@group_slaves_by_key_func]`
- paasta_tools/metrics/metastatus_lib.py:606:35: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[typing.TypeVar]`, found `Sequence[_GenericNodeT@group_slaves_by_key_func]`
- paasta_tools/metrics/metastatus_lib.py:608:36: error[no-matching-overload] No overload of function `__new__` matches arguments
- paasta_tools/metrics/metastatus_lib.py:769:41: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `_GenericNodeT@filter_slaves`
- paasta_tools/metrics/metastatus_lib.py:776:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `(Sequence[typing.TypeVar], /) -> Sequence[typing.TypeVar]`
+ paasta_tools/metrics/metastatus_lib.py:776:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `(Sequence[_GenericNodeT@_GenericNodeSortFunctionT], /) -> Sequence[_GenericNodeT@_GenericNodeSortFunctionT]`
- paasta_tools/metrics/metastatus_lib.py:815:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `(Sequence[typing.TypeVar], /) -> Sequence[typing.TypeVar]`
+ paasta_tools/metrics/metastatus_lib.py:815:5: error[invalid-parameter-default] Default value of type `None` is not assignable to annotated parameter type `(Sequence[_GenericNodeT@_GenericNodeSortFunctionT], /) -> Sequence[_GenericNodeT@_GenericNodeSortFunctionT]`
- Found 1112 diagnostics
+ Found 1101 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/fixtures.py:788:45: error[unresolved-attribute] Object of type `((...) -> object) | ((...) -> Generator[object, None, None])` has no attribute `__name__`
+ src/_pytest/fixtures.py:1004:42: error[unresolved-attribute] Object of type `((...) -> FixtureValue@FixtureDef) | ((...) -> Generator[FixtureValue@FixtureDef, None, None])` has no attribute `__name__`
+ src/_pytest/fixtures.py:1087:28: error[invalid-return-type] Return type does not match returned value: expected `FixtureValue@FixtureDef`, found `FixtureValue@FixtureDef | None`
+ src/_pytest/pathlib.py:177:12: error[unresolved-attribute] Object of type `Path` has no attribute `lower`
- Found 429 diagnostics
+ Found 433 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/_exception_handler.py:59:34: error[invalid-await] `@Todo | Response | Awaitable[Response]` is not awaitable
- starlette/_exception_handler.py:59:42: error[invalid-argument-type] Argument is incorrect: Expected `Request`, found `Request | WebSocket`
- starlette/_exception_handler.py:59:42: error[invalid-argument-type] Argument is incorrect: Expected `WebSocket`, found `Request | WebSocket`
- starlette/applications.py:33:19: error[invalid-type-form] Invalid subscript of object of type `UnionType` in type expression
- starlette/middleware/errors.py:176:38: error[invalid-await] `@Todo | Response | Awaitable[Response]` is not awaitable
- starlette/middleware/errors.py:176:51: error[invalid-argument-type] Argument is incorrect: Expected `WebSocket`, found `Request`
- starlette/routing.py:67:51: error[invalid-assignment] Object of type `((Request, /) -> Awaitable[Response] | Response) | partial[Unknown]` is not assignable to `(Request, /) -> Awaitable[Response]`
+ starlette/routing.py:67:51: error[invalid-assignment] Object of type `(((Request, /) -> Awaitable[Response] | Response) & (() -> Awaitable[object])) | partial[Unknown]` is not assignable to `(Request, /) -> Awaitable[Response]`
- starlette/routing.py:590:19: error[invalid-type-form] Invalid subscript of object of type `UnionType` in type expression
- starlette/routing.py:615:36: error[invalid-type-form] Invalid subscript of object of type `UnionType` in type expression
+ starlette/routing.py:624:17: error[invalid-argument-type] Argument to function `asynccontextmanager` is incorrect: Expected `(...) -> AsyncIterator[Unknown]`, found `((Any, /) -> AbstractAsyncContextManager[None, bool | None]) | ((Any, /) -> AbstractAsyncContextManager[Mapping[str, Any], bool | None])`
+ starlette/routing.py:632:17: error[invalid-argument-type] Argument to function `_wrap_gen_lifespan_context` is incorrect: Expected `(Any, /) -> Generator[Any, Any, Any]`, found `((Any, /) -> AbstractAsyncContextManager[None, bool | None]) | ((Any, /) -> AbstractAsyncContextManager[Mapping[str, Any], bool | None])`
- tests/test_applications.py:423:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_applications.py:445:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 216 diagnostics
+ Found 208 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_core/actions/invocation.py:114:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kopf/_core/actions/invocation.py:155:28: error[invalid-argument-type] Argument to function `is_async_fn` is incorrect: Expected `((...) -> @Todo) | None`, found `object`
+ kopf/_core/actions/invocation.py:155:28: error[invalid-argument-type] Argument to function `is_async_fn` is incorrect: Expected `((...) -> object) | None`, found `object`
- kopf/_core/intents/registries.py:276:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `def login_via_pykube(*, logger: Logger | LoggerAdapter[Any], **_: Any) -> ConnectionInfo | None`
+ kopf/_core/intents/registries.py:276:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> object`, found `def login_via_pykube(*, logger: Logger | LoggerAdapter[Any], **_: Any) -> ConnectionInfo | None`
- kopf/_core/intents/registries.py:285:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `def login_via_client(*, logger: Logger | LoggerAdapter[Any], **_: Any) -> ConnectionInfo | None`
+ kopf/_core/intents/registries.py:285:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> object`, found `def login_via_client(*, logger: Logger | LoggerAdapter[Any], **_: Any) -> ConnectionInfo | None`
- kopf/_core/intents/registries.py:297:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `def login_with_kubeconfig(**_: Any) -> ConnectionInfo | None`
+ kopf/_core/intents/registries.py:297:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> object`, found `def login_with_kubeconfig(**_: Any) -> ConnectionInfo | None`
- kopf/_core/intents/registries.py:306:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `def login_with_service_account(**_: Any) -> ConnectionInfo | None`
+ kopf/_core/intents/registries.py:306:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> object`, found `def login_with_service_account(**_: Any) -> ConnectionInfo | None`
- kopf/_core/reactor/subhandling.py:72:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> @Todo`, found `object`
+ kopf/_core/reactor/subhandling.py:72:17: error[invalid-argument-type] Argument is incorrect: Expected `(Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, Unknown, /) -> object`, found `object`
- Found 261 diagnostics
+ Found 260 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- boostedblob/boost.py:544:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ boostedblob/boost.py:577:16: error[invalid-return-type] Return type does not match returned value: expected `NotReady | Exhausted | T@dequeue_underlying`, found `object`
+ boostedblob/boost.py:589:16: error[invalid-return-type] Return type does not match returned value: expected `T@blocking_dequeue_underlying`, found `object`
+ boostedblob/listing.py:300:19: error[unsupported-operator] Operator `/` is unsupported between objects of type `LocalPath` and `LocalPath`
+ boostedblob/listing.py:300:29: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `LocalPath`
- Found 18 diagnostics
+ Found 21 diagnostics

mkosi (https://github.com/systemd/mkosi)
- mkosi/config.py:1690:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 97 diagnostics
+ Found 96 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/AuthenticatedUser.py:757:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/AuthenticatedUser.py:784:73: error[not-iterable] Object of type `list[Label] | _NotSetType` may not be iterable
+ github/AuthenticatedUser.py:790:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/AuthenticatedUser.py:817:73: error[not-iterable] Object of type `list[Label] | _NotSetType` may not be iterable
+ github/AuthenticatedUser.py:823:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/AuthenticatedUser.py:873:31: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/AuthenticatedUser.py:875:32: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Branch.py:211:106: error[not-iterable] Object of type `list[str | tuple[str, int]] | _NotSetType` may not be iterable
+ github/Branch.py:222:34: error[not-iterable] Object of type `list[str | tuple[str, int]] | _NotSetType` may not be iterable
+ github/Branch.py:225:74: error[not-iterable] Object of type `list[str] | _NotSetType` may not be iterable
+ github/Branch.py:378:106: error[not-iterable] Object of type `list[str | tuple[str, int]] | _NotSetType` may not be iterable
+ github/Branch.py:384:30: error[not-iterable] Object of type `list[str | tuple[str, int]] | _NotSetType` may not be iterable
+ github/Branch.py:387:70: error[not-iterable] Object of type `list[str] | _NotSetType` may not be iterable
+ github/BranchProtection.py:169:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `str | _NotSetType`
+ github/CheckRun.py:253:45: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/CheckRun.py:255:47: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Gist.py:259:107: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `dict[str, InputFileContent | None] | _NotSetType`
- github/Issue.py:459:73: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ github/Issue.py:463:32: error[not-iterable] Object of type `list[NamedUser | str] | _NotSetType` may not be iterable
+ github/MainClass.py:553:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `_NotSetType | datetime`
+ github/MainClass.py:645:27: error[no-matching-overload] No overload of bound method `join` matches arguments
+ github/MainClass.py:915:42: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `_NotSetType | (Repository & Unknown)`
+ github/Milestone.py:202:41: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `_NotSetType | date`
+ github/NamedUser.py:460:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `_NotSetType | datetime`
+ github/Organization.py:919:88: error[not-iterable] Object of type `list[Repository] | _NotSetType` may not be iterable
+ github/Organization.py:921:93: error[not-iterable] Object of type `list[Repository] | _NotSetType` may not be iterable
+ github/Organization.py:1013:79: error[not-iterable] Object of type `list[Repository] | _NotSetType` may not be iterable
+ github/Organization.py:1048:85: error[not-iterable] Object of type `list[Repository] | _NotSetType` may not be iterable
+ github/Organization.py:1237:73: error[not-iterable] Object of type `list[Label] | _NotSetType` may not be iterable
+ github/Organization.py:1239:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Organization.py:1438:40: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `NamedUser | _NotSetType`
+ github/Organization.py:1440:53: error[not-iterable] Object of type `list[Team] | _NotSetType` may not be iterable
+ github/PullRequest.py:561:30: warning[possibly-missing-attribute] Attribute `sha` may be missing on object of type `Commit | _NotSetType`
+ github/PullRequest.py:686:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
- github/Repository.py:1716:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/Repository.py:1723:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/Repository.py:3213:103: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ github/Repository.py:1450:41: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
+ github/Repository.py:1452:44: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
+ github/Repository.py:1631:41: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
+ github/Repository.py:1648:44: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `GitTree | _NotSetType`
+ github/Repository.py:1719:44: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `Milestone | _NotSetType`
+ github/Repository.py:1802:45: error[unresolved-attribute] Object of type `_NotSetType & ~date` has no attribute `isoformat`
+ github/Repository.py:1860:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `Issue | _NotSetType`
+ github/Repository.py:2287:19: error[no-matching-overload] No overload of function `quote` matches arguments
+ github/Repository.py:2432:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Repository.py:2434:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Repository.py:2770:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
+ github/Repository.py:2772:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
+ github/Repository.py:2855:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
+ github/Repository.py:2857:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
+ github/Repository.py:2906:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
+ github/Repository.py:2908:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `InputGitAuthor | _NotSetType`
+ github/Repository.py:3210:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `NamedUser | _NotSetType`
+ github/Repository.py:3220:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Repository.py:3225:45: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `(NamedUser & ~str) | (_NotSetType & ~str)`
+ github/Repository.py:3250:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Repository.py:3493:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Repository.py:3897:31: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Repository.py:3899:32: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Repository.py:4217:45: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/Repository.py:4219:47: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `datetime | _NotSetType`
+ github/RepositoryAdvisory.py:284:92: error[invalid-argument-type] Argument to bound method `_validate_vulnerability` is incorrect: Expected `SimpleAdvisoryVulnerability | AdvisoryVulnerability`, found `object`
+ github/RepositoryAdvisory.py:290:71: error[invalid-argument-type] Argument to function `_validate_credit` is incorrect: Expected `SimpleCredit | AdvisoryCredit`, found `object`
+ github/RepositoryAdvisory.py:306:84: error[invalid-argument-type] Argument to function `_to_github_dict` is incorrect: Expected `SimpleAdvisoryVulnerability | AdvisoryVulnerability`, found `object`
+ github/RepositoryAdvisory.py:313:70: error[invalid-argument-type] Argument to function `_to_github_dict` is incorrect: Expected `SimpleCredit | AdvisoryCredit`, found `object`
- Found 296 diagnostics
+ Found 354 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/core/result.py:27:22: error[invalid-type-arguments] Type `typing.TypeVar` is not assignable to upper bound `Exception` of type variable `E@Err`
- src/schemathesis/specs/openapi/negative/mutations.py:145:25: error[invalid-assignment] Object of type `typing.TypeVar` is not assignable to `list[(MutationContext, (SearchStrategy[typing.TypeVar], /) -> typing.TypeVar, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]]`
+ src/schemathesis/specs/openapi/negative/mutations.py:145:25: error[invalid-assignment] Object of type `T@Draw` is not assignable to `list[(MutationContext, (SearchStrategy[T@Draw], /) -> T@Draw, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]]`
- src/schemathesis/specs/openapi/negative/mutations.py:145:30: error[invalid-argument-type] Argument is incorrect: Expected `SearchStrategy[typing.TypeVar]`, found `SearchStrategy[list[(MutationContext, (SearchStrategy[typing.TypeVar], /) -> typing.TypeVar, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]]]`
+ src/schemathesis/specs/openapi/negative/mutations.py:145:30: error[invalid-argument-type] Argument is incorrect: Expected `SearchStrategy[T@Draw]`, found `SearchStrategy[list[(MutationContext, (SearchStrategy[T@Draw], /) -> T@Draw, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]]]`
- src/schemathesis/specs/openapi/negative/mutations.py:152:25: error[invalid-assignment] Object of type `typing.TypeVar` is not assignable to `list[(MutationContext, (SearchStrategy[typing.TypeVar], /) -> typing.TypeVar, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]]`
+ src/schemathesis/specs/openapi/negative/mutations.py:152:25: error[invalid-assignment] Object of type `T@Draw` is not assignable to `list[(MutationContext, (SearchStrategy[T@Draw], /) -> T@Draw, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]]`
- src/schemathesis/specs/openapi/negative/mutations.py:152:30: error[invalid-argument-type] Argument is incorrect: Expected `SearchStrategy[typing.TypeVar]`, found `SearchStrategy[list[(MutationContext, (SearchStrategy[typing.TypeVar], /) -> typing.TypeVar, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]]]`
+ src/schemathesis/specs/openapi/negative/mutations.py:152:30: error[invalid-argument-type] Argument is incorrect: Expected `SearchStrategy[T@Draw]`, found `SearchStrategy[list[(MutationContext, (SearchStrategy[T@Draw], /) -> T@Draw, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]]]`
- src/schemathesis/specs/openapi/negative/mutations.py:162:28: error[call-non-callable] Object of type `TypeVar` is not callable
+ src/schemathesis/specs/openapi/negative/mutations.py:162:28: error[call-non-callable] Object of type `T@Draw` is not callable
- src/schemathesis/specs/openapi/negative/mutations.py:164:60: error[unresolved-attribute] Object of type `typing.TypeVar` has no attribute `is_enabled`
+ src/schemathesis/specs/openapi/negative/mutations.py:164:60: error[unresolved-attribute] Object of type `T@Draw` has no attribute `is_enabled`
- src/schemathesis/specs/openapi/negative/mutations.py:164:89: error[unresolved-attribute] Object of type `(MutationContext, (SearchStrategy[typing.TypeVar], /) -> typing.TypeVar, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]` has no attribute `__name__`
+ src/schemathesis/specs/openapi/negative/mutations.py:164:89: error[unresolved-attribute] Object of type `(MutationContext, (SearchStrategy[T@Draw], /) -> T@Draw, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]` has no attribute `__name__`
- src/schemathesis/specs/openapi/negative/mutations.py:231:72: error[unresolved-attribute] Object of type `typing.TypeVar` has no attribute `is_enabled`
+ src/schemathesis/specs/openapi/negative/mutations.py:231:72: error[unresolved-attribute] Object of type `T@Draw` has no attribute `is_enabled`
- src/schemathesis/specs/openapi/negative/mutations.py:248:9: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `typing.TypeVar`
+ src/schemathesis/specs/openapi/negative/mutations.py:248:9: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `T@Draw`
- src/schemathesis/specs/openapi/negative/mutations.py:288:27: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `str`, found `typing.TypeVar`
+ src/schemathesis/specs/openapi/negative/mutations.py:288:27: error[invalid-argument-type] Argument to bound method `remove` is incorrect: Expected `str`, found `T@Draw`
- src/schemathesis/specs/openapi/negative/mutations.py:292:63: error[unresolved-attribute] Object of type `typing.TypeVar` has no attribute `is_enabled`
+ src/schemathesis/specs/openapi/negative/mutations.py:292:63: error[unresolved-attribute] Object of type `T@Draw` has no attribute `is_enabled`
- src/schemathesis/specs/openapi/negative/mutations.py:297:46: error[invalid-argument-type] Argument to function `prevent_unsatisfiable_schema` is incorrect: Expected `str`, found `typing.TypeVar`
+ src/schemathesis/specs/openapi/negative/mutations.py:297:46: error[invalid-argument-type] Argument to function `prevent_unsatisfiable_schema` is incorrect: Expected `str`, found `T@Draw`
- src/schemathesis/specs/openapi/negative/mutations.py:361:32: error[not-iterable] Object of type `typing.TypeVar` is not iterable
+ src/schemathesis/specs/openapi/negative/mutations.py:361:32: error[not-iterable] Object of type `T@Draw` is not iterable
- src/schemathesis/specs/openapi/negative/mutations.py:361:37: error[invalid-argument-type] Argument is incorrect: Expected `SearchStrategy[typing.TypeVar]`, found `SearchStrategy[list[Unknown]]`
+ src/schemathesis/specs/openapi/negative/mutations.py:361:37: error[invalid-argument-type] Argument is incorrect: Expected `SearchStrategy[T@Draw]`, found `SearchStrategy[list[Unknown]]`
- src/schemathesis/specs/openapi/negative/mutations.py:390:12: error[unresolved-attribute] Object of type `typing.TypeVar` has no attribute `is_enabled`
+ src/schemathesis/specs/openapi/negative/mutations.py:390:12: error[unresolved-attribute] Object of type `T@Draw` has no attribute `is_enabled`
- src/schemathesis/specs/openapi/negative/mutations.py:393:20: error[unresolved-attribute] Object of type `typing.TypeVar` has no attribute `is_enabled`
+ src/schemathesis/specs/openapi/negative/mutations.py:393:20: error[unresolved-attribute] Object of type `T@Draw` has no attribute `is_enabled`
- src/schemathesis/specs/openapi/negative/mutations.py:393:49: error[unresolved-attribute] Object of type `(MutationContext, (SearchStrategy[typing.TypeVar], /) -> typing.TypeVar, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]` has no attribute `__name__`
+ src/schemathesis/specs/openapi/negative/mutations.py:393:49: error[unresolved-attribute] Object of type `(MutationContext, (SearchStrategy[T@Draw], /) -> T@Draw, dict[str, Any], /) -> tuple[MutationResult, MutationMetadata | None]` has no attribute `__name__`
- src/sc

... (truncated 8822 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~47MB
+     struct metadata = ~49MB
-     struct fields = ~49MB
+     struct fields = ~52MB


```

</details>




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-25 08:56_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-25 08:56_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-25 10:13_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-25 10:13_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-25 11:17_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-25 11:17_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-26 14:31_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-26 14:31_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-27 09:26_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-27 09:26_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-27 12:55_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-27 12:55_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-27 13:43_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-27 13:43_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 13:59_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-27 15:05_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-27 15:05_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-27 15:26_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-27 15:29_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-27 20:29_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-27 20:29_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-27 20:39_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-27 20:39_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-27 20:52_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-27 20:52_

---

_Marked ready for review by @sharkdp on 2025-11-27 21:01_

---

_Review requested from @carljm by @sharkdp on 2025-11-27 21:01_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-27 21:01_

---

_Review requested from @dcreager by @sharkdp on 2025-11-27 21:01_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-28 08:44_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-28 08:44_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-28 14:08_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-28 14:08_

---

_@sharkdp reviewed on 2025-11-28 14:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:194 on 2025-11-28 14:36_

I tried a couple of different approaches, but so far, I have not found a way to default-specialize type aliases that are used without an explicit specialization. It's fairly easy to turn this into `Unknown | int`, but so far, I haven't found a way that doesn't break random other things.

So these stay as TODOs with this PR

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-28 16:16_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-28 16:16_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:393 on 2025-11-28 16:29_

Pyrefly and pyright accept `TransparentAlias` as a valid type alias, but [mypy does not](https://mypy-play.net/?mypy=latest&python=3.12&gist=32eeea189747857ec9ae7e01f0fa30db). I think we could consider adding a TODO here for us to reject it too when it appears in a type expression. `TransparentAlias[int]` fails at runtime, just like `T[int]` fails at runtime. And there's no reason to use `TransparentAlias[int]` rather than just `int`.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:448 on 2025-11-28 16:30_

great work, this is fantastic!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:469 on 2025-11-28 16:30_

I guess the `Todo` message here is a bit misleading, because there's no `ParamSpec` involved here?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:671 on 2025-11-28 16:40_

the TODO comment feels like it's still relevant here? I think `Unknown` or `list[Unknown]` would be best, rather than `list[int]`. This is going to raise an exception at runtime if they're using Python <3.14 and they don't have `from __future__ import annotations` at the top of their file. And we don't really know _what_ they intended to write: they might have actually wanted their type alias to be generic over two type variables.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:723 on 2025-11-28 16:42_

```suggestion
    # TODO: `list[Unknown] | set[Unknown]` might be better
    reveal_type(x)  # revealed: list[typing.TypeVar] | set[typing.TypeVar]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:794 on 2025-11-28 16:44_

I would be fine with continuing to not emit errors on these; it sort-of seems like a feature that we understand them 😄 But I also don't oppose changing starting to emit diagnostics on these later if it simplifies our implementation somehow

---

_@AlexWaygood reviewed on 2025-11-28 16:50_

Just a review of the tests so far. A couple more tests you could add (possibly in a followup). I haven't checked whether or not these pass on your branch, just some pathological cases I thought of while reading your tests:

```py
from typing import TypeAlias, TypeVar

T = TypeVar("T")
U = TypeVar("U")

TotallyStringifiedPEP613: TypeAlias = "dict[T, U]"
TotallyStringifiedPartiallySpecialized: TypeAlias = "TotallyStringifiedPEP613[U, int]"

def f(x: "TotallyStringifiedPartiallySpecialized[str]"):
    reveal_type(x)  # revealed: dict[str, int]
```

(Edit David: Added. This one still resolves to a `@Todo` type that was introduced for this precise case)

and

```py
from typing import TypeVar

T = TypeVar("T")
U = TypeVar("U")
V = TypeVar("V")

X = tuple[T, *tuple[U, ...], V]
Y = X[T, tuple[int, str, U], bytes]

def g(obj: Y[bool, range]):
    reveal_type(obj)  # revealed: tuple[bool, *tuple[tuple[int, str, range], ...], bytes]
```

(Edit David: Added. This one passes!)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4803 on 2025-11-28 16:57_

oh, this is clever!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:10884 on 2025-11-28 17:00_

should we rename the method to just `infer_type_alias_specialization`? it seems we now call it for explicit type aliases and implicit type aliases 😄

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:11118 on 2025-11-28 17:03_

we should refactor the method so that the return type is `CallableType` rather than `Type`, then we wouldn't need the `.expect()`. I can pick that up after this is landed.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder/type_expression.rs`:983 on 2025-11-28 17:07_

I think there are probably lots more error cases in type-expression parsing where we should do similarly

---

_@AlexWaygood approved on 2025-11-28 17:07_

Fantastic work!

---

_@sharkdp reviewed on 2025-11-28 18:56_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:393 on 2025-11-28 18:56_

> `TransparentAlias[int]` fails at runtime, just like `T[int]` fails at runtime

Ok, but it's sort-of reasonable to have it in a type annotation? I spent extra effort to support it after Carl mentioned it, but we can also remove that special handling again, I guess.

---

_@sharkdp reviewed on 2025-11-28 18:59_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:469 on 2025-11-28 18:59_

Hm, not here directly, but `CallableIntToStr` really traces back to `MyCallable = Callable[P, T]` where `P` is a `ParamSpec`, and this is just how our `@Todo` types propagate?

---

_@sharkdp reviewed on 2025-11-28 19:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:794 on 2025-11-28 19:01_

I'll remove the `TODO`s and just mention that we might later want to change those.

---

_@AlexWaygood reviewed on 2025-11-28 19:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:393 on 2025-11-28 19:04_

I'd personally lean towards rejecting it because i can't see any use case for it — it probably indicates that the user is confused about Python typing in some way?

But i don't have a strong opinion; don't let this slow you down when it comes to getting the PR in, if you and/or Carl disagree

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:4803 on 2025-11-28 19:04_

To be fair, the `.replace` pattern is not something I invented. We already use this in other places to preserve the outer binding context.

---

_@sharkdp reviewed on 2025-11-28 19:04_

---

_@AlexWaygood reviewed on 2025-11-28 19:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:469 on 2025-11-28 19:04_

Oh i see. That's fine then

---

_@sharkdp reviewed on 2025-11-28 19:07_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:10884 on 2025-11-28 19:07_

Oh, "explicit" is an overloaded term here. This is supposed to be read as: "infer *an explicit specialization* of a (implicit or PEP-613/explicit) type alias". This is in line with `infer_explicit_type_alias_type_specialization`, which was already there before (but has been slightly renamed here).

We could consider calling it `infer_explicit_specialization_of_type_alias` or similar…

---

_@sharkdp reviewed on 2025-11-28 19:10_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:11118 on 2025-11-28 19:10_

Done

---

_@sharkdp reviewed on 2025-11-28 19:15_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:393 on 2025-11-28 19:15_

> I'd personally lean towards rejecting it because i can't see any use case for it — it probably indicates that the user is confused about Python typing in some way?

Maybe. But it seems like an artificial restriction to me? If `MyAlias = T | None` and `MyAlias = Annotated[T, "bla"]` are fine, then why should `MyAlias = T` be forbidden? A PEP 695 type alias `type MyAlias[T] = T` is (hopefully) also allowed.

---

_Merged by @sharkdp on 2025-11-28 19:38_

---

_Closed by @sharkdp on 2025-11-28 19:38_

---

_Branch deleted on 2025-11-28 19:38_

---
