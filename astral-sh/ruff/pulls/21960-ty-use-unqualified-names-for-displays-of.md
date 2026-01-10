```yaml
number: 21960
title: "[ty] Use unqualified names for displays of `TypeAliasType`s and unbound `ParamSpec`s/`TypeVar`s"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: alex/typevar-display
created_at: 2025-12-13T16:09:40Z
updated_at: 2025-12-13T20:23:18Z
url: https://github.com/astral-sh/ruff/pull/21960
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Use unqualified names for displays of `TypeAliasType`s and unbound `ParamSpec`s/`TypeVar`s

---

_Pull request opened by @AlexWaygood on 2025-12-13 16:09_

## Summary

Currently if you create a TypeVar or a ParamSpec in a Python project in VSCode with the ty-vscode extension enabled, you get an inlay like this, which is verbose and pretty redundant IMO. Here's a screenshot from docstring-adder:

<img width="1890" height="178" alt="image" src="https://github.com/user-attachments/assets/fdc6f8b1-1a82-4e7b-a0ba-a29011256d29" />

@Gankra solved this for most trivial initializer calls in https://github.com/astral-sh/ruff/pull/21848, but not for TypeVars/ParamSpecs/TypeAliasTypes, because these have their own distinct `Type` variants in ty and the displays for these variants currently use the fully qualified names of these types rather than the unqualified names.

This PR switches those types to use the unqualified names in their displays, which means we hook into the machinery @Gankra added in #21848, and therefore the inlay hints go away.

I think we originally used the qualified names for some typing-module special forms because it matched the repr at runtime:

```pycon
>>> from typing import Literal
>>> Literal
typing.Literal
```

And then that became a pattern that we followed with other special typing-module types. But `Literal` itself no longer has ^that display in our model (following https://github.com/astral-sh/ruff/pull/21775), so the pattern has already been broken. And using the qualified names for these displays is inconsistent with our displays for most nominal types, which generally use the unqualified names by default.

## Test Plan

Mdtests updated; inlay snapshots added


---

_Review requested from @carljm by @AlexWaygood on 2025-12-13 16:09_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-13 16:09_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-13 16:09_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-13 16:09_

---

_Label `ty` added by @AlexWaygood on 2025-12-13 16:09_

---

_Label `diagnostics` added by @AlexWaygood on 2025-12-13 16:09_

---

_Renamed from "[ty] Use unqualified names for displays of `ParamSpec`, `TypeVar` and `TypeAliasType`" to "[ty] Use unqualified names for displays of `TypeAliasType`s and unbound `ParamSpec`s/`TypeVar`s" by @AlexWaygood on 2025-12-13 16:10_

---

_Comment by @astral-sh-bot[bot] on 2025-12-13 16:13_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-13 16:42:00.354281276 +0000
+++ new-output.txt	2025-12-13 16:42:04.015312025 +0000
@@ -43,11 +43,11 @@
 aliases_newtype.py:63:15: error[invalid-newtype] Wrong number of arguments in `NewType` creation, expected 2, found 3
 aliases_newtype.py:65:38: error[invalid-newtype] invalid base for `typing.NewType`: type `Any`
 aliases_type_statement.py:10:52: error[invalid-type-arguments] Too many type arguments: expected 2, got 3
-aliases_type_statement.py:17:1: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `bit_count`
+aliases_type_statement.py:17:1: error[unresolved-attribute] Object of type `TypeAliasType` has no attribute `bit_count`
 aliases_type_statement.py:19:1: error[call-non-callable] Object of type `TypeAliasType` is not callable
-aliases_type_statement.py:23:7: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `other_attrib`
-aliases_type_statement.py:26:18: error[invalid-base] Invalid class base with type `typing.TypeAliasType`
-aliases_type_statement.py:31:22: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `typing.TypeAliasType`
+aliases_type_statement.py:23:7: error[unresolved-attribute] Object of type `TypeAliasType` has no attribute `other_attrib`
+aliases_type_statement.py:26:18: error[invalid-base] Invalid class base with type `TypeAliasType`
+aliases_type_statement.py:31:22: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Divergent, ...]`, found `TypeAliasType`
 aliases_type_statement.py:37:22: error[invalid-type-form] Function calls are not allowed in type expressions
 aliases_type_statement.py:38:22: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_type_statement.py:39:22: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
@@ -68,7 +68,7 @@
 aliases_type_statement.py:82:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias3`
 aliases_type_statement.py:88:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias6`
 aliases_type_statement.py:89:1: error[cyclic-type-alias-definition] Cyclic definition of `RecursiveTypeAlias7`
