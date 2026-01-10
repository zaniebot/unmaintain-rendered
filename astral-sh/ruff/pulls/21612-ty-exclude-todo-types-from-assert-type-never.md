```yaml
number: 21612
title: "[ty] Exclude `@Todo` types from `assert_{type,never}` checks"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: david/exclude-todo-types
created_at: 2025-11-24T13:06:03Z
updated_at: 2025-11-25T12:31:56Z
url: https://github.com/astral-sh/ruff/pull/21612
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Exclude `@Todo` types from `assert_{type,never}` checks

---

_Pull request opened by @sharkdp on 2025-11-24 13:06_

## Summary

Not sure if this a good idea, I can see arguments either way. This was prompted by a wave of type-assertion failures involving `@Todo` types in https://github.com/astral-sh/ruff/pull/21553.

---

_Label `ty` added by @sharkdp on 2025-11-24 13:06_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 13:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-24 13:18:13.056680705 +0000
+++ new-output.txt	2025-11-24 13:18:16.748705077 +0000
@@ -5,21 +5,8 @@
 _directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_explicit.py:52:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_explicit.py:53:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_explicit.py:54:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_explicit.py:56:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_explicit.py:59:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_implicit.py:63:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_implicit.py:64:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_implicit.py:65:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-aliases_implicit.py:67:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_implicit.py:70:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
-aliases_implicit.py:71:5: error[type-assertion-failure] Type `list[bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown | <class 'int'> | <class 'str'>]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
 aliases_implicit.py:109:9: error[invalid-type-form] Variable of type `list[<class 'int'> | Unknown]` is not allowed in a type expression
@@ -125,10 +112,6 @@
 callables_annotation.py:58:5: error[invalid-type-form] Special form `typing.Callable` expected exactly two arguments (parameter types and return type)
 callables_annotation.py:58:14: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
 callables_annotation.py:157:20: error[invalid-assignment] Object of type `Proto7` is not assignable to `Proto6`
-callables_kwargs.py:24:5: error[type-assertion-failure] Type `int` does not match asserted type `@Todo(`Unpack[]` special form)`
-callables_kwargs.py:32:9: error[type-assertion-failure] Type `str` does not match asserted type `@Todo(`Unpack[]` special form)`
-callables_kwargs.py:35:5: error[type-assertion-failure] Type `str` does not match asserted type `@Todo(`Unpack[]` special form)`
-callables_kwargs.py:41:5: error[type-assertion-failure] Type `TD1` does not match asserted type `dict[str, @Todo(`Unpack[]` special form)]`
 callables_kwargs.py:52:11: error[too-many-positional-arguments] Too many positional arguments to function `func1`: expected 0, got 3
 callables_kwargs.py:64:11: error[invalid-argument-type] Argument to function `func2` is incorrect: Expected `str`, found `Literal[1]`
 callables_kwargs.py:64:14: error[parameter-already-assigned] Multiple values provided for parameter `v3` of function `func2`
@@ -383,7 +366,6 @@
 enums_member_values.py:96:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 enums_members.py:82:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((x) -> Unknown)`
 enums_members.py:82:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
-enums_members.py:83:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((...) -> Unknown)`
 enums_members.py:83:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
 enums_members.py:84:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `property`
 enums_members.py:84:35: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
@@ -415,26 +397,13 @@
 generics_basic.py:171:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
 generics_basic.py:172:1: error[invalid-generic-class] `Generic` base class must include all type variables used in other base classes
 generics_basic.py:199:5: error[type-assertion-failure] Type `Iterator[Any]` does not match asserted type `Unknown`
-generics_defaults.py:30:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'NoNonDefaults'>`
-generics_defaults.py:31:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
-generics_defaults.py:32:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'NoNonDefaults[str, int]'>`
-generics_defaults.py:38:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'OneDefault[int | float, bool]'>`
-generics_defaults.py:45:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults'>`
-generics_defaults.py:46:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
 generics_defaults.py:50:1: error[missing-argument] No argument provided for required parameter `T2` of class `AllTheDefaults`
