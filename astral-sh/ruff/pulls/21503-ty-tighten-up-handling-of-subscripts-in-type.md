```yaml
number: 21503
title: "[ty] tighten up handling of subscripts in type expressions"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/unknown-subscript
created_at: 2025-11-17T20:07:58Z
updated_at: 2025-11-18T18:49:11Z
url: https://github.com/astral-sh/ruff/pull/21503
synced_at: 2026-01-12T15:57:26Z
```

# [ty] tighten up handling of subscripts in type expressions

---

_@carljm_

## Summary

Get rid of the catch-all todo type from subscripting a base type we haven't implemented handling for yet in a type expression, and turn it into a diagnostic instead.

Handle a few more cases explicitly, to avoid false positives from the above change:
1. Subscripting any dynamic type (not just a todo type) in a type expression should just result in that same dynamic type. This is important for gradual guarantee, and matches other type checkers.
2. Subscripting a generic alias may be an error or not, depending whether the specialization itself contains typevars. Don't try to handle this yet (it should be handled in a later PR for specializing generic non-PEP695 type aliases), just use a dedicated todo type for it.
3. Add a temporary todo branch to avoid false positives from string PEP 613 type aliases. This can be removed in the next PR, with PEP 613 type alias support.

## Test Plan

Adjusted mdtests, ecosystem.

All new diagnostics in conformance suite are supposed to be diagnostics, so this PR is a strict improvement there.

New diagnostics in the ecosystem are surfacing cases where we already don't understand an annotation, but now we emit a diagnostic about it. They are mostly intentional choices. Analysis of particular cases:

* `attrs`, `bokeh`, `django-stubs`, `dulwich`, `ibis`, `kornia`, `mitmproxy`, `mongo-python-driver`, `mypy`, `pandas`, `poetry`, `prefect`, `pydantic`, `pytest`, `scrapy`, `trio`, `werkzeug`, and `xarray`  are all cases where under `from __future__ import annotations` or Python 3.14 deferred-annotations semantics, we follow normal name-scoping rules, whereas some other type checkers prefer global names over local names. This means we don't like it if e.g. you have a class with a method or attribute named `type` or `tuple`, and you also try to use `type` or `tuple` in method/attribute annotations of that class. This PR isn't changing those semantics, just revealing them in more cases where previously we just silently fell back to `Unknown`. I think failing with a diagnostic (so authors can alias names as needed to avoid relying on scoping rules that differ between type checkers) is better than failing silently here.
* `beartype` assumes we support `TypeForm` (because it only supports mypy and pyright, it uses `if MYPY:` to hide the `TypeForm` from mypy, and pyright supports `TypeForm`), and we don't yet.
* `graphql-core` likes to use a `try: ... except ImportError: ...` pattern for importing special forms from `typing` with fallback to `typing_extensions`, instead of using `sys.version_info` checks. We don't handle this well when type checking under an older Python version (where the import from `typing` is not found); we see the imported name as of type e.g. `Unknown | SpecialFormType(...)`, and because of the union with `Unknown` we fail to handle it as the special form type. Mypy and pyright also don't seem to support this pattern. They don't complain about subscripting such special forms, but they do silently fail to treat them as the desired special form.  Again here, if we are going to fail I'd rather fail with a diagnostic rather than silently.
* `ibis` is [trying to use](https://github.com/ibis-project/ibis/blob/main/ibis/common/collections.py#L372) `frozendict: type[FrozenDict]` as a way to create a "type alias" to `FrozenDict`, but this is wrong: that means `frozendict: type[FrozenDict[Any, Any]]`.
* `mypy` has some errors due to the fact that type-checking `typing.pyi` itself (without knowing that it's the real `typing.pyi`) doesn't work very well.
* `mypy-protobuf` imports some types from the protobufs library that end up unioned with `Unknown` for some reason, and so we don't allow explicit-specialization of them. Depending on the reason they end up unioned with `Unknown`, we might want to better support this? But it's orthogonal to this PR -- we aren't failing any worse here, just alerting the author that we didn't understand their annotation.
* `pwndbg` has unresolved references due to star-importing from a dependency that isn't installed, and uses un-imported names like `Dict` in annotation expressions. Some of the unresolved references were hidden by https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/infer/builder.rs#L7223-L7228 when some annotations previously resolved to a Todo type that no longer do.

---

_Label `ty` added by @carljm on 2025-11-17 20:08_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 20:10_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-18 18:17:10.806838177 +0000
+++ new-output.txt	2025-11-18 18:17:14.257848390 +0000
@@ -6,22 +6,22 @@
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
 aliases_explicit.py:45:10: error[invalid-type-form] Variable of type `Literal["int | str"]` is not allowed in a type expression
-aliases_explicit.py:52:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(unknown type subscript)`
+aliases_explicit.py:52:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 aliases_explicit.py:53:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_explicit.py:54:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(unknown type subscript)`
+aliases_explicit.py:54:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 aliases_explicit.py:56:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
 aliases_explicit.py:57:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_explicit.py:59:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(unknown type subscript)]`
+aliases_explicit.py:59:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
 aliases_explicit.py:61:5: error[type-assertion-failure] Type `int | str` does not match asserted type `Unknown`
 aliases_explicit.py:101:6: error[call-non-callable] Object of type `UnionType` is not callable
 aliases_implicit.py:54:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
-aliases_implicit.py:63:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(unknown type subscript)`
+aliases_implicit.py:63:5: error[type-assertion-failure] Type `list[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 aliases_implicit.py:64:5: error[type-assertion-failure] Type `tuple[str, ...] | list[str]` does not match asserted type `@Todo(Generic specialization of types.UnionType)`
-aliases_implicit.py:65:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(unknown type subscript)`
+aliases_implicit.py:65:5: error[type-assertion-failure] Type `tuple[int, int, int, str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 aliases_implicit.py:67:5: error[type-assertion-failure] Type `(int, str, /) -> str` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
 aliases_implicit.py:68:5: error[type-assertion-failure] Type `(int, str, str, /) -> None` does not match asserted type `@Todo(Generic specialization of typing.Callable)`
-aliases_implicit.py:70:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(unknown type subscript)]`
-aliases_implicit.py:71:5: error[type-assertion-failure] Type `list[bool]` does not match asserted type `@Todo(unknown type subscript)`
+aliases_implicit.py:70:5: error[type-assertion-failure] Type `int | str | None | list[list[int]]` does not match asserted type `int | str | None | list[@Todo(specialized generic alias in type expression)]`
+aliases_implicit.py:71:5: error[type-assertion-failure] Type `list[bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 aliases_implicit.py:107:9: error[invalid-type-form] Variable of type `list[Unknown | <class 'int'> | <class 'str'>]` is not allowed in a type expression
 aliases_implicit.py:108:9: error[invalid-type-form] Variable of type `tuple[tuple[<class 'int'>, <class 'str'>]]` is not allowed in a type expression
 aliases_implicit.py:109:9: error[invalid-type-form] Variable of type `list[<class 'int'> | Unknown]` is not allowed in a type expression
@@ -58,6 +58,7 @@
 aliases_type_statement.py:40:22: error[invalid-type-form] List comprehensions are not allowed in type expressions
 aliases_type_statement.py:41:22: error[invalid-type-form] Dict literals are not allowed in type expressions
 aliases_type_statement.py:42:22: error[invalid-type-form] Function calls are not allowed in type expressions
+aliases_type_statement.py:43:22: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
 aliases_type_statement.py:43:28: error[invalid-type-form] Int literals are not allowed in this context in a type expression
 aliases_type_statement.py:44:22: error[invalid-type-form] `if` expressions are not allowed in type expressions
 aliases_type_statement.py:45:22: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
@@ -76,6 +77,7 @@
 aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar]'>` with no `__class_getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar, typing.TypeVar]'>` with no `__class_getitem__` method
+annotations_forward_refs.py:47:10: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
 annotations_forward_refs.py:49:10: error[invalid-type-form] Variable of type `Literal[1]` is not allowed in a type expression
 annotations_forward_refs.py:54:11: error[fstring-type-annotation] Type expressions cannot use f-strings
 annotations_forward_refs.py:55:11: error[invalid-type-form] Variable of type `<module 'types'>` is not allowed in a type expression
@@ -101,6 +103,7 @@
 annotations_typeexpr.py:91:9: error[invalid-type-form] List comprehensions are not allowed in type expressions
 annotations_typeexpr.py:92:9: error[invalid-type-form] Dict literals are not allowed in type expressions
 annotations_typeexpr.py:93:9: error[invalid-type-form] Function calls are not allowed in type expressions
+annotations_typeexpr.py:94:9: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
 annotations_typeexpr.py:94:15: error[invalid-type-form] Int literals are not allowed in this context in a type expression
 annotations_typeexpr.py:95:9: error[invalid-type-form] `if` expressions are not allowed in type expressions
 annotations_typeexpr.py:96:9: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
@@ -447,7 +450,7 @@
 generics_defaults_referential.py:94:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
 generics_defaults_referential.py:95:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar[int, list[int]]'>`
 generics_defaults_specialization.py:26:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, str]` does not match asserted type `SomethingWithNoDefaults[int, typing.TypeVar]`
-generics_defaults_specialization.py:27:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, bool]` does not match asserted type `@Todo(unknown type subscript)`
+generics_defaults_specialization.py:27:5: error[type-assertion-failure] Type `SomethingWithNoDefaults[int, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_defaults_specialization.py:30:1: error[non-subscriptable] Cannot subscript object of type `<class 'SomethingWithNoDefaults[int, typing.TypeVar]'>` with no `__class_getitem__` method
 generics_defaults_specialization.py:45:1: error[type-assertion-failure] Type `@Todo(unsupported nested subscript in type[X])` does not match asserted type `<class 'Bar'>`
 generics_paramspec_basic.py:10:1: error[invalid-paramspec] The name of a `ParamSpec` (`NotIt`) must match the name of the variable it is assigned to (`WrongName`)
@@ -593,16 +596,16 @@
 generics_typevartuple_concat.py:52:1: error[type-assertion-failure] Type `tuple[int, bool, str]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_overloads.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_typevartuple_specialization.py:16:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `@Todo(unknown type subscript)`
-generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(unknown type subscript)`
+generics_typevartuple_specialization.py:46:5: error[type-assertion-failure] Type `tuple[int, int | float, bool]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:47:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:50:23: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:50:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
-generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `@Todo(unknown type subscript)`
-generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(unknown type subscript)`
+generics_typevartuple_specialization.py:51:5: error[type-assertion-failure] Type `tuple[int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:52:5: error[type-assertion-failure] Type `tuple[str, @Todo(specialized non-generic class)]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:52:37: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[()]`?
 generics_typevartuple_specialization.py:59:14: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
-generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `@Todo(unknown type subscript)`
-generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `@Todo(unknown type subscript)`
+generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Type `tuple[str, int]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Type `tuple[int | float]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Type `tuple[Any, *tuple[Any, ...]]` does not match asserted type `tuple[@Todo(PEP 646), ...]`
 generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[tuple[@Todo(PEP 646), ...], T1@func7, T2@func7]`
 generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown]`
@@ -612,8 +615,8 @@
 generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Type `tuple[tuple[()], str, bool, int | float]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
 generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Type `tuple[tuple[bool], str, int | float, int]` does not match asserted type `tuple[tuple[@Todo(PEP 646), ...], Unknown, Unknown, Unknown]`
 generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], int]` does not match asserted type `@Todo(Support for `typing.GenericAlias` instances in type expressions)`
-generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(unknown type subscript)`
-generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(unknown type subscript)`
+generics_typevartuple_specialization.py:158:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
+generics_typevartuple_specialization.py:159:5: error[type-assertion-failure] Type `tuple[*tuple[int, ...], str]` does not match asserted type `@Todo(specialized generic alias in type expression)`
 generics_typevartuple_unpack.py:17:13: error[invalid-argument-type] `@Todo(starred expression)` is not a valid argument to `Generic`
 generics_upper_bound.py:37:1: error[type-assertion-failure] Type `list[int]` does not match asserted type `list[Unknown | int]`
 generics_upper_bound.py:38:1: error[type-assertion-failure] Type `set[int]` does not match asserted type `set[Unknown | int]`
@@ -837,6 +840,7 @@
 qualifiers_annotated.py:40:17: error[invalid-type-form] List comprehensions are not allowed in type expressions
 qualifiers_annotated.py:41:17: error[invalid-type-form] Dict literals are not allowed in type expressions
 qualifiers_annotated.py:42:17: error[invalid-type-form] Function calls are not allowed in type expressions
+qualifiers_annotated.py:43:17: error[invalid-type-form] Invalid subscript of object of type `list[Unknown | <class 'int'>]` in type expression
 qualifiers_annotated.py:43:23: error[invalid-type-form] Int literals are not allowed in this context in a type expression
 qualifiers_annotated.py:44:17: error[invalid-type-form] `if` expressions are not allowed in type expressions
 qualifiers_annotated.py:45:17: error[unresolved-reference] Name `var1` used when not defined
@@ -989,5 +993,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 991 diagnostics
+Found 995 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-17 20:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ src/attr/__init__.pyi:136:11: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | None` in type expression
- Found 594 diagnostics
+ Found 595 diagnostics

kornia (https://github.com/kornia/kornia)
+ kornia/color/colormap.py:61:29: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(cls) -> Unknown` in type expression
+ kornia/color/colormap.py:91:22: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(cls) -> Unknown` in type expression
- Found 764 diagnostics
+ Found 766 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/headers.py:65:75: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def set(self, key: str, value: Any, /, **kwargs: Any) -> None` in type expression
+ src/werkzeug/datastructures/headers.py:230:75: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def set(self, key: str, value: Any, /, **kwargs: Any) -> None` in type expression
- Found 407 diagnostics
+ Found 409 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/door/_func/doorcheck.py:131:11: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
- Found 504 diagnostics
+ Found 505 diagnostics

black (https://github.com/psf/black)
- src/black/trans.py:55:16: error[]8;;https://ty.dev/rules#non-subscriptable\non-subscriptable]8;;\] Cannot subscript object of type `object` with no `__getitem__` method
- Found 64 diagnostics
+ Found 63 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/_code/code.py:501:21: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/_pytest/_code/code.py:507:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/_pytest/_code/code.py:551:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/_pytest/_code/code.py:590:45: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/_pytest/_code/code.py:596:23: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/_pytest/recwarn.py:190:23: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
- testing/typing_raises_group.py:237:5: error[]8;;https://ty.dev/rules#type-assertion-failure\type-assertion-failure]8;;\] Type `((...) -> bool) | None` does not match asserted type `Unknown | ((@Todo, /) -> bool) | None`
+ testing/typing_raises_group.py:237:5: error[]8;;https://ty.dev/rules#type-assertion-failure\type-assertion-failure]8;;\] Type `((Unknown, /) -> bool) | None` does not match asserted type `Unknown | ((Unknown, /) -> bool) | None`
- Found 439 diagnostics
+ Found 445 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/spiderloader.py:42:23: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, /) -> Unknown` in type expression
+ scrapy/spiderloader.py:119:52: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self) -> Unknown` in type expression
+ scrapy/spiderloader.py:127:23: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self) -> Unknown` in type expression
+ scrapy/spiderloader.py:145:23: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self) -> Unknown` in type expression
- Found 1702 diagnostics
+ Found 1706 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/applications.py:33:19: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `UnionType` in type expression
+ starlette/routing.py:590:19: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `UnionType` in type expression
+ starlette/routing.py:615:36: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `UnionType` in type expression
- Found 222 diagnostics
+ Found 225 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/execution/execute.py:216:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/execute.py:216:59: error[]8;;https://ty.dev/rules#invalid-assignment\invalid-assignment]8;;\] Object of type `staticmethod[Unknown, Unknown]` is not assignable to `(Any, /) -> @Todo`
+ src/graphql/execution/execute.py:216:59: error[]8;;https://ty.dev/rules#invalid-assignment\invalid-assignment]8;;\] Object of type `staticmethod[Unknown, Unknown]` is not assignable to `(Any, /) -> Unknown`
+ src/graphql/execution/execute.py:233:39: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/execute.py:268:39: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- src/graphql/execution/execute.py:500:38: error[]8;;https://ty.dev/rules#invalid-await\invalid-await]8;;\] `@Todo | UndefinedType` is not awaitable
+ src/graphql/execution/execute.py:500:38: error[]8;;https://ty.dev/rules#invalid-await\invalid-await]8;;\] `Unknown | UndefinedType` is not awaitable
- src/graphql/execution/execute.py:556:54: error[]8;;https://ty.dev/rules#invalid-argument-type\invalid-argument-type]8;;\] Argument to function `resolve` is incorrect: Expected `Awaitable[GraphQLWrappedResult[dict[str, Any]]]`, found `@Todo | UndefinedType`
+ src/graphql/execution/execute.py:556:54: error[]8;;https://ty.dev/rules#invalid-argument-type\invalid-argument-type]8;;\] Argument to function `resolve` is incorrect: Expected `Awaitable[GraphQLWrappedResult[dict[str, Any]]]`, found `Unknown | UndefinedType`
- src/graphql/execution/execute.py:1984:46: error[]8;;https://ty.dev/rules#invalid-await\invalid-await]8;;\] `@Todo | GraphQLWrappedResult[None]` is not awaitable
+ src/graphql/execution/execute.py:1984:46: error[]8;;https://ty.dev/rules#invalid-await\invalid-await]8;;\] `Unknown | GraphQLWrappedResult[None]` is not awaitable
+ src/graphql/execution/execute.py:2124:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/execute.py:2188:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/execute.py:2229:42: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/execute.py:2257:31: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/incremental_graph.py:82:6: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/incremental_graph.py:89:6: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/types.py:240:13: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:243:14: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:244:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:343:12: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:349:14: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:350:18: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:351:16: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:353:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:454:11: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:455:13: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:459:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:576:13: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:579:14: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:580:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:701:13: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.NotRequired` in type expression
+ src/graphql/execution/types.py:706:6: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/types.py:763:6: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/types.py:778:6: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/execution/types.py:798:13: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `types.UnionType | Unknown` in type expression
+ src/graphql/execution/types.py:805:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `types.UnionType | Unknown` in type expression
+ src/graphql/execution/types.py:910:6: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/graphql.py:41:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/graphql.py:111:42: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/graphql.py:139:31: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/graphql.py:177:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:43:39: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:48:50: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:53:38: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:58:50: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:63:34: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:68:40: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:79:33: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:84:51: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:89:44: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:96:6: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/predicates.py:101:43: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/language/source.py:79:31: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/pyutils/async_reduce.py:27:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/pyutils/is_awaitable.py:20:33: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/pyutils/is_iterable.py:28:34: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/pyutils/is_iterable.py:35:32: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:183:28: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:213:37: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:455:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:584:39: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:610:39: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:753:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `types.UnionType | Unknown` in type expression
+ src/graphql/type/definition.py:754:21: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `types.UnionType | Unknown` in type expression
+ src/graphql/type/definition.py:814:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:857:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `types.UnionType | Unknown` in type expression
+ src/graphql/type/definition.py:858:21: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `types.UnionType | Unknown` in type expression
+ src/graphql/type/definition.py:918:38: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:963:16: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `types.UnionType | Unknown` in type expression
+ src/graphql/type/definition.py:1003:34: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1071:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `types.UnionType | Unknown` in type expression
+ src/graphql/type/definition.py:1197:33: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1316:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `types.UnionType | Unknown` in type expression
+ src/graphql/type/definition.py:1375:41: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1490:33: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1579:34: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1594:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1616:37: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1629:37: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1689:34: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1725:33: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1745:38: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/definition.py:1765:37: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/directives.py:142:37: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/scalars.py:321:58: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/type/schema.py:442:31: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
+ src/graphql/validation/rules/known_type_names.py:92:6: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- tests/execution/test_abstract.py:47:24: error[]8;;https://ty.dev/rules#invalid-await\invalid-await]8;;\] `ExecutionResult | @Todo` is not awaitable
+ tests/execution/test_abstract.py:47:24: error[]8;;https://ty.dev/rules#invalid-await\invalid-await]8;;\] `ExecutionResult | Unknown` is not awaitable
+ tests/type/test_definition.py:1291:39: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Unknown | typing.TypeGuard` in type expression
- Found 408 diagnostics
+ Found 489 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/worktree.py:147:23: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self) -> Unknown` in type expression
- Found 175 diagnostics
+ Found 176 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/utils/env/env_manager.py:260:48: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, name: str | None = None) -> Unknown` in type expression
+ tests/repositories/fixtures/pypi.org/generate.py:111:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, location: Path | None = None) -> Iterator[ReleaseFileMetadata]` in type expression
- Found 976 diagnostics
+ Found 978 diagnostics