-aliases_typealiastype.py:32:7: error[unresolved-attribute] Object of type `typing.TypeAliasType` has no attribute `other_attrib`
+aliases_typealiastype.py:32:7: error[unresolved-attribute] Object of type `TypeAliasType` has no attribute `other_attrib`
 aliases_typealiastype.py:52:40: error[invalid-type-form] Function calls are not allowed in type expressions
 aliases_typealiastype.py:53:40: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 aliases_typealiastype.py:54:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
@@ -580,7 +580,7 @@
 generics_syntax_scoping.py:44:17: error[unresolved-reference] Name `T` used when not defined
 generics_syntax_scoping.py:81:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((**P@decorator2) -> R@decorator2, /) -> (**P@decorator2) -> R@decorator2`
 generics_syntax_scoping.py:113:9: error[type-assertion-failure] Type `str` does not match asserted type `Literal[""]`
-generics_syntax_scoping.py:116:13: error[type-assertion-failure] Type `TypeVar` does not match asserted type `typing.TypeVar`
+generics_syntax_scoping.py:116:13: error[type-assertion-failure] Type `TypeVar` does not match asserted type `TypeVar`
 generics_syntax_scoping.py:121:9: error[type-assertion-failure] Type `int | float | complex` does not match asserted type `complex`
 generics_syntax_scoping.py:124:13: error[type-assertion-failure] Type `int | float | complex` does not match asserted type `complex`
 generics_type_erasure.py:38:16: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `Literal[""]`

```

</details>




---

_Comment by @AlexWaygood on 2025-12-13 16:14_

> ```diff
> -generics_syntax_scoping.py:116:13: error[type-assertion-failure] Type `TypeVar` does not match asserted type `typing.TypeVar`
> +generics_syntax_scoping.py:116:13: error[type-assertion-failure] Type `TypeVar` does not match asserted type `TypeVar`
> ```

uh, not our greatest-ever error message, but the existing one was already pretty dreadful

---

_Comment by @astral-sh-bot[bot] on 2025-12-13 16:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/raises.py:1002:43: error[invalid-type-arguments] Type `typing.TypeVar` is not assignable to upper bound `BaseException` of type variable `BaseExcT_co_default@RaisesExc`
+ src/_pytest/raises.py:1002:43: error[invalid-type-arguments] Type `TypeVar` is not assignable to upper bound `BaseException` of type variable `BaseExcT_co_default@RaisesExc`