-generics_defaults.py:52:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:55:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:59:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:63:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'AllTheDefaults[int, int | float | complex, str, int, bool]'>`
-generics_defaults.py:79:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Class_ParamSpec'>`
 generics_defaults.py:80:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Class_ParamSpec[Unknown]`
 generics_defaults.py:80:32: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
 generics_defaults.py:81:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Class_ParamSpec[Unknown]`
 generics_defaults.py:81:29: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[bool, bool]`?
 generics_defaults.py:81:46: error[too-many-positional-arguments] Too many positional arguments to class `Class_ParamSpec`: expected 1, got 2
 generics_defaults.py:91:26: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_defaults.py:94:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Class_TypeVarTuple'>`
-generics_defaults.py:95:1: error[type-assertion-failure] Type `@Todo(specialized non-generic class)` does not match asserted type `Class_TypeVarTuple`
 generics_defaults.py:127:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T4@func1`
 generics_defaults.py:131:1: error[type-assertion-failure] Type `Any` does not match asserted type `int`
 generics_defaults.py:142:12: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
@@ -442,15 +411,10 @@
 generics_defaults.py:155:49: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int | float, bool]`?
 generics_defaults.py:156:58: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[bytes]`?
 generics_defaults.py:170:1: error[type-assertion-failure] Type `(Foo7[int], /) -> Foo7[int]` does not match asserted type `def meth(self, /) -> Self@meth`
-generics_defaults_referential.py:23:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'slice'>`
 generics_defaults_referential.py:36:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
 generics_defaults_referential.py:37:10: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Literal[""]`
-generics_defaults_referential.py:94:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
-generics_defaults_referential.py:95:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar[int, list[int]]'>`
 generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, typing.TypeVar]`
-generics_defaults_specialization.py:27:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
-generics_defaults_specialization.py:45:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
 generics_paramspec_basic.py:10:1: error[invalid-paramspec] The name of a `ParamSpec` (`NotIt`) must match the name of the variable it is assigned to (`WrongName`)
 generics_paramspec_basic.py:23:20: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `P@func1`
 generics_paramspec_basic.py:27:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
@@ -458,7 +422,6 @@
 generics_paramspec_components.py:83:18: error[parameter-already-assigned] Multiple values provided for parameter 1 (`x`) of function `foo`
 generics_paramspec_semantics.py:13:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> str`
 generics_paramspec_semantics.py:17:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
-generics_paramspec_semantics.py:22:1: error[type-assertion-failure] Type `(str, bool, /) -> str` does not match asserted type `(...) -> str`
 generics_paramspec_semantics.py:30:56: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
 generics_paramspec_semantics.py:34:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:38:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
@@ -466,7 +429,6 @@
 generics_paramspec_semantics.py:57:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:76:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 generics_paramspec_semantics.py:82:28: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[int]`?
-generics_paramspec_semantics.py:84:5: error[type-assertion-failure] Type `(int, /) -> str` does not match asserted type `(...) -> str`
 generics_paramspec_semantics.py:87:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 generics_paramspec_semantics.py:91:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
 generics_paramspec_semantics.py:101:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> bool`