mypy (https://github.com/python/mypy)
+ mypy/test/testtypes.py:268:43: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def tuple(self, *a: Type) -> TupleType` in type expression
+ mypy/typeshed/stdlib/multiprocessing/managers.pyi:326:39: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Overload[(self, sequence: Sequence[_T@list], /) -> ListProxy[_T@list], (self) -> ListProxy[Any]]` in type expression
+ mypy/typeshed/stdlib/multiprocessing/managers.pyi:328:39: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Overload[(self, sequence: Sequence[_T@list], /) -> ListProxy[_T@list], (self) -> ListProxy[Any]]` in type expression
- mypy/typeshed/stdlib/typing.pyi:289:35: warning[]8;;https://ty.dev/rules#unused-ignore-comment\unused-ignore-comment]8;;\] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/typing.pyi:301:35: warning[]8;;https://ty.dev/rules#unused-ignore-comment\unused-ignore-comment]8;;\] Unused blanket `type: ignore` directive
+ mypy/typeshed/stdlib/typing.pyi:411:40: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:411:61: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:832:16: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:978:29: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:978:64: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:985:45: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:993:22: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:994:14: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:998:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:1019:16: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:1020:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:1021:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:1025:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:1027:28: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing.pyi:1028:27: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing_extensions.pyi:646:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing_extensions.pyi:655:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing_extensions.pyi:674:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypy/typeshed/stdlib/typing_extensions.pyi:685:17: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypyc/test-data/fixtures/typing-full.pyi:137:28: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Literal[0]` in type expression
+ mypyc/test-data/fixtures/typing-full.pyi:139:34: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypyc/test-data/fixtures/typing-full.pyi:139:53: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_SpecialForm` in type expression
+ mypyc/test-data/fixtures/typing-full.pyi:141:33: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `Literal[0]` in type expression
- Found 1583 diagnostics
+ Found 1607 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ test/mitmproxy/proxy/layers/http/test_http3.py:89:41: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def Placeholder[T](cls: @Todo = typing.Any) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
+ test/mitmproxy/proxy/layers/http/test_http3.py:91:35: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def Placeholder[T](cls: @Todo = typing.Any) -> T@Placeholder | _Placeholder[T@Placeholder]` in type expression
- Found 1843 diagnostics
+ Found 1845 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/_gp/optim_mixed.py:116:16: error[]8;;https://ty.dev/rules#invalid-return-type\invalid-return-type]8;;\] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[Any]], int | float, bool]`, found `tuple[@Todo, ndarray[Unknown, dtype[Any]], Literal[True]]`
+ optuna/_gp/optim_mixed.py:116:16: error[]8;;https://ty.dev/rules#invalid-return-type\invalid-return-type]8;;\] Return type does not match returned value: expected `tuple[ndarray[Unknown, dtype[Any]], int | float, bool]`, found `tuple[Unknown, ndarray[Unknown, dtype[Any]], Literal[True]]`
- optuna/importance/_ped_anova/scott_parzen_estimator.py:110:12: error[]8;;https://ty.dev/rules#invalid-return-type\invalid-return-type]8;;\] Return type does not match returned value: expected `tuple[int, ndarray[Unknown, dtype[Any]]]`, found `tuple[@Todo, signedinteger[Unknown]]`
+ optuna/importance/_ped_anova/scott_parzen_estimator.py:110:12: error[]8;;https://ty.dev/rules#invalid-return-type\invalid-return-type]8;;\] Return type does not match returned value: expected `tuple[int, ndarray[Unknown, dtype[Any]]]`, found `tuple[Unknown, signedinteger[Unknown]]`

koda-validate (https://github.com/keithasaurus/koda-validate)
+ koda_validate/maybe.py:35:68: warning[]8;;https://ty.dev/rules#unused-ignore-comment\unused-ignore-comment]8;;\] Unused blanket `type: ignore` directive
+ koda_validate/maybe.py:47:68: warning[]8;;https://ty.dev/rules#unused-ignore-comment\unused-ignore-comment]8;;\] Unused blanket `type: ignore` directive
- Found 71 diagnostics
+ Found 73 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_socket.py:625:19: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/trio/_socket.py:886:19: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/trio/_sync.py:88:13: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def set(self) -> None` in type expression
+ src/trio/testing/_raises_group.py:50:21: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/trio/testing/_raises_group.py:54:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/trio/testing/_raises_group.py:60:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ src/trio/testing/_raises_group.py:73:23: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
- Found 522 diagnostics
+ Found 529 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- src/tests/test_static_typing.py:44:13: error[]8;;https://ty.dev/rules#type-assertion-failure\type-assertion-failure]8;;\] Type `str` does not match asserted type `@Todo`
+ src/tests/test_static_typing.py:44:13: error[]8;;https://ty.dev/rules#type-assertion-failure\type-assertion-failure]8;;\] Type `str` does not match asserted type `Unknown`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ gridfs/asynchronous/grid_file.py:287:75: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, session: AsyncClientSession | None = None) -> CoroutineType[Any, Any, Unknown]` in type expression
+ gridfs/synchronous/grid_file.py:287:64: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, session: ClientSession | None = None) -> Unknown` in type expression
- Found 472 diagnostics
+ Found 474 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/core/indexing.py:400:29: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ xarray/core/indexing.py:406:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
- xarray/core/missing.py:324:22: warning[]8;;https://ty.dev/rules#possibly-missing-attribute\possibly-missing-attribute]8;;\] Attribute `name` may be missing on object of type `Unknown | Variable | ndarray[@Todo, @Todo]`
+ xarray/core/missing.py:324:22: warning[]8;;https://ty.dev/rules#possibly-missing-attribute\possibly-missing-attribute]8;;\] Attribute `name` may be missing on object of type `Unknown | Variable | ndarray[@Todo, Unknown]`
- Found 1761 diagnostics
+ Found 1763 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/ext/commands/bot.py:89:33: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias` in type expression
- Found 477 diagnostics
+ Found 478 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/types/field.py:54:29: error[]8;;https://ty.dev/rules#unsupported-operator\unsupported-operator]8;;\] Operator `|` is unsupported between objects of type `object` and `object`
- Found 380 diagnostics
+ Found 379 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/disasm/riscv.py:123:13: error[]8;;https://ty.dev/rules#unresolved-reference\unresolved-reference]8;;\] Name `RISCV_INS_AUIPC` used when not defined
+ pwndbg/aglib/disasm/riscv.py:125:13: error[]8;;https://ty.dev/rules#unresolved-reference\unresolved-reference]8;;\] Name `RISCV_INS_C_MV` used when not defined
+ pwndbg/aglib/disasm/riscv.py:127:13: error[]8;;https://ty.dev/rules#unresolved-reference\unresolved-reference]8;;\] Name `RISCV_INS_C_LI` used when not defined
+ pwndbg/aglib/disasm/riscv.py:129:13: error[]8;;https://ty.dev/rules#unresolved-reference\unresolved-reference]8;;\] Name `RISCV_INS_LUI` used when not defined
+ pwndbg/aglib/disasm/riscv.py:130:13: error[]8;;https://ty.dev/rules#unresolved-reference\unresolved-reference]8;;\] Name `RISCV_INS_C_LUI` used when not defined
- Found 2684 diagnostics
+ Found 2689 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[]8;;https://ty.dev/rules#no-matching-overload\no-matching-overload]8;;\] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ pkg_resources/__init__.py:815:20: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
+ pkg_resources/__init__.py:825:20: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
+ pkg_resources/__init__.py:842:44: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
+ pkg_resources/__init__.py:948:20: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
+ pkg_resources/__init__.py:957:20: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
+ pkg_resources/__init__.py:972:44: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
+ pkg_resources/__init__.py:1226:20: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
+ pkg_resources/__init__.py:1241:44: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
+ pkg_resources/__init__.py:1274:20: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
+ pkg_resources/__init__.py:1294:11: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `GenericAlias | @Todo` in type expression
- Found 1165 diagnostics
+ Found 1175 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
+ django-stubs/contrib/gis/gdal/envelope.pyi:19:21: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/envelope.pyi:21:21: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/envelope.pyi:23:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:106:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:106:47: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:108:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:108:46: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:111:42: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:114:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:114:30: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:134:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:134:31: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:134:37: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:136:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:136:30: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:136:36: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:149:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/gdal/geometries.pyi:151:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/collections.pyi:11:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/collections.pyi:13:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/coordseq.pyi:10:36: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/coordseq.pyi:32:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:9:36: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:12:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:12:31: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:14:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/linestring.pyi:14:30: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:12:30: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:18:25: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:18:31: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:18:37: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:20:24: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:20:30: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/gis/geos/polygon.pyi:20:36: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `property` in type expression
+ django-stubs/contrib/staticfiles/finders.pyi:10:39: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, ignore_patterns: Iterable[str] | None) -> Iterable[Any]` in type expression
+ django-stubs/contrib/staticfiles/finders.pyi:14:59: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, ignore_patterns: Iterable[str] | None) -> Iterable[Any]` in type expression
+ django-stubs/contrib/staticfiles/finders.pyi:18:16: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, ignore_patterns: Iterable[str] | None) -> Iterable[Any]` in type expression
+ django-stubs/contrib/staticfiles/finders.pyi:25:59: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, ignore_patterns: Iterable[str] | None) -> Iterable[Any]` in type expression
+ django-stubs/contrib/staticfiles/finders.pyi:31:11: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, ignore_patterns: Iterable[str] | None) -> Iterable[Any]` in type expression
+ django-stubs/contrib/staticfiles/finders.pyi:38:59: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, ignore_patterns: Iterable[str] | None) -> Iterable[Any]` in type expression
+ django-stubs/contrib/staticfiles/finders.pyi:47:59: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def list(self, ignore_patterns: Iterable[str] | None) -> Iterable[Any]` in type expression
- django-stubs/http/request.pyi:215:44: warning[]8;;https://ty.dev/rules#unused-ignore-comment\unused-ignore-comment]8;;\] Unused blanket `type: ignore` directive
+ django-stubs/utils/datastructures.pyi:81:23: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `def dict(self) -> Unknown` in type expression
- tests/assert_type/views/test_generic.py:15:1: error[]8;;https://ty.dev/rules#type-assertion-failure\type-assertion-failure]8;;\] Type `QuerySet[MyModel, MyModel] | None` does not match asserted type `@Todo | None`
+ tests/assert_type/views/test_generic.py:15:1: error[]8;;https://ty.dev/rules#type-assertion-failure\type-assertion-failure]8;;\] Type `QuerySet[MyModel, MyModel] | None` does not match asserted type `Unknown | None`
- Found 458 diagnostics
+ Found 499 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/_specs.pyi:72:25: error[]8;;https://ty.dev/rules#unsupported-operator\unsupported-operator]8;;\] Operator `|` is unsupported between objects of type `object` and `object`
+ src/bokeh/command/subcommand.py:61:23: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Invalid subscript of object of type `_UnspecifiedType` in type expression
- src/bokeh/core/property/visual.pyi:48:36: error[]8;;https://ty.dev/rules#invalid-type-form\invalid-type-form]8;;\] Variable of type `UnionType` is not allowed in a type expression
- src/bokeh/plotting/_plot.py:91:48: warning[]8;;https://ty.dev/rules#possibly-missing-attribute\possibly-missing-attribute]8;;\] Attribute `groups` may be missing on object of type `Range | tuple[int | float, int | float] | Sequence[str] | (@Todo & ~None)`
+ src/bokeh/plotting/_plot.py:91:48: warning[]8;;https://ty.dev/rules#possibly-missing-attribute\possibly-missing-attribute]8;;\] Attribute `groups` may be missing on object of type `Range | tuple[int | float, int | float] | Sequence[str] | (Unknown & ~None)`
- Found 650

... (truncated 80 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-17 20:22_

---

_Comment by @astral-sh-bot[bot] on 2025-11-17 20:28_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-type-form` | 222 | 1 | 0 |
| `invalid-argument-type` | 0 | 2 | 9 |
| `unused-ignore-comment` | 4 | 3 | 0 |
| `non-subscriptable` | 0 | 5 | 0 |
| `unresolved-reference` | 5 | 0 | 0 |
| `invalid-await` | 0 | 0 | 3 |
| `invalid-return-type` | 0 | 0 | 3 |
| `possibly-missing-attribute` | 0 | 0 | 3 |
| `type-assertion-failure` | 0 | 0 | 3 |
| `unsupported-operator` | 0 | 2 | 0 |
| `invalid-assignment` | 0 | 0 | 1 |
| **Total** | **231** | **13** | **22** |