scrapy (https://github.com/scrapy/scrapy)
- scrapy/utils/python.py:152:13: error[invalid-assignment] Invalid subscript assignment with key of type `_SelfT@new_method` and value of type `_T@memoizemethod_noargs` on object of type `WeakKeyDictionary[typing.TypeVar, _T@memoizemethod_noargs]`
+ scrapy/utils/python.py:152:13: error[invalid-assignment] Invalid subscript assignment with key of type `_SelfT@new_method` and value of type `_T@memoizemethod_noargs` on object of type `WeakKeyDictionary[TypeVar, _T@memoizemethod_noargs]`
- scrapy/utils/python.py:153:16: error[invalid-argument-type] Method `__getitem__` of type `bound method WeakKeyDictionary[typing.TypeVar, _T@memoizemethod_noargs].__getitem__(key: typing.TypeVar) -> _T@memoizemethod_noargs` cannot be called with key of type `_SelfT@new_method` on object of type `WeakKeyDictionary[typing.TypeVar, _T@memoizemethod_noargs]`
+ scrapy/utils/python.py:153:16: error[invalid-argument-type] Method `__getitem__` of type `bound method WeakKeyDictionary[TypeVar, _T@memoizemethod_noargs].__getitem__(key: TypeVar) -> _T@memoizemethod_noargs` cannot be called with key of type `_SelfT@new_method` on object of type `WeakKeyDictionary[TypeVar, _T@memoizemethod_noargs]`

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/_specs.pyi:122:22: error[non-subscriptable] Cannot subscript object of type `typing.TypeVar` with no `__getitem__` method
+ src/bokeh/_specs.pyi:122:22: error[non-subscriptable] Cannot subscript object of type `TypeVar` with no `__getitem__` method
- src/bokeh/_specs.pyi:194:26: error[non-subscriptable] Cannot subscript object of type `typing.TypeVar` with no `__getitem__` method
+ src/bokeh/_specs.pyi:194:26: error[non-subscriptable] Cannot subscript object of type `TypeVar` with no `__getitem__` method
- src/bokeh/core/property_aliases.pyi:65:25: error[non-subscriptable] Cannot subscript object of type `typing.TypeVar` with no `__getitem__` method
+ src/bokeh/core/property_aliases.pyi:65:25: error[non-subscriptable] Cannot subscript object of type `TypeVar` with no `__getitem__` method

ibis (https://github.com/ibis-project/ibis)
- ibis/common/tests/test_grounds.py:268:27: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `float`
+ ibis/common/tests/test_grounds.py:268:27: error[invalid-argument-type] Argument is incorrect: Expected `TypeVar`, found `float`

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/objecttools.py:474:12: error[invalid-return-type] Return type does not match returned value: expected `((**P@excmessage_decorator) -> T@excmessage_decorator, /) -> (**P@excmessage_decorator) -> T@excmessage_decorator`, found `(typing.TypeVar, /) -> typing.TypeVar`
+ hydpy/core/objecttools.py:474:12: error[invalid-return-type] Return type does not match returned value: expected `((**P@excmessage_decorator) -> T@excmessage_decorator, /) -> (**P@excmessage_decorator) -> T@excmessage_decorator`, found `(TypeVar, /) -> TypeVar`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/batch.py:787:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> Batch` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/batch.py:787:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> Batch` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/batch.py:788:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Batch` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/batch.py:788:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Batch` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/batch.py:789:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_getitem(key: Hashable) -> Batch` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/batch.py:789:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_getitem(key: Hashable) -> Batch` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/bus.py:683:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/bus.py:683:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/bus.py:684:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/bus.py:684:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/bus.py:685:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/bus.py:685:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3712:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> Self@drop) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/frame.py:3712:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> Self@drop) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/frame.py:3713:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_loc(key: Hashable) -> Self@drop) | @Todo` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3713:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_loc(key: Hashable) -> Self@drop) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3714:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_getitem(key: Hashable) -> Self@drop) | @Todo` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3714:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@drop._drop_getitem(key: Hashable) -> Self@drop) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3728:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@masked_array._extract_iloc_masked_array(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> MaskedArray[Any, Any]) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/frame.py:3728:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@masked_array._extract_iloc_masked_array(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> MaskedArray[Any, Any]) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/frame.py:3729:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@masked_array._extract_loc_masked_array(key: Hashable) -> MaskedArray[Any, Any]) | @Todo` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3729:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@masked_array._extract_loc_masked_array(key: Hashable) -> MaskedArray[Any, Any]) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3730:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@masked_array._extract_getitem_masked_array(key: Hashable) -> MaskedArray[Any, Any]) | @Todo` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3730:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@masked_array._extract_getitem_masked_array(key: Hashable) -> MaskedArray[Any, Any]) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3737:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@assign._extract_iloc_assign(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> FrameAssignILoc) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/frame.py:3737:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@assign._extract_iloc_assign(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 4 union elements) -> FrameAssignILoc) | @Todo` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/frame.py:3738:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@assign._extract_loc_assign(key: Hashable) -> FrameAssignILoc) | @Todo` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3738:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@assign._extract_loc_assign(key: Hashable) -> FrameAssignILoc) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/frame.py:3739:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@assign._extract_getitem_assign(key: Hashable) -> FrameAssignILoc) | @Todo` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/frame.py:3739:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `(bound method Self@assign._extract_getitem_assign(key: Hashable) -> FrameAssignILoc) | @Todo` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/index.py:713:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/index.py:713:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/index.py:714:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/index.py:714:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:780:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/series.py:780:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/series.py:781:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/series.py:781:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:782:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/series.py:782:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:791:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_iloc_mask(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@mask` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/series.py:791:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_iloc_mask(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@mask` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/series.py:792:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_loc_mask(key: Hashable) -> Self@mask` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/series.py:792:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_loc_mask(key: Hashable) -> Self@mask` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:793:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_loc_mask(key: Hashable) -> Self@mask` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/series.py:793:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@mask._extract_loc_mask(key: Hashable) -> Self@mask` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:802:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@masked_array._extract_iloc_masked_array(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> MaskedArray[Any, Any]` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/series.py:802:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@masked_array._extract_iloc_masked_array(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> MaskedArray[Any, Any]` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/series.py:803:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@masked_array._extract_loc_masked_array(key: Hashable) -> MaskedArray[Any, Any]` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/series.py:803:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@masked_array._extract_loc_masked_array(key: Hashable) -> MaskedArray[Any, Any]` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:804:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@masked_array._extract_loc_masked_array(key: Hashable) -> MaskedArray[Any, Any]` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/series.py:804:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@masked_array._extract_loc_masked_array(key: Hashable) -> MaskedArray[Any, Any]` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:814:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@assign._extract_iloc_assign(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> SeriesAssign` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/series.py:814:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@assign._extract_iloc_assign(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> SeriesAssign` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/series.py:815:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@assign._extract_loc_assign(key: Hashable) -> SeriesAssign` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/series.py:815:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@assign._extract_loc_assign(key: Hashable) -> SeriesAssign` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/series.py:816:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@assign._extract_loc_assign(key: Hashable) -> SeriesAssign` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/series.py:816:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@assign._extract_loc_assign(key: Hashable) -> SeriesAssign` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/yarn.py:426:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> typing.TypeVar` of type variable `TILocSelectorFunc`
+ static_frame/core/yarn.py:426:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_iloc(key: int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements) -> Self@drop` does not satisfy upper bound `(int | integer[Any] | ndarray[Any, Any] | ... omitted 3 union elements, /) -> TypeVar` of type variable `TILocSelectorFunc`
- static_frame/core/yarn.py:427:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/yarn.py:427:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`
- static_frame/core/yarn.py:428:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> typing.TypeVar` of type variable `TLocSelectorFunc`
+ static_frame/core/yarn.py:428:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `bound method Self@drop._drop_loc(key: Hashable) -> Self@drop` does not satisfy upper bound `(Hashable, /) -> TypeVar` of type variable `TLocSelectorFunc`

zulip (https://github.com/zulip/zulip)
- zerver/lib/rest.py:189:40: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `(...) -> HttpResponse`
+ zerver/lib/rest.py:189:40: error[invalid-argument-type] Argument is incorrect: Expected `TypeVar`, found `(...) -> HttpResponse`
- zerver/lib/rest.py:205:40: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `(...) -> HttpResponse`
+ zerver/lib/rest.py:205:40: error[invalid-argument-type] Argument is incorrect: Expected `TypeVar`, found `(...) -> HttpResponse`
- zerver/lib/rest.py:215:43: error[invalid-argument-type] Argument to function `process_as_post` is incorrect: Expected `(...) -> HttpResponse`, found `((...) -> HttpResponse) | ((...) -> HttpResponse) | typing.TypeVar`
+ zerver/lib/rest.py:215:43: error[invalid-argument-type] Argument to function `process_as_post` is incorrect: Expected `(...) -> HttpResponse`, found `((...) -> HttpResponse) | ((...) -> HttpResponse) | TypeVar`
- zerver/views/auth.py:1190:1: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `def api_get_server_settings(request: HttpRequest) -> HttpResponse`
+ zerver/views/auth.py:1190:1: error[invalid-argument-type] Argument is incorrect: Expected `TypeVar`, found `def api_get_server_settings(request: HttpRequest) -> HttpResponse`
- zerver/views/development/email_log.py:52:1: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `def generate_all_emails(request: HttpRequest) -> HttpResponse`
+ zerver/views/development/email_log.py:52:1: error[invalid-argument-type] Argument is incorrect: Expected `TypeVar`, found `def generate_all_emails(request: HttpRequest) -> HttpResponse`
- zerver/views/realm.py:580:1: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `def check_subdomain_available(request: HttpRequest, subdomain: str) -> HttpResponse`
+ zerver/views/realm.py:580:1: error[invalid-argument-type] Argument is incorrect: Expected `TypeVar`, found `def check_subdomain_available(request: HttpRequest, subdomain: str) -> HttpResponse`
- zerver/views/report.py:13:1: error[invalid-argument-type] Argument to function `csrf_exempt` is incorrect: Argument type `typing.TypeVar` does not satisfy upper bound `(...) -> Any` of type variable `_F`
+ zerver/views/report.py:13:1: error[invalid-argument-type] Argument to function `csrf_exempt` is incorrect: Argument type `TypeVar` does not satisfy upper bound `(...) -> Any` of type variable `_F`
- zerver/views/report.py:14:1: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `(...) -> Unknown`
+ zerver/views/report.py:14:1: error[invalid-argument-type] Argument is incorrect: Expected `TypeVar`, found `(...) -> Unknown`
- zerver/views/video_calls.py:352:1: error[invalid-argument-type] Argument to function `csrf_exempt` is incorrect: Argument type `typing.TypeVar` does not satisfy upper bound `(...) -> Any` of type variable `_F`
+ zerver/views/video_calls.py:352:1: error[invalid-argument-type] Argument to function `csrf_exempt` is incorrect: Argument type `TypeVar` does not satisfy upper bound `(...) -> Any` of type variable `_F`
- zerver/views/video_calls.py:353:1: error[invalid-argument-type] Argument is incorrect: Expected `typing.TypeVar`, found `(...) -> Unknown`
+ zerver/views/video_calls.py:353:1: error[invalid-argument-type] Argument is incorrect: Expected `TypeVar`, found `(...) -> Unknown`

egglog-python (https://github.com/egraphs-good/egglog-python)
- python/egglog/thunk.py:50:31: error[invalid-argument-type] Argument is incorrect: Expected `(...) -> typing.TypeVar`, found `(...) -> T@fn`
+ python/egglog/thunk.py:50:31: error[invalid-argument-type] Argument is incorrect: Expected `(...) -> TypeVar`, found `(...) -> T@fn`


```

</details>


No memory usage changes detected ✅



---

_@Gankra approved on 2025-12-13 20:19_

hell yeah

---

_Merged by @AlexWaygood on 2025-12-13 20:23_

---

_Closed by @AlexWaygood on 2025-12-13 20:23_

---

_Branch deleted on 2025-12-13 20:23_

---