@@ -568,10 +530,7 @@
 generics_type_erasure.py:38:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `Literal[""]`
 generics_type_erasure.py:40:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `Literal[0]`
 generics_typevartuple_args.py:16:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_args.py:20:1: error[type-assertion-failure] Type `tuple[int, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_args.py:27:77: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_args.py:31:1: error[type-assertion-failure] Type `tuple[()]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_args.py:32:1: error[type-assertion-failure] Type `tuple[int, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:12:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_basic.py:16:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:23:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
@@ -580,41 +539,21 @@
 generics_typevartuple_basic.py:66:27: error[too-many-positional-arguments] Too many positional arguments to function `__new__`: expected 2, got 4
 generics_typevartuple_basic.py:67:27: error[unknown-argument] Argument `bound` does not match any known parameter of function `__new__`
 generics_typevartuple_basic.py:75:50: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_basic.py:84:1: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_basic.py:106:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_callable.py:29:57: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:33:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[int | float | complex, str, int]`
 generics_typevartuple_callable.py:37:34: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[str]`
-generics_typevartuple_callable.py:41:1: error[type-assertion-failure] Type `tuple[str, int, int | float | complex]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_callable.py:42:1: error[type-assertion-failure] Type `tuple[str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_callable.py:45:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_callable.py:49:1: error[type-assertion-failure] Type `tuple[int | float, str, int | float | complex]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_concat.py:22:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_concat.py:47:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo(PEP 646), ...]`
-generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Type `tuple[int, bool, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_overloads.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:50:23: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:50:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:59:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Type `tuple[Any, *tuple[Any, ...]]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func7, T2@func7]`
-generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
-generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Type `tuple[tuple[str], bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
-generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Type `tuple[tuple[str, bool], int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
 generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func9, T2@func9, T3@func9]`
-generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
-generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Type `tuple[tuple[bool], str, int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
-generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `@Todo(Support for `typing.GenericAlias` instances in type expressions)`
-generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
-generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_unpack.py:17:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_upper_bound.py:37:1: error[type-assertion-failure] Type `list[int]` does not match asserted type `list[Unknown | int]`
 generics_upper_bound.py:38:1: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[Unknown | int]`
@@ -869,7 +808,6 @@
 qualifiers_final_decorator.py:21:16: error[subclass-of-final-class] Class `Derived1` cannot inherit from final class `Base1`
 qualifiers_final_decorator.py:89:9: error[invalid-overload] `@final` decorator should be applied only to the overload implementation
 qualifiers_final_decorator.py:118:9: error[invalid-method-override] Invalid override of method `method`: Definition is incompatible with `Base5_2.method`
-specialtypes_any.py:86:1: error[type-assertion-failure] Type `int` does not match asserted type `int | @Todo(instance attribute on class with dynamic base)`
 specialtypes_never.py:19:22: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Never`
 specialtypes_never.py:32:12: error[invalid-return-type] Return type does not match returned value: expected `int`, found `Literal["whatever works"]`
 specialtypes_never.py:86:21: error[invalid-assignment] Object of type `list[Never]` is not assignable to `list[int]`
@@ -1002,5 +940,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1004 diagnostics
+Found 942 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-24 13:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ tests/type_tests.py:29:45: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'type-assertion-failure'
+ tests/type_tests.py:36:46: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'type-assertion-failure'
+ tests/type_tests.py:44:69: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'type-assertion-failure'
+ tests/type_tests.py:52:71: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'type-assertion-failure'
- Found 169 diagnostics
+ Found 173 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/addons/test_dns_resolver.py:155:17: error[type-assertion-failure] Type `@Todo` is not equivalent to `Never`
- Found 1878 diagnostics
+ Found 1877 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/config/config_options_tests.py:1484:9: error[type-assertion-failure] Type `int | None` does not match asserted type `@Todo`
- Found 226 diagnostics
+ Found 225 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/models/sw1d_network.py:1167:37: error[type-assertion-failure] Type `@Todo & ~None & ~RoutingModel_V2 & ~RoutingModel_V1 & ~RoutingModel_V3` is not equivalent to `Never`
- Found 641 diagnostics
+ Found 640 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- tests/assert_type/contrib/admin/test_utils.py:74:1: error[type-assertion-failure] Type `list[@Todo]` does not match asserted type `list[str]`
- tests/assert_type/contrib/admin/test_utils.py:75:1: error[type-assertion-failure] Type `list[@Todo]` does not match asserted type `list[str]`
- tests/assert_type/contrib/admin/test_utils.py:76:1: error[type-assertion-failure] Type `list[@Todo]` does not match asserted type `list[str]`
- tests/assert_type/contrib/admin/test_utils.py:77:1: error[type-assertion-failure] Type `list[@Todo]` does not match asserted type `list[str]`
- tests/assert_type/views/test_generic.py:14:1: error[type-assertion-failure] Type `type[MyModel]` does not match asserted type `@Todo`
- tests/assert_type/views/test_generic.py:24:1: error[type-assertion-failure] Type `type[MyModel] | None` does not match asserted type `@Todo | None`
- Found 445 diagnostics
+ Found 439 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/arrays/test_extension_array.py:46:11: error[type-assertion-failure] Type `ExtensionArray | @Todo` does not match asserted type `Unknown`
- tests/indexes/arithmetic/bool/test_add.py:49:11: error[type-assertion-failure] Type `Index[bool]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_add.py:50:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_add.py:51:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_add.py:52:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_mul.py:83:11: error[type-assertion-failure] Type `Index[bool]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_mul.py:84:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_mul.py:85:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_mul.py:86:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_mul.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_mul.py:89:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_sub.py:59:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_sub.py:60:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_sub.py:61:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_sub.py:62:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_sub.py:68:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_truediv.py:65:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_truediv.py:66:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_truediv.py:67:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/bool/test_truediv.py:68:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_add.py:49:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_add.py:50:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_add.py:51:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_add.py:52:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_mul.py:83:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_mul.py:84:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_mul.py:85:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_mul.py:86:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_mul.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_mul.py:89:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_sub.py:51:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_sub.py:52:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_sub.py:53:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_sub.py:54:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_sub.py:59:11: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_truediv.py:54:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_truediv.py:55:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_truediv.py:56:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/complex/test_truediv.py:57:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_add.py:49:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_add.py:50:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_add.py:51:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_add.py:52:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_floordiv.py:84:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_floordiv.py:85:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_floordiv.py:86:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_floordiv.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_floordiv.py:89:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_floordiv.py:90:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_mul.py:83:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_mul.py:84:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_mul.py:85:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_mul.py:86:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_mul.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_mul.py:89:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_sub.py:51:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_sub.py:52:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_sub.py:53:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_sub.py:54:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_sub.py:59:11: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_truediv.py:83:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_truediv.py:84:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_truediv.py:85:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_truediv.py:86:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_truediv.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/float/test_truediv.py:89:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_add.py:49:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_add.py:50:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_add.py:51:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_add.py:52:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_floordiv.py:84:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_floordiv.py:85:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_floordiv.py:86:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_floordiv.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_floordiv.py:89:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_floordiv.py:90:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_mul.py:83:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_mul.py:84:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_mul.py:85:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_mul.py:86:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_mul.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_mul.py:89:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_sub.py:51:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_sub.py:52:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_sub.py:53:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_sub.py:54:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_sub.py:59:11: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_truediv.py:83:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_truediv.py:84:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_truediv.py:85:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_truediv.py:86:11: error[type-assertion-failure] Type `Index[int | float | complex]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_truediv.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/int/test_truediv.py:89:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/str/test_add.py:57:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/str/test_add.py:58:11: error[type-assertion-failure] Type `Index[str]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/str/test_mul.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/str/test_mul.py:89:11: error[type-assertion-failure] Type `Index[str]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/str/test_mul.py:91:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/str/test_mul.py:92:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/str/test_mul.py:93:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/str/test_mul.py:94:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_floordiv.py:84:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_floordiv.py:85:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_floordiv.py:86:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_floordiv.py:88:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_floordiv.py:89:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_floordiv.py:90:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_mul.py:58:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_mul.py:59:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_mul.py:60:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_mul.py:61:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_truediv.py:59:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_truediv.py:60:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_truediv.py:61:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/test_truediv.py:62:11: error[type-assertion-failure] Type `Index[Any]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_floordiv.py:85:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_floordiv.py:86:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_floordiv.py:87:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_floordiv.py:89:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_floordiv.py:90:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_floordiv.py:91:11: error[type-assertion-failure] Type `Index[int]` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_mul.py:69:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_mul.py:70:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_mul.py:71:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_mul.py:73:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_truediv.py:85:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_truediv.py:86:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_truediv.py:87:11: error[type-assertion-failure] Type `TimedeltaIndex` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_truediv.py:89:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_truediv.py:90:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/indexes/arithmetic/timedeltaindex/test_truediv.py:91:11: error[type-assertion-failure] Type `Index[int | float]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:249:11: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:250:11: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:251:11: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:257:9: error[type-assertion-failure] Type `ndarray[tuple[int], Unknown]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:1363:11: error[type-assertion-failure] Type `signedinteger[_64Bit]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:1372:11: error[type-assertion-failure] Type `signedinteger[_64Bit]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:1373:11: error[type-assertion-failure] Type `signedinteger[_64Bit]` does not match asserted type `@Todo`
- tests/indexes/test_indexes.py:1374:11: error[type-assertion-failure] Type `signedinteger[_64Bit]` does not match asserted type `@Todo`
- tests/indexes/test_rangeindex.py:49:9: error[type-assertion-failure] Type `tuple[Index[Any], @Todo | None, @Todo | None]` does not match asserted type `Unknown`
- tests/scalars/test_scalars.py:737:17: error[type-assertion-failure] Type `bool[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:738:17: error[type-assertion-failure] Type `bool[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:754:17: error[type-assertion-failure] Type `bool[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:755:17: error[type-assertion-failure] Type `bool[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1103:17: error[type-assertion-failure] Type `bool[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1104:17: error[type-assertion-failure] Type `bool[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1120:17: error[type-assertion-failure] Type `bool[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1121:17: error[type-assertion-failure] Type `bool[bool]` does not match asserted type `@Todo`
- tests/scalars/test_scalars.py:1845:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1846:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1848:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1849:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1851:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1852:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1866:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1867:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1869:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1870:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1872:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1873:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1887:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1888:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1890:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1891:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1893:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/scalars/test_scalars.py:1894:20: error[type-assertion-failure] Type `@Todo` does not match asserted type `bool`
- tests/series/arithmetic/bool/test_add.py:72:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_add.py:73:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_add.py:74:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_add.py:75:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_mul.py:119:11: error[type-assertion-failure] Type `Series[bool]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_mul.py:120:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_mul.py:121:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_mul.py:122:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_mul.py:124:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_mul.py:125:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_sub.py:87:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_sub.py:88:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_sub.py:89:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_sub.py:90:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_sub.py:96:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_truediv.py:133:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_truediv.py:134:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_truediv.py:135:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/bool/test_truediv.py:136:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_add.py:84:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_add.py:85:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_add.py:86:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_add.py:87:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_mul.py:131:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_mul.py:132:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_mul.py:133:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_mul.py:134:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_mul.py:136:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_mul.py:137:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_sub.py:86:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_sub.py:87:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_sub.py:88:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_sub.py:89:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_sub.py:94:11: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_truediv.py:174:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_truediv.py:175:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_truediv.py:176:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/complex/test_truediv.py:177:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_add.py:72:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_add.py:73:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_add.py:74:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_add.py:75:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_floordiv.py:124:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_floordiv.py:125:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_floordiv.py:126:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_floordiv.py:128:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_floordiv.py:129:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_floordiv.py:130:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_mul.py:119:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_mul.py:120:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_mul.py:121:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_mul.py:122:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_mul.py:124:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_mul.py:125:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_sub.py:74:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_sub.py:75:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_sub.py:76:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_sub.py:77:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_sub.py:82:11: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_truediv.py:177:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_truediv.py:178:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_truediv.py:179:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_truediv.py:180:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_truediv.py:182:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/float/test_truediv.py:183:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_add.py:72:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_add.py:73:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_add.py:74:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_add.py:75:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_floordiv.py:124:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_floordiv.py:125:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_floordiv.py:126:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_floordiv.py:128:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_floordiv.py:129:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_floordiv.py:130:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_mul.py:119:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_mul.py:120:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_mul.py:121:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_mul.py:122:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_mul.py:124:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_mul.py:125:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_sub.py:74:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_sub.py:75:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_sub.py:76:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_sub.py:77:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_sub.py:82:11: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_truediv.py:177:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_truediv.py:178:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_truediv.py:179:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_truediv.py:180:11: error[type-assertion-failure] Type `Series[int | float | complex]` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_truediv.py:182:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/int/test_truediv.py:183:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/str/test_add.py:75:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/str/test_add.py:76:11: error[type-assertion-failure] Type `Series[str]` does not match asserted type `@Todo`
- tests/series/arithmetic/str/test_mul.py:124:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/str/test_mul.py:125:11: error[type-assertion-failure] Type `Series[str]` does not match asserted type `@Todo`
- tests/series/arithmetic/str/test_mul.py:127:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/str/test_mul.py:128:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/str/test_mul.py:129:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/str/test_mul.py:130:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/test_add.py:83:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_add.py:84:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_add.py:85:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_add.py:86:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_floordiv.py:124:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_floordiv.py:125:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_floordiv.py:126:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_floordiv.py:129:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/test_floordiv.py:132:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/test_floordiv.py:135:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/test_mul.py:78:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_mul.py:79:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_mul.py:80:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_mul.py:81:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_sub.py:81:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_sub.py:82:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_sub.py:83:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_sub.py:84:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_sub.py:235:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_truediv.py:149:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_truediv.py:150:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_truediv.py:151:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_truediv.py:152:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/series/arithmetic/test_truediv.py:154:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/test_truediv.py:155:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_add.py:99:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_add.py:100:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_floordiv.py:135:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_floordiv.py:136:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_floordiv.py:137:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_floordiv.py:139:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_floordiv.py:140:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_floordiv.py:141:11: error[type-assertion-failure] Type `Series[int]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_mul.py:97:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_mul.py:98:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_mul.py:99:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_mul.py:101:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_sub.py:109:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_sub.py:110:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_truediv.py:161:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_truediv.py:162:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_truediv.py:163:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_truediv.py:165:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_truediv.py:166:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timedelta/test_truediv.py:167:11: error[type-assertion-failure] Type `Series[int | float]` does not match asserted type `@Todo`
- tests/series/arithmetic/timestamp/test_add.py:116:9: error[type-assertion-failure] Type `Never` does not match asserted type `@Todo`
- tests/series/arithmetic/timestamp/test_add.py:117:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/series/arithmetic/timestamp/test_sub.py:106:11: error[type-assertion-failure] Type `Series[Timedelta]` does not match asserted type `@Todo`
- tests/series/arithmetic/timestamp/test_sub.py:107:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/series/arithmetic/timestamp/test_sub.py:117:11: error[type-assertion-failure] Type `Series[Timestamp]` does not match asserted type `@Todo`
- tests/test_api_types.py:22:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:23:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:24:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:25:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:26:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:27:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:43:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:44:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:45:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:46:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:48:9: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:51:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:143:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:144:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:145:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:146:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:147:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:148:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:176:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:177:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:178:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:179:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:181:9: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_api_types.py:184:11: error[type-assertion-failure] Type `bool` does not match asserted type `@Todo`
- tests/test_frame.py:2643:11: error[type-assertion-failure] Type `DataFrame` does not match asserted type `@Todo`
- tests/test_pandas.py:768:11: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/test_pandas.py:770:9: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `@Todo`
- tests/test_pandas.py:777:9: error[type-assertion-failure] Type `Series[Any]` does not m

... (truncated 39 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-24 13:12_

---

_Renamed from "[ty] Exclude @Todo types from `assert_{type,never}` checks" to "[ty] Exclude `@Todo` types from `assert_{type,never}` checks" by @sharkdp on 2025-11-24 13:17_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-24 13:18_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-24 13:18_

---

_Comment by @astral-sh-bot[bot] on 2025-11-24 13:27_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `type-assertion-failure` | 0 | 381 | 0 |
| `unused-ignore-comment` | 4 | 0 | 0 |
| **Total** | **4** | **381** | **0** |

**[Full report with detailed diff](https://david-exclude-todo-types.ecosystem-663.pages.dev/diff)** ([timing results](https://david-exclude-todo-types.ecosystem-663.pages.dev/timing))




---

_Comment by @sharkdp on 2025-11-24 14:24_

Opening for review to see what others think.

---

_Marked ready for review by @sharkdp on 2025-11-24 14:25_

---

_Review requested from @carljm by @sharkdp on 2025-11-24 14:25_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-24 14:25_

---

_Review requested from @dcreager by @sharkdp on 2025-11-24 14:25_

---

_Comment by @carljm on 2025-11-24 22:31_

I think on balance I would come down against doing this? I feel like `assert_type` and `assert_never` are important not to lie about.

---

_Comment by @carljm on 2025-11-24 22:39_

But I can see the argument in favor (silencing false positives in areas where we have known limitations), so open to discussion...

---

_Closed by @sharkdp on 2025-11-25 12:31_

---