**[Full report with detailed diff](https://cjm-unknown-subscript.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-unknown-subscript.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @carljm on 2025-11-17 20:56_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-17 20:56_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-11-18 00:25_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-18 00:25_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-11-18 02:01_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-18 02:01_

---

_Marked ready for review by @carljm on 2025-11-18 02:09_

---

_Review requested from @AlexWaygood by @carljm on 2025-11-18 02:09_

---

_Review requested from @sharkdp by @carljm on 2025-11-18 02:09_

---

_Review requested from @dcreager by @carljm on 2025-11-18 02:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/invalid.md`:102 on 2025-11-18 16:49_

I don't think this makes grammatical sense -- cannot subscript object of type _what_, exactly? 😄

Also, you can subscript `[1, 2, 3]`, right?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/literal.md`:333 on 2025-11-18 16:53_

that seems like a confusing diagnostic -- you _can_ subscript lots of instances of `_SpecialForm` in type expressions, just not this specific instance. Maybe something like this?

```suggestion
# error: [invalid-type-form] "Invalid subscript of object of type `_SpecialForm` in a type expression"
```

---

_@AlexWaygood approved on 2025-11-18 16:53_

---

_@carljm reviewed on 2025-11-18 17:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/invalid.md`:102 on 2025-11-18 17:08_

That's because it's a partial match on the message string, it's not the full message; I didn't feel like writing out the full types :)

I can include the full message, or at least enough to make it less confusing.

> Also, you can subscript `[1, 2, 3]`, right?

The full message includes "in a type expression"

---

_@carljm reviewed on 2025-11-18 17:09_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/annotations/literal.md`:333 on 2025-11-18 17:09_

Good suggestion, thanks!

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-11-18 17:56_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-11-18 17:56_

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 18:27_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@sharkdp reviewed on 2025-11-18 18:32_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:200 on 2025-11-18 18:32_

Working on it ... :smile: 

---

_@sharkdp approved on 2025-11-18 18:34_

---

_@carljm reviewed on 2025-11-18 18:42_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/implicit_type_aliases.md`:200 on 2025-11-18 18:42_

I was actually going to ask if you wanted these tests here or in a separate "generics" section, since they are testing the combination of both generic aliases and unions :) Feel free to move them if you want in your PR.

---

_Merged by @carljm on 2025-11-18 18:43_

---

_Closed by @carljm on 2025-11-18 18:43_

---

_Branch deleted on 2025-11-18 18:49_

---
