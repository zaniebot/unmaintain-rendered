```yaml
number: 19604
title: "[ty] Track different uses of legacy typevars, including context when rendering typevars"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/typevar-context
created_at: 2025-07-28T16:22:35Z
updated_at: 2025-08-01T16:20:34Z
url: https://github.com/astral-sh/ruff/pull/19604
synced_at: 2026-01-12T15:56:43Z
```

# [ty] Track different uses of legacy typevars, including context when rendering typevars

---

_@dcreager_

This PR introduces a few related changes:

- We now keep track of each time a legacy typevar is bound in a different generic context (e.g. class, function), and internally create a new `TypeVarInstance` for each usage. This means the rest of the code can now assume that salsa-equivalent `TypeVarInstance`s refer to the same typevar, even taking into account that legacy typevars can be used more than once.

- We also go ahead and track the binding context of PEP 695 typevars. That's _much_ easier to track since we have the binding context right there during type inference.

- With that in place, we can now include the name of the binding context when rendering typevars (e.g. `T@f` instead of `T`)

---

_Label `ty` added by @dcreager on 2025-07-28 16:22_

---

_Comment by @github-actions[bot] on 2025-07-28 16:24_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-01 16:10:16.545867493 +0000
+++ new-output.txt	2025-08-01 16:10:16.607867387 +0000
@@ -1,7 +1,7 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
-_directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
+_directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Spam`
 _directives_deprecated_library.py:41:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | float`
 _directives_deprecated_library.py:45:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 aliases_explicit.py:41:24: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
@@ -149,7 +149,7 @@
 callables_protocol.py:97:1: error[invalid-assignment] Object of type `def cb4_bad1(x: int) -> None` is not assignable to `Proto4`
 callables_protocol.py:121:1: error[invalid-assignment] Object of type `def cb6_bad1(*vals: bytes, *, max_len: int | None = None) -> list[bytes]` is not assignable to `NotProto6`
 callables_protocol.py:176:14: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Protocol`
-callables_protocol.py:179:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R`
+callables_protocol.py:179:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
 callables_subtyping.py:26:5: error[invalid-assignment] Object of type `(int, /) -> int` is not assignable to `(int | float, /) -> int | float`
 callables_subtyping.py:29:5: error[invalid-assignment] Object of type `(int | float, /) -> int | float` is not assignable to `(int, /) -> int`
 callables_subtyping.py:204:21: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Protocol`
@@ -189,10 +189,10 @@
 constructors_call_new.py:76:17: error[missing-argument] No argument provided for required parameter `x` of bound method `__init__`
 constructors_call_new.py:89:1: error[type-assertion-failure] Argument does not have asserted type `int | Class6`
 constructors_call_new.py:89:13: error[missing-argument] No argument provided for required parameter `x` of bound method `__init__`
-constructors_call_new.py:113:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Class8[list[T]]`
+constructors_call_new.py:113:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Class8[list[T@Class8]]`
 constructors_call_new.py:116:1: error[type-assertion-failure] Argument does not have asserted type `Class8[list[int]]`
 constructors_call_new.py:117:1: error[type-assertion-failure] Argument does not have asserted type `Class8[list[str]]`
-constructors_call_new.py:125:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
+constructors_call_new.py:125:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Class9`
 constructors_call_new.py:140:47: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Class11[int]`
 constructors_call_type.py:40:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of function `__new__`
 constructors_call_type.py:50:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of bound method `__init__`
@@ -201,7 +201,7 @@
 constructors_callable.py:37:1: error[type-assertion-failure] Argument does not have asserted type `Class1`
 constructors_callable.py:49:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:50:1: error[type-assertion-failure] Argument does not have asserted type `Class2`
-constructors_callable.py:57:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
+constructors_callable.py:57:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Class3`
 constructors_callable.py:63:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:64:1: error[type-assertion-failure] Argument does not have asserted type `Class3`
 constructors_callable.py:73:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
@@ -221,7 +221,7 @@
 constructors_callable.py:193:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:194:1: error[type-assertion-failure] Argument does not have asserted type `Class9`
 dataclasses_descriptors.py:23:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int | Desc1`
-dataclasses_descriptors.py:50:63: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[T] | T`
+dataclasses_descriptors.py:50:63: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[T@Desc2] | T@Desc2`
 dataclasses_descriptors.py:66:1: error[type-assertion-failure] Argument does not have asserted type `int`
 dataclasses_descriptors.py:67:1: error[type-assertion-failure] Argument does not have asserted type `str`
 dataclasses_final.py:27:1: error[invalid-assignment] Cannot assign to final attribute `final_classvar` on type `<class 'D'>`
@@ -270,7 +270,7 @@
 dataclasses_transform_class.py:106:24: error[unknown-argument] Argument `id` does not match any known parameter of bound method `__init__`
 dataclasses_transform_class.py:119:18: error[unknown-argument] Argument `id` does not match any known parameter of bound method `__init__`
 dataclasses_transform_class.py:119:24: error[unknown-argument] Argument `name` does not match any known parameter of bound method `__init__`
-dataclasses_transform_converter.py:25:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T`
+dataclasses_transform_converter.py:25:6: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@model_field`
 dataclasses_transform_converter.py:48:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter1() -> int`
 dataclasses_transform_converter.py:49:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Unknown, /) -> Unknown`, found `def bad_converter2(*, x: int) -> int`
 dataclasses_transform_converter.py:107:5: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 6
@@ -287,7 +287,7 @@
 dataclasses_transform_func.py:57:1: error[invalid-assignment] Object of type `Literal[3]` is not assignable to attribute `name` of type `str`
 dataclasses_transform_func.py:61:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
 dataclasses_transform_func.py:65:36: error[unknown-argument] Argument `salary` does not match any known parameter
-dataclasses_transform_func.py:77:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T`
+dataclasses_transform_func.py:77:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@create_model_frozen`
 dataclasses_transform_func.py:97:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
 dataclasses_transform_meta.py:60:36: error[unknown-argument] Argument `other_name` does not match any known parameter
 dataclasses_transform_meta.py:73:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
@@ -365,7 +365,7 @@
 generics_base_class.py:45:5: error[type-assertion-failure] Argument does not have asserted type `Iterator[int]`
 generics_base_class.py:49:22: error[too-many-positional-arguments] Too many positional arguments to class `LinkedList`: expected 1, got 2
 generics_base_class.py:61:18: error[too-many-positional-arguments] Too many positional arguments to class `MyDict`: expected 1, got 2
-generics_basic.py:34:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr` and `AnyStr`
+generics_basic.py:34:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr@concat` and `AnyStr@concat`
 generics_basic.py:139:5: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_basic.py:140:5: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_basic.py:157:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap1[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap1[str, int]`
@@ -392,7 +392,7 @@
 generics_defaults.py:91:26: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
 generics_defaults.py:94:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
 generics_defaults.py:95:1: error[type-assertion-failure] Argument does not have asserted type `@Todo`
-generics_defaults.py:127:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T4`
+generics_defaults.py:127:32: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T4@func1`
 generics_defaults.py:141:12: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
 generics_defaults.py:151:12: error[invalid-argument-type] `@Todo` is not a valid argument to `Generic`
 generics_defaults.py:154:49: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int | float, bool]`?
@@ -441,64 +441,64 @@
 generics_scoping.py:15:1: error[type-assertion-failure] Argument does not have asserted type `str`
 generics_scoping.py:42:1: error[type-assertion-failure] Argument does not have asserted type `str`
 generics_scoping.py:43:1: error[type-assertion-failure] Argument does not have asserted type `bytes`
-generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
+generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@ParentA`
 generics_self_advanced.py:18:1: error[type-assertion-failure] Argument does not have asserted type `ParentA`
 generics_self_advanced.py:19:1: error[type-assertion-failure] Argument does not have asserted type `ChildA`
-generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
-generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self]`
-generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[Self]`
-generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `Self | None`, found `LinkedList[int]`
-generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `Self | None`
-generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `Self | None`
-generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self`
-generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self`, found `Shape`
-generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self`, found `Shape`
+generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@ParentB`
+generics_self_advanced.py:35:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
+generics_self_advanced.py:36:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@ChildB]`
+generics_self_advanced.py:37:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
+generics_self_advanced.py:38:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
+generics_self_advanced.py:43:9: error[type-assertion-failure] Argument does not have asserted type `list[Self@ChildB]`
+generics_self_advanced.py:44:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
+generics_self_advanced.py:45:9: error[type-assertion-failure] Argument does not have asserted type `Self@ChildB`
+generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `Self@LinkedList | None`, found `LinkedList[int]`
+generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `Self@LinkedList | None`
+generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `Self@LinkedList | None`
+generics_self_basic.py:14:9: error[type-assertion-failure] Argument does not have asserted type `Self@Shape`
+generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@Shape`, found `Shape`
+generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@Shape`, found `Shape`
 generics_self_basic.py:51:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
 generics_self_basic.py:52:1: error[type-assertion-failure] Argument does not have asserted type `Circle`
 generics_self_basic.py:54:1: error[type-assertion-failure] Argument does not have asserted type `Shape`
 generics_self_basic.py:55:1: error[type-assertion-failure] Argument does not have asserted type `Circle`
-generics_self_basic.py:64:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
+generics_self_basic.py:64:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Container`
 generics_self_basic.py:67:26: error[invalid-type-form] Special form `typing.Self` expected no type parameter
 generics_self_basic.py:74:5: error[type-assertion-failure] Argument does not have asserted type `Container[int]`
 generics_self_basic.py:75:5: error[type-assertion-failure] Argument does not have asserted type `Container[str]`
-generics_self_basic.py:83:5: error[type-assertion-failure] Argument does not have asserted type `Container[T]`
+generics_self_basic.py:83:5: error[type-assertion-failure] Argument does not have asserted type `Container[T@object_with_generic_type]`
 generics_self_protocols.py:48:5: error[type-assertion-failure] Argument does not have asserted type `ShapeProtocol`
 generics_self_usage.py:73:14: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:73:23: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:76:6: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
-generics_self_usage.py:82:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
-generics_self_usage.py:86:16: error[invalid-return-type] Return type does not match returned value: expected `Self`, found `Foo3`
-generics_self_usage.py:98:22: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T`
+generics_self_usage.py:82:54: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Foo2`
+generics_self_usage.py:86:16: error[invalid-return-type] Return type does not match returned value: expected `Self@Foo3`, found `Foo3`
+generics_self_usage.py:98:22: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@Bar`
 generics_self_usage.py:101:15: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:103:12: error[invalid-base] Invalid class base with type `typing.Self`
 generics_self_usage.py:106:30: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
-generics_self_usage.py:111:19: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
-generics_self_usage.py:116:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
-generics_self_usage.py:121:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self`
-generics_self_usage.py:125:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[Self]`
-generics_syntax_compatibility.py:23:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `V | K`
-generics_syntax_compatibility.py:26:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `M | K`
+generics_self_usage.py:111:19: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Base`
+generics_self_usage.py:116:40: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@Base`
+generics_self_usage.py:121:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@MyMetaclass`
+generics_self_usage.py:125:37: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[Self@MyMetaclass]`
+generics_syntax_compatibility.py:23:38: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `V@ClassC | K@method1`
+generics_syntax_compatibility.py:26:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `M@method2 | K`
 generics_syntax_declarations.py:17:1: error[invalid-generic-class] Cannot both inherit from `typing.Generic` and use PEP 695 type variables
 generics_syntax_declarations.py:25:20: error[invalid-generic-class] Cannot both inherit from subscripted `Protocol` and use PEP 695 type variables
-generics_syntax_declarations.py:32:9: error[unresolved-attribute] Type `T` has no attribute `is_integer`
+generics_syntax_declarations.py:32:9: error[unresolved-attribute] Type `T@ClassD` has no attribute `is_integer`
 generics_syntax_declarations.py:48:17: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, int]`?
 generics_syntax_declarations.py:60:17: error[invalid-type-variable-constraints] TypeVar must have at least two constrained types
 generics_syntax_declarations.py:64:17: error[invalid-type-variable-constraints] TypeVar must have at least two constrained types
 generics_syntax_declarations.py:71:17: error[invalid-type-form] Variable of type `tuple[<class 'bytes'>, <class 'str'>]` is not allowed in a type expression
 generics_syntax_declarations.py:75:18: error[invalid-type-form] Int literals are not allowed in this context in a type expression
 generics_syntax_declarations.py:79:23: error[unresolved-reference] Name `S` used when not defined
-generics_syntax_infer_variance.py:21:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T`
-generics_syntax_infer_variance.py:24:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T]`
+generics_syntax_infer_variance.py:21:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@ShouldBeCovariant1`
+generics_syntax_infer_variance.py:24:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T@ShouldBeCovariant1]`
 generics_syntax_infer_variance.py:28:1: error[invalid-assignment] Object of type `ShouldBeCovariant1[int]` is not assignable to `ShouldBeCovariant1[int | float]`
 generics_syntax_infer_variance.py:29:1: error[invalid-assignment] Object of type `ShouldBeCovariant1[int | float]` is not assignable to `ShouldBeCovariant1[int]`
 generics_syntax_infer_variance.py:36:1: error[invalid-assignment] Object of type `ShouldBeCovariant2[int]` is not assignable to `ShouldBeCovariant2[int | float]`
 generics_syntax_infer_variance.py:37:1: error[invalid-assignment] Object of type `ShouldBeCovariant2[int | float]` is not assignable to `ShouldBeCovariant2[int]`
-generics_syntax_infer_variance.py:41:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `ShouldBeCovariant2[T]`
+generics_syntax_infer_variance.py:41:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `ShouldBeCovariant2[T@ShouldBeCovariant3]`
 generics_syntax_infer_variance.py:45:1: error[invalid-assignment] Object of type `ShouldBeCovariant3[int]` is not assignable to `ShouldBeCovariant3[int | float]`
 generics_syntax_infer_variance.py:46:1: error[invalid-assignment] Object of type `ShouldBeCovariant3[int | float]` is not assignable to `ShouldBeCovariant3[int]`
 generics_syntax_infer_variance.py:74:1: error[invalid-assignment] Object of type `ShouldBeCovariant5[int]` is not assignable to `ShouldBeCovariant5[int | float]`
@@ -519,9 +519,9 @@
 generics_syntax_infer_variance.py:156:1: error[invalid-assignment] Object of type `ShouldBeContravariant1[int | float]` is not assignable to `ShouldBeContravariant1[int]`
 generics_syntax_scoping.py:18:26: error[unresolved-reference] Name `T` used when not defined
 generics_syntax_scoping.py:35:7: error[unresolved-reference] Name `T` used when not defined
-generics_syntax_scoping.py:40:23: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((...) -> R, /) -> (...) -> R`
+generics_syntax_scoping.py:40:23: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((...) -> R@decorator1, /) -> (...) -> R@decorator1`
 generics_syntax_scoping.py:44:17: error[unresolved-reference] Name `T` used when not defined
-generics_syntax_scoping.py:81:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((...) -> R, /) -> (...) -> R`
+generics_syntax_scoping.py:81:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `((...) -> R@decorator2, /) -> (...) -> R@decorator2`
 generics_syntax_scoping.py:113:9: error[type-assertion-failure] Argument does not have asserted type `str`
 generics_syntax_scoping.py:116:13: error[type-assertion-failure] Argument does not have asserted type `TypeVar`
 generics_syntax_scoping.py:121:9: error[type-assertion-failure] Argument does not have asserted type `int | float | complex`
@@ -564,11 +564,11 @@
 generics_typevartuple_specialization.py:93:5: error[type-assertion-failure] Argument does not have asserted type `tuple[str, int]`
 generics_typevartuple_specialization.py:94:5: error[type-assertion-failure] Argument does not have asserted type `tuple[int | float]`
 generics_typevartuple_specialization.py:95:5: error[type-assertion-failure] Argument does not have asserted type `tuple[Any, *tuple[Any, ...]]`
-generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, T1, T2]`
+generics_typevartuple_specialization.py:130:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, T1@func7, T2@func7]`
 generics_typevartuple_specialization.py:135:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[()], str, bool]`
 generics_typevartuple_specialization.py:136:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[str], bool, int | float]`
 generics_typevartuple_specialization.py:137:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[str, bool], int | float, int]`
-generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, T1, T2, T3]`
+generics_typevartuple_specialization.py:143:39: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `tuple[@Todo, T1@func9, T2@func9, T3@func9]`
 generics_typevartuple_specialization.py:148:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[()], str, bool, int | float]`
 generics_typevartuple_specialization.py:149:5: error[type-assertion-failure] Argument does not have asserted type `tuple[tuple[bool], str, int | float, int]`
 generics_typevartuple_specialization.py:157:5: error[type-assertion-failure] Argument does not have asserted type `tuple[*tuple[int, ...], int]`
@@ -581,8 +581,8 @@
 generics_upper_bound.py:51:8: error[invalid-argument-type] Argument to function `longer` is incorrect: Argument type `Literal[3]` does not satisfy upper bound of type variable `ST`
 generics_upper_bound.py:51:11: error[invalid-argument-type] Argument to function `longer` is incorrect: Argument type `Literal[3]` does not satisfy upper bound of type variable `ST`
 generics_variance.py:14:6: error[invalid-legacy-type-variable] A legacy `typing.TypeVar` cannot be both covariant and contravariant
-generics_variance.py:26:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T_co]`
-generics_variance.py:57:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `B_co`
+generics_variance.py:26:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T_co@ImmutableList]`
+generics_variance.py:57:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `B_co@func`
 generics_variance.py:175:25: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
 generics_variance.py:175:35: error[non-subscriptable] Cannot subscript object of type `<class 'Co[T_co]'>` with no `__class_getitem__` method
 generics_variance.py:179:29: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
@@ -597,19 +597,19 @@
 generics_variance.py:196:5: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
 generics_variance.py:196:15: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
 generics_variance.py:196:25: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[T_contra]'>` with no `__class_getitem__` method
-generics_variance_inference.py:19:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T3`
+generics_variance_inference.py:19:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T3@ClassA`
 generics_variance_inference.py:24:5: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int, int, int]`
 generics_variance_inference.py:25:5: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int | float, int | float, int]`
 generics_variance_inference.py:26:5: error[invalid-assignment] Object of type `ClassA[int | float, int, int]` is not assignable to `ClassA[int | float, int, int | float]`
 generics_variance_inference.py:28:5: error[invalid-assignment] Object of type `ClassA[int, int | float, int | float]` is not assignable to `ClassA[int, int, int]`
 generics_variance_inference.py:29:5: error[invalid-assignment] Object of type `ClassA[int, int | float, int | float]` is not assignable to `ClassA[int, int, int | float]`
-generics_variance_inference.py:33:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T`
-generics_variance_inference.py:36:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T]`
+generics_variance_inference.py:33:42: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@ShouldBeCovariant1`
+generics_variance_inference.py:36:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T@ShouldBeCovariant1]`
 generics_variance_inference.py:40:1: error[invalid-assignment] Object of type `ShouldBeCovariant1[int]` is not assignable to `ShouldBeCovariant1[int | float]`
 generics_variance_inference.py:41:1: error[invalid-assignment] Object of type `ShouldBeCovariant1[int | float]` is not assignable to `ShouldBeCovariant1[int]`
 generics_variance_inference.py:48:1: error[invalid-assignment] Object of type `ShouldBeCovariant2[int]` is not assignable to `ShouldBeCovariant2[int | float]`
 generics_variance_inference.py:49:1: error[invalid-assignment] Object of type `ShouldBeCovariant2[int | float]` is not assignable to `ShouldBeCovariant2[int]`
-generics_variance_inference.py:53:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `ShouldBeCovariant2[T]`
+generics_variance_inference.py:53:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `ShouldBeCovariant2[T@ShouldBeCovariant3]`
 generics_variance_inference.py:57:1: error[invalid-assignment] Object of type `ShouldBeCovariant3[int]` is not assignable to `ShouldBeCovariant3[int | float]`
 generics_variance_inference.py:58:1: error[invalid-assignment] Object of type `ShouldBeCovariant3[int | float]` is not assignable to `ShouldBeCovariant3[int]`
 generics_variance_inference.py:66:1: error[invalid-assignment] Object of type `ShouldBeCovariant4[int]` is not assignable to `ShouldBeCovariant4[int | float]`
@@ -638,9 +638,9 @@
 literals_interactions.py:16:5: error[index-out-of-bounds] Index -5 is out of bounds for tuple `tuple[int, str, list[bool]]` with length 3
 literals_interactions.py:17:5: error[index-out-of-bounds] Index 4 is out of bounds for tuple `tuple[int, str, list[bool]]` with length 3
 literals_interactions.py:18:5: error[index-out-of-bounds] Index -4 is out of bounds for tuple `tuple[int, str, list[bool]]` with length 3
-literals_interactions.py:60:49: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Matrix[A, B]`
-literals_interactions.py:63:52: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Matrix[A, C]`
-literals_interactions.py:66:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Matrix[B, A]`
+literals_interactions.py:60:49: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Matrix[A@Matrix, B@Matrix]`
+literals_interactions.py:63:52: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Matrix[A@Matrix, C@__matmul__]`
+literals_interactions.py:66:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Matrix[B@Matrix, A@Matrix]`
 literals_interactions.py:106:35: error[invalid-argument-type] Argument to function `expects_bad_status` is incorrect: Expected `Literal["MALFORMED", "ABORTED"]`, found `str`
 literals_interactions.py:109:32: error[invalid-argument-type] Argument to function `expects_pending_status` is incorrect: Expected `Literal["PENDING"]`, found `str`
 literals_literalstring.py:23:51: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `LiteralString`
@@ -758,7 +758,7 @@
 overloads_evaluation.py:115:5: error[no-matching-overload] No overload of function `example2` matches arguments
 overloads_evaluation.py:161:5: error[type-assertion-failure] Argument does not have asserted type `Literal[0, 1]`
 overloads_evaluation.py:234:5: error[type-assertion-failure] Argument does not have asserted type `int`
-overloads_evaluation.py:291:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T`
+overloads_evaluation.py:291:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@example6`
 overloads_evaluation.py:322:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 protocols_class_objects.py:30:5: error[invalid-argument-type] Argument to function `fun` is incorrect: Expected `type[Proto]`, found `<class 'Concrete'>`
 protocols_class_objects.py:35:1: error[invalid-assignment] Object of type `<class 'Concrete'>` is not assignable to `type[Proto]`
@@ -781,14 +781,14 @@
 protocols_modules.py:47:1: error[invalid-assignment] Object of type `<module '_protocols_modules2'>` is not assignable to `Reporter1`
 protocols_modules.py:48:1: error[invalid-assignment] Object of type `<module '_protocols_modules2'>` is not assignable to `Reporter2`
 protocols_modules.py:49:1: error[invalid-assignment] Object of type `<module '_protocols_modules2'>` is not assignable to `Reporter3`
-protocols_recursive.py:76:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T`
+protocols_recursive.py:76:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@func1`
 protocols_recursive.py:81:1: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 protocols_runtime_checkable.py:23:8: error[invalid-argument-type] Class `Proto1` cannot be used as the second argument to `isinstance`: This call will raise `TypeError` at runtime
 protocols_subtyping.py:16:6: error[call-non-callable] Cannot instantiate class `Proto1`: This call will raise `TypeError` at runtime
 protocols_subtyping.py:38:5: error[invalid-assignment] Object of type `Proto2` is not assignable to `Concrete2`
 protocols_subtyping.py:55:5: error[invalid-assignment] Object of type `Proto2` is not assignable to `Proto3`
 protocols_variance.py:84:16: error[invalid-argument-type] `ParamSpec` is not a valid argument to `Protocol`
-protocols_variance.py:85:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R`
+protocols_variance.py:85:62: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `R@__call__`
 qualifiers_annotated.py:43:17: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 qualifiers_annotated.py:44:17: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
 qualifiers_annotated.py:44:18: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
@@ -822,7 +822,7 @@
 specialtypes_never.py:19:22: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Never`
 specialtypes_never.py:32:12: error[invalid-return-type] Return type does not match returned value: expected `int`, found `Literal["whatever works"]`
 specialtypes_never.py:86:5: error[invalid-assignment] Object of type `list[Never]` is not assignable to `list[int]`
-specialtypes_never.py:105:12: error[invalid-return-type] Return type does not match returned value: expected `ClassC[U]`, found `ClassC[Never]`
+specialtypes_never.py:105:12: error[invalid-return-type] Return type does not match returned value: expected `ClassC[U@func10]`, found `ClassC[Never]`
 specialtypes_none.py:21:7: error[invalid-argument-type] Argument to function `func1` is incorrect: Expected `None`, found `<class 'NoneType'>`
 specialtypes_none.py:27:1: error[invalid-assignment] Object of type `None` is not assignable to `Iterable[Unknown]`
 specialtypes_none.py:32:1: error[missing-argument] No argument provided for required parameter `value` of function `__eq__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-07-28 16:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_core/_contextmanagers.py:72:16: error[invalid-return-type] Return type does not match returned value: expected `_T_co`, found `object`
+ src/anyio/_core/_contextmanagers.py:72:16: error[invalid-return-type] Return type does not match returned value: expected `_T_co@__enter__`, found `object`
- src/anyio/_core/_contextmanagers.py:165:16: error[invalid-return-type] Return type does not match returned value: expected `_T_co`, found `object`
+ src/anyio/_core/_contextmanagers.py:165:16: error[invalid-return-type] Return type does not match returned value: expected `_T_co@__aenter__`, found `object`
+ src/anyio/_core/_fileio.py:201:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Argument type `AnyStr@wrap_file` does not satisfy constraints of type variable `AnyStr`
+ src/anyio/from_thread.py:291:16: error[invalid-return-type] Return type does not match returned value: expected `T_Retval@call`, found `T_Retval@call`
- Found 86 diagnostics
+ Found 88 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.pyi:166:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Iterable[_T_co]'>]`
+ more_itertools/more.pyi:166:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Iterable[_T_co@_SizedIterable]'>]`
- more_itertools/more.pyi:169:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Reversible[_T_co]'>]`
+ more_itertools/more.pyi:169:1: error[inconsistent-mro] Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[typing.Protocol[_T_co], <class 'Sized'>, <class 'Reversible[_T_co@_SizedReversible]'>]`

bidict (https://github.com/jab/bidict)
- bidict/_base.py:519:16: error[invalid-return-type] Return type does not match returned value: expected `BT`, found `BidictBase[Any, Any]`
+ bidict/_base.py:519:16: error[invalid-return-type] Return type does not match returned value: expected `BT@__ror__`, found `BidictBase[Any, Any]`
- bidict/_bidict.py:41:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT, KT]`
+ bidict/_bidict.py:41:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT@MutableBidict, KT@MutableBidict]`
- bidict/_bidict.py:44:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT, KT]`
+ bidict/_bidict.py:44:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT@MutableBidict, KT@MutableBidict]`
- bidict/_bidict.py:185:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bidict[VT, KT]`
+ bidict/_bidict.py:185:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bidict[VT@bidict, KT@bidict]`
- bidict/_bidict.py:188:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bidict[VT, KT]`
+ bidict/_bidict.py:188:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bidict[VT@bidict, KT@bidict]`
- bidict/_frozen.py:34:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT, KT]`
+ bidict/_frozen.py:34:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT@frozenbidict, KT@frozenbidict]`
- bidict/_frozen.py:37:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT, KT]`
+ bidict/_frozen.py:37:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT@frozenbidict, KT@frozenbidict]`
- bidict/_orderedbase.py:138:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
+ bidict/_orderedbase.py:138:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT@OrderedBidictBase, KT@OrderedBidictBase]`
- bidict/_orderedbase.py:141:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
+ bidict/_orderedbase.py:141:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT@OrderedBidictBase, KT@OrderedBidictBase]`
- bidict/_orderedbidict.py:39:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT, KT]`
+ bidict/_orderedbidict.py:39:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT@OrderedBidict, KT@OrderedBidict]`
- bidict/_orderedbidict.py:42:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT, KT]`
+ bidict/_orderedbidict.py:42:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT@OrderedBidict, KT@OrderedBidict]`

beartype (https://github.com/beartype/beartype)
- beartype/_util/api/standard/utilfunctools.py:319:12: error[invalid-return-type] Return type does not match returned value: expected `BeartypeableT`, found `_lru_cache_wrapper[Unknown]`
+ beartype/_util/api/standard/utilfunctools.py:319:12: error[invalid-return-type] Return type does not match returned value: expected `BeartypeableT@beartype_functools_lru_cache`, found `_lru_cache_wrapper[Unknown]`
- beartype/_util/cache/utilcachecall.py:180:34: error[unresolved-attribute] Type `CallableT & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:180:34: error[unresolved-attribute] Type `CallableT@callable_cached & ((...) -> object)` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:417:34: error[unresolved-attribute] Type `CallableT & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:417:34: error[unresolved-attribute] Type `CallableT@method_cached_arg_by_id & ((...) -> object)` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:568:44: error[unresolved-attribute] Type `CallableT & ((...) -> object)` has no attribute `__name__`
+ beartype/_util/cache/utilcachecall.py:568:44: error[unresolved-attribute] Type `CallableT@property_cached & ((...) -> object)` has no attribute `__name__`

pegen (https://github.com/we-like-parsers/pegen)
- src/pegen/parser.py:28:19: error[unresolved-attribute] Type `F` has no attribute `__name__`
+ src/pegen/parser.py:28:19: error[unresolved-attribute] Type `F@logger` has no attribute `__name__`
- src/pegen/parser.py:48:19: error[unresolved-attribute] Type `F` has no attribute `__name__`
+ src/pegen/parser.py:48:19: error[unresolved-attribute] Type `F@memoize` has no attribute `__name__`
- src/pegen/parser.py:85:19: error[unresolved-attribute] Type `(P, /) -> T | None` has no attribute `__name__`
+ src/pegen/parser.py:85:19: error[unresolved-attribute] Type `(P@memoize_left_rec, /) -> T@memoize_left_rec | None` has no attribute `__name__`

pyp (https://github.com/hauntsaninja/pyp)
+ tests/test_pyp.py:45:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `StringIO`
- Found 6 diagnostics
+ Found 7 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ pyinstrument/util.py:65:30: error[invalid-argument-type] Argument to function `file_is_a_tty` is incorrect: Argument type `AnyStr@file_supports_color` does not satisfy constraints of type variable `AnyStr`
- Found 40 diagnostics
+ Found 41 diagnostics

Expression (https://github.com/cognitedata/Expression)
- expression/collections/block.py:271:16: error[invalid-return-type] Return type does not match returned value: expected `_TSourceSum | Literal[0]`, found `int`
+ expression/collections/block.py:271:16: error[invalid-return-type] Return type does not match returned value: expected `_TSourceSum@sum | Literal[0]`, found `int`
- expression/collections/block.py:934:12: error[invalid-return-type] Return type does not match returned value: expected `_TSourceSum | Literal[0]`, found `int`
+ expression/collections/block.py:934:12: error[invalid-return-type] Return type does not match returned value: expected `_TSourceSum@sum | Literal[0]`, found `int`
+ expression/collections/seq.py:444:24: error[invalid-argument-type] Argument is incorrect: Expected `_TSource@choose`, found `_TSource@mapper`
- expression/collections/seq.py:637:41: error[invalid-argument-type] Argument to function `init_infinite` is incorrect: Expected `(int, /) -> Unknown`, found `def identity(value: _A) -> _A`
+ expression/collections/seq.py:637:41: error[invalid-argument-type] Argument to function `init_infinite` is incorrect: Expected `(int, /) -> Unknown`, found `def identity(value: _A@identity) -> _A@identity`
+ expression/core/option.py:221:16: error[invalid-return-type] Return type does not match returned value: expected `Option[_TSource@of_optional]`, found `Option[_TSource@of_optional | None]`
- expression/core/result.py:85:73: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `_TSource | _TSourceOut`
+ expression/core/result.py:85:73: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `_TSource@default_with | _TSourceOut@Result`
- expression/core/result.py:97:65: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult, _TErrorOut]`
+ expression/core/result.py:97:65: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult@map, _TErrorOut@Result]`
- expression/core/result.py:113:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult, _TErrorOut]`
+ expression/core/result.py:113:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult@map2, _TErrorOut@Result]`
- expression/core/result.py:125:70: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TSourceOut, _TResult]`
+ expression/core/result.py:125:70: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TSourceOut@Result, _TResult@map_error]`
- expression/core/result.py:137:86: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult, _TErrorOut]`
+ expression/core/result.py:137:86: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult@bind, _TErrorOut@Result]`
- expression/core/result.py:177:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TSourceOut, _TErrorOut]`
+ expression/core/result.py:177:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TSourceOut@Result, _TErrorOut@Result]`
- expression/core/result.py:191:23: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `dict[str, _TSourceOut | _TErrorOut | Literal["ok", "error"]]`
+ expression/core/result.py:191:23: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `dict[str, _TSourceOut@Result | _TErrorOut@Result | Literal["ok", "error"]]`
- expression/core/result.py:205:23: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TErrorOut, _TSourceOut]`
+ expression/core/result.py:205:23: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TErrorOut@Result, _TSourceOut@Result]`
- expression/effect/async_result.py:32:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult, _TError]`
+ expression/effect/async_result.py:32:10: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TResult@bind, _TError@AsyncResultBuilder]`
- expression/effect/async_result.py:79:94: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TSource, _TError]`
+ expression/effect/async_result.py:79:94: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `Result[_TSource@AsyncResultBuilder, _TError@AsyncResultBuilder]`
+ expression/extra/option/pipeline.py:91:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Some(value: _T1@Some) -> Option[_T1@Some], Unknown, /) -> def Some(value: _T1@Some) -> Option[_T1@Some]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
- expression/extra/option/pipeline.py:91:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Some(value: _T1) -> Option[_T1], Unknown, /) -> def Some(value: _T1) -> Option[_T1]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
- expression/extra/parser.py:247:32: error[invalid-argument-type] Argument to function `preturn` is incorrect: Expected `(_A, /) -> (_B, /) -> _C`, found `((_A, /) -> (_B, /) -> _C, /) -> (_B, /) -> _C`
+ expression/extra/result/catch.py:50:16: error[invalid-return-type] Return type does not match returned value: expected `((...) -> _TSource@catch, /) -> ((...) -> Result[_TSource@catch, _TError@catch]) | Result[_TSource@catch, _TError@catch]`, found `(...) -> Result[_TSource@catch, _TError@catch]`
+ expression/extra/result/catch.py:50:26: error[invalid-argument-type] Argument to function `decorator` is incorrect: Expected `(...) -> _TSource@catch`, found `(...) -> _TSource@catch`
+ expression/extra/result/catch.py:52:12: error[invalid-return-type] Return type does not match returned value: expected `((...) -> _TSource@catch, /) -> ((...) -> Result[_TSource@catch, _TError@catch]) | Result[_TSource@catch, _TError@catch]`, found `def decorator(fn: (...) -> _TSource@catch) -> (...) -> Result[_TSource@catch, _TError@catch]`
- expression/extra/result/pipeline.py:96:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Ok(value: _TSource) -> Result[_TSource, Any], Unknown, /) -> def Ok(value: _TSource) -> Result[_TSource, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
+ expression/extra/result/pipeline.py:96:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Ok(value: _TSource@Ok) -> Result[_TSource@Ok, Any], Unknown, /) -> def Ok(value: _TSource@Ok) -> Result[_TSource@Ok, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
- expression/extra/result/traversable.py:50:21: error[invalid-argument-type] Argument to function `traverse` is incorrect: Expected `(Result[_TSource, _TError], /) -> Result[Unknown, Unknown]`, found `def identity(value: _A) -> _A`
+ expression/extra/result/traversable.py:50:21: error[invalid-argument-type] Argument to function `traverse` is incorrect: Expected `(Result[_TSource@sequence, _TError@sequence], /) -> Result[Unknown, Unknown]`, found `def identity(value: _A@identity) -> _A@identity`
- expression/extra/result/traversable.py:50:31: error[invalid-argument-type] Argument to function `traverse` is incorrect: Expected `Block[Result[_TSource, _TError]]`, found `Block[Result[Result[_TSource, _TError], Unknown]]`
- tests/test_array.py:357:1: error[invalid-assignment] Object of type `def singleton(value: _TSource) -> TypedArray[_TSource]` is not assignable to `(int, /) -> TypedArray[int]`
+ tests/test_array.py:357:1: error[invalid-assignment] Object of type `def singleton(value: _TSource@singleton) -> TypedArray[_TSource@singleton]` is not assignable to `(int, /) -> TypedArray[int]`
- tests/test_block.py:335:1: error[invalid-assignment] Object of type `def singleton(value: _TSource) -> Block[_TSource]` is not assignable to `(int, /) -> Block[int]`
+ tests/test_block.py:335:1: error[invalid-assignment] Object of type `def singleton(value: _TSource@singleton) -> Block[_TSource@singleton]` is not assignable to `(int, /) -> Block[int]`
- tests/test_map.py:49:19: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Map[str, int], /) -> Unknown`, found `def to_seq(table: Map[_Key, _Value]) -> Iterable[tuple[_Key, _Value]]`
+ tests/test_map.py:49:19: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Map[str, int], /) -> Unknown`, found `def to_seq(table: Map[_Key@to_seq, _Value@to_seq]) -> Iterable[tuple[_Key@to_seq, _Value@to_seq]]`
- tests/test_parser.py:232:9: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Parser[str], /) -> Unknown`, found `def many(parser: Parser[_A]) -> Parser[Block[_A]]`
+ tests/test_parser.py:232:9: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Parser[str], /) -> Unknown`, found `def many(parser: Parser[_A@many]) -> Parser[Block[_A@many]]`
- tests/test_result.py:487:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def swap(result: Result[_TSource, _TError]) -> Result[_TError, _TSource]`
+ tests/test_result.py:487:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def swap(result: Result[_TSource@swap, _TError@swap]) -> Result[_TError@swap, _TSource@swap]`
- tests/test_result.py:573:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def merge(result: Result[_TSource, _TSource]) -> _TSource`
+ tests/test_result.py:573:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def merge(result: Result[_TSource@merge, _TSource@merge]) -> _TSource@merge`
- tests/test_seq.py:308:20: error[invalid-argument-type] Argument to bound method `choose` is incorrect: Expected `(None | Literal[42], /) -> Option[Unknown]`, found `def of_optional(value: _TSource | None) -> Option[_TSource]`
+ tests/test_seq.py:308:20: error[invalid-argument-type] Argument to bound method `choose` is incorrect: Expected `(None | Literal[42], /) -> Option[Unknown]`, found `def of_optional(value: _TSource@of_optional | None) -> Option[_TSource@of_optional]`
- tests/test_seq.py:321:1: error[invalid-assignment] Object of type `def singleton(item: _TSource) -> Seq[_TSource]` is not assignable to `(int, /) -> Seq[int]`
+ tests/test_seq.py:321:1: error[invalid-assignment] Object of type `def singleton(item: _TSource@singleton) -> Seq[_TSource@singleton]` is not assignable to `(int, /) -> Seq[int]`
- Found 227 diagnostics
+ Found 230 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:3432:13: error[invalid-assignment] Object of type `(Sequence[@Todo(Inference of subscript on special form)] & ~Mapping[Unknown, Unknown]) | (Mapping[AnyKeyT, int | float] & ~Mapping[Unknown, Unknown])` is not assignable to `Sequence[Unknown] | AbstractSet[AnyKeyT]`
+ aioredis/client.py:3432:13: error[invalid-assignment] Object of type `(Sequence[@Todo(Inference of subscript on special form)] & ~Mapping[Unknown, Unknown]) | (Mapping[AnyKeyT@_zaggregate, int | float] & ~Mapping[Unknown, Unknown])` is not assignable to `Sequence[Unknown] | AbstractSet[AnyKeyT@_zaggregate]`

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `Self`, found `UpdateDictMixin[Any, Any]`
+ src/werkzeug/datastructures/mixins.py:238:28: error[invalid-argument-type] Argument is incorrect: Expected `Self@UpdateDictMixin`, found `UpdateDictMixin[Any, Any]`
- src/werkzeug/datastructures/mixins.py:278:18: error[invalid-super-argument] `Self` is not an instance or subclass of `<class 'UpdateDictMixin'>` in `super(<class 'UpdateDictMixin'>, Self)` call
+ src/werkzeug/datastructures/mixins.py:278:18: error[invalid-super-argument] `Self@UpdateDictMixin` is not an instance or subclass of `<class 'UpdateDictMixin'>` in `super(<class 'UpdateDictMixin'>, Self@UpdateDictMixin)` call
+ src/werkzeug/sansio/request.py:305:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
+ src/werkzeug/sansio/request.py:318:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `<class 'int'>`
+ src/werkzeug/sansio/request.py:503:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_set_header(value: str | None, on_update: ((HeaderSet, /) -> None) | None = None) -> HeaderSet`
+ src/werkzeug/sansio/response.py:336:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_age(value: str | None = None) -> timedelta | None`
+ src/werkzeug/sansio/response.py:355:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `<class 'int'>`
+ src/werkzeug/sansio/response.py:391:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
+ src/werkzeug/sansio/response.py:392:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def http_date(timestamp: date | int | float | struct_time | None = None) -> str`
+ src/werkzeug/sansio/response.py:404:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
+ src/werkzeug/sansio/response.py:405:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def http_date(timestamp: date | int | float | struct_time | None = None) -> str`
+ src/werkzeug/sansio/response.py:417:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
+ src/werkzeug/sansio/response.py:418:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def http_date(timestamp: date | int | float | struct_time | None = None) -> str`
+ src/werkzeug/sansio/response.py:715:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_set_header(value: str | None, on_update: ((HeaderSet, /) -> None) | None = None) -> HeaderSet`
+ src/werkzeug/sansio/response.py:716:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def dump_header(iterable: Iterable[Any]) -> str`
+ src/werkzeug/sansio/response.py:722:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_set_header(value: str | None, on_update: ((HeaderSet, /) -> None) | None = None) -> HeaderSet`
+ src/werkzeug/sansio/response.py:723:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def dump_header(iterable: Iterable[Any]) -> str`
+ src/werkzeug/sansio/response.py:734:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `def parse_set_header(value: str | None, on_update: ((HeaderSet, /) -> None) | None = None) -> HeaderSet`
+ src/werkzeug/sansio/response.py:735:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@header_property, /) -> str) | None`, found `def dump_header(iterable: Iterable[Any]) -> str`
+ src/werkzeug/sansio/response.py:741:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@header_property) | None`, found `<class 'int'>`
- src/werkzeug/utils.py:85:33: error[unresolved-attribute] Type `(Any, /) -> _T` has no attribute `__name__`
+ src/werkzeug/utils.py:85:33: error[unresolved-attribute] Type `(Any, /) -> _T@cached_property` has no attribute `__name__`
- src/werkzeug/utils.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `_T`, found `Any | _Missing`
+ src/werkzeug/utils.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_property`, found `Any | _Missing`
+ tests/test_utils.py:153:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_TAccessorValue@environ_property | None`, found `Literal["spam"]`
+ tests/test_utils.py:155:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@environ_property) | None`, found `<class 'int'>`
+ tests/test_utils.py:156:65: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@environ_property) | None`, found `<class 'int'>`
+ tests/test_utils.py:158:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((str, /) -> _TAccessorValue@environ_property) | None`, found `def parse_date(value: str | None) -> datetime | None`
+ tests/test_utils.py:158:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((_TAccessorValue@environ_property, /) -> str) | None`, found `def http_date(timestamp: date | int | float | struct_time | None = None) -> str`
- Found 367 diagnostics
+ Found 390 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
+ koda_validate/generic.py:152:56: error[invalid-argument-type] Argument is incorrect: Argument type `ExactMatchT@EqualsValidator` does not satisfy constraints of type variable `ExactMatchT`
- koda_validate/generic.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes`, found `str | bytes`
+ koda_validate/generic.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@Strip`, found `str | bytes`
- koda_validate/generic.py:330:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes`, found `str | bytes`
+ koda_validate/generic.py:330:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@UpperCase`, found `str | bytes`
- koda_validate/generic.py:336:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes`, found `str | bytes`
+ koda_validate/generic.py:336:16: error[invalid-return-type] Return type does not match returned value: expected `StrOrBytes@LowerCase`, found `str | bytes`
- Found 38 diagnostics
+ Found 39 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/pq/_debug.py:94:38: error[unresolved-attribute] Type `Func` has no attribute `__name__`
+ psycopg/psycopg/pq/_debug.py:94:38: error[unresolved-attribute] Type `Func@debugging` has no attribute `__name__`
- psycopg/psycopg/pq/pq_ctypes.py:94:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T`, found `ReferenceType[Unknown]`
+ psycopg/psycopg/pq/pq_ctypes.py:94:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T@py_object`, found `ReferenceType[Unknown]`
- psycopg_pool/psycopg_pool/pool.py:240:17: error[invalid-assignment] Object of type `WaitingClient[Connection[@Todo(Support for `typing.TypeAlias`)]]` is not assignable to `WaitingClient[CT]`
+ psycopg_pool/psycopg_pool/pool.py:240:17: error[invalid-assignment] Object of type `WaitingClient[Connection[@Todo(Support for `typing.TypeAlias`)]]` is not assignable to `WaitingClient[CT@ConnectionPool]`
- psycopg_pool/psycopg_pool/pool_async.py:268:17: error[invalid-assignment] Object of type `WaitingClient[AsyncConnection[@Todo(Support for `typing.TypeAlias`)]]` is not assignable to `WaitingClient[ACT]`
+ psycopg_pool/psycopg_pool/pool_async.py:268:17: error[invalid-assignment] Object of type `WaitingClient[AsyncConnection[@Todo(Support for `typing.TypeAlias`)]]` is not assignable to `WaitingClient[ACT@AsyncConnectionPool]`

kopf (https://github.com/nolar/kopf)
- kopf/_cogs/structs/dicts.py:241:25: warning[possibly-unbound-attribute] Attribute `obj` on type `(_T & Unknown & ~None) | (Iterable[_T] & Unknown) | (_T & _dummy) | (Iterable[_T] & _dummy)` is possibly unbound
+ kopf/_cogs/structs/dicts.py:241:25: warning[possibly-unbound-attribute] Attribute `obj` on type `(_T@walk & Unknown & ~None) | (Iterable[_T@walk] & Unknown) | (_T@walk & _dummy) | (Iterable[_T@walk] & _dummy)` is possibly unbound
- kopf/_kits/webhacks.py:81:26: error[invalid-argument-type] Argument to function `wraps` is incorrect: Argument type `(...) -> _RWrapped` does not satisfy upper bound of type variable `_ServerFn`
+ kopf/_kits/webhacks.py:81:26: error[invalid-argument-type] Argument to function `wraps` is incorrect: Argument type `(...) -> _RWrapped@wraps` does not satisfy upper bound of type variable `_ServerFn`

comtypes (https://github.com/enthought/comtypes)
+ comtypes/test/test_clear_cache.py:19:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
+ comtypes/test/test_client.py:81:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
+ comtypes/test/test_client.py:104:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
+ comtypes/test/test_client.py:305:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
+ comtypes/test/test_getactiveobj.py:8:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
+ comtypes/test/test_ienum.py:11:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
+ comtypes/test/test_imfattributes.py:11:41: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
+ comtypes/test/test_monikers.py:10:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
+ comtypes/test/test_storage.py:11:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
+ comtypes/test/test_viewobject.py:15:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`
- Found 407 diagnostics
+ Found 417 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_channel.py:203:50: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `SendType`
+ src/trio/_channel.py:203:50: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `SendType@MemorySendChannel`
- src/trio/_channel.py:546:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MemoryReceiveChannel[Unknown]`, found `MemorySendChannel[Unknown] | MemoryReceiveChannel[Unknown]`
+ src/trio/_channel.py:546:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MemoryReceiveChannel[Unknown]`, found `MemorySendChannel[T@as_safe_channel] | MemoryReceiveChannel[T@as_safe_channel]`
- src/trio/_core/_tests/test_io.py:65:16: error[unresolved-attribute] Type `(socket | int, /) -> RetT` has no attribute `__name__`
+ src/trio/_core/_tests/test_io.py:65:16: error[unresolved-attribute] Type `(socket | int, /) -> RetT@also_using_fileno` has no attribute `__name__`
- src/trio/_core/_tests/test_run.py:2668:5: error[invalid-assignment] Object of type `<class 'RuntimeError'> | RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2]]` is not assignable to `type[RuntimeError] | RaisesGroup[RuntimeError]`
+ src/trio/_core/_tests/test_run.py:2668:5: error[invalid-assignment] Object of type `<class 'RuntimeError'> | RaisesGroup[ExcT_1@__init__ | ExceptionGroup[ExcT_2@__init__]]` is not assignable to `type[RuntimeError] | RaisesGroup[RuntimeError]`
- src/trio/_path.py:77:16: error[invalid-return-type] Return type does not match returned value: expected `PathT`, found `Path`
+ src/trio/_path.py:77:16: error[invalid-return-type] Return type does not match returned value: expected `PathT@_wrap_method_path`, found `Path`
- src/trio/_socket.py:441:54: error[unresolved-attribute] Type `(...) -> T` has no attribute `__name__`
+ src/trio/_socket.py:441:54: error[unresolved-attribute] Type `(...) -> T@_make_simple_sock_method_wrapper` has no attribute `__name__`
- src/trio/_socket.py:446:71: error[unresolved-attribute] Type `(...) -> T` has no attribute `__name__`
+ src/trio/_socket.py:446:71: error[unresolved-attribute] Type `(...) -> T@_make_simple_sock_method_wrapper` has no attribute `__name__`
- src/trio/_tests/test_testing_raisesgroup.py:40:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/test_testing_raisesgroup.py:40:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:84:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[RuntimeError, ValueError]`
+ src/trio/_tests/test_testing_raisesgroup.py:84:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[RuntimeError, ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:140:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
+ src/trio/_tests/test_testing_raisesgroup.py:140:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ExceptionGroup[Exception]]`
- src/trio/_tests/test_testing_raisesgroup.py:140:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/test_testing_raisesgroup.py:140:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:343:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/test_testing_raisesgroup.py:343:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:384:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/test_testing_raisesgroup.py:384:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:435:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/test_testing_raisesgroup.py:435:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:996:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/test_testing_raisesgroup.py:996:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:1008:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/test_testing_raisesgroup.py:1008:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_testing_raisesgroup.py:1045:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[OSError]`
+ src/trio/_tests/test_testing_raisesgroup.py:1045:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[OSError]`
- src/trio/_tests/test_testing_raisesgroup.py:1111:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/test_testing_raisesgroup.py:1111:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/test_threads.py:89:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync(fn: (...) -> RetT, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT`
+ src/trio/_tests/test_threads.py:89:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync(fn: (...) -> RetT@from_thread_run_sync, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run_sync`
- src/trio/_tests/test_threads.py:96:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync(fn: (...) -> RetT, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT`
+ src/trio/_tests/test_threads.py:96:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync(fn: (...) -> RetT@from_thread_run_sync, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run_sync`
- src/trio/_tests/test_threads.py:104:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run(afn: (...) -> Awaitable[RetT], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT`
+ src/trio/_tests/test_threads.py:104:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run(afn: (...) -> Awaitable[RetT@from_thread_run], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run`
- src/trio/_tests/test_threads.py:112:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run(afn: (...) -> Awaitable[RetT], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT`
+ src/trio/_tests/test_threads.py:112:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run(afn: (...) -> Awaitable[RetT@from_thread_run], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run`
- src/trio/_tests/type_tests/raisesgroup.py:24:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/type_tests/raisesgroup.py:24:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:30:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/type_tests/raisesgroup.py:30:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:133:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/type_tests/raisesgroup.py:133:37: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:156:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ExceptionGroup[Exception]]`
+ src/trio/_tests/type_tests/raisesgroup.py:156:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ExceptionGroup[Exception]]`
- src/trio/_tests/type_tests/raisesgroup.py:156:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co]`, found `tuple[ValueError]`
+ src/trio/_tests/type_tests/raisesgroup.py:156:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[_ExceptionT_co@ExceptionGroup]`, found `tuple[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:169:5: error[invalid-assignment] Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2]]` is not assignable to `RaisesGroup[ValueError]`
+ src/trio/_tests/type_tests/raisesgroup.py:169:5: error[invalid-assignment] Object of type `RaisesGroup[ExcT_1@__init__ | ExceptionGroup[ExcT_2@__init__]]` is not assignable to `RaisesGroup[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:170:5: error[invalid-assignment] Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2]]` is not assignable to `RaisesGroup[ValueError]`
+ src/trio/_tests/type_tests/raisesgroup.py:170:5: error[invalid-assignment] Object of type `RaisesGroup[ExcT_1@__init__ | ExceptionGroup[ExcT_2@__init__]]` is not assignable to `RaisesGroup[ValueError]`
- src/trio/_tests/type_tests/raisesgroup.py:171:5: error[invalid-assignment] Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2]]` is not assignable to `RaisesGroup[ValueError]`
+ src/trio/_tests/type_tests/raisesgroup.py:171:5: error[invalid-assignment] Object of type `RaisesGroup[ExcT_1@__init__ | ExceptionGroup[ExcT_2@__init__]]` is not assignable to `RaisesGroup[ValueError]`
- src/trio/_util.py:191:9: error[unresolved-attribute] Unresolved attribute `__name__` on type `CallT`.
+ src/trio/_util.py:191:9: error[unresolved-attribute] Unresolved attribute `__name__` on type `CallT@async_wraps`.
- src/trio/_util.py:192:9: error[unresolved-attribute] Unresolved attribute `__qualname__` on type `CallT`.
+ src/trio/_util.py:192:9: error[unresolved-attribute] Unresolved attribute `__qualname__` on type `CallT@async_wraps`.
- src/trio/testing/_raises_group.py:543:9: error[invalid-super-argument] `RaisesGroup[ExcT_1 | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]]` is not an instance or subclass of `<class 'RaisesGroup'>` in `super(<class 'RaisesGroup'>, RaisesGroup[ExcT_1 | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]])` call
+ src/trio/testing/_raises_group.py:543:9: error[invalid-super-argument] `RaisesGroup[ExcT_1@__init__ | BaseExcT_1@__init__ | BaseExceptionGroup[BaseExcT_2@__init__]]` is not an instance or subclass of `<class 'RaisesGroup'>` in `super(<class 'RaisesGroup'>, RaisesGroup[ExcT_1@__init__ | BaseExcT_1@__init__ | BaseExceptionGroup[BaseExcT_2@__init__]])` call

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
+ bson/__init__.py:1097:12: error[invalid-return-type] Return type does not match returned value: expected `dict[str, Any] | _DocumentType@decode`, found `dict[str, Any] | _DocumentType@decode`
- Found 403 diagnostics
+ Found 404 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/abc.py:1943:9: error[invalid-parameter-default] Default value of type `<class 'VoiceClient'>` is not assignable to annotated parameter type `(Client, Connectable, /) -> T`
+ discord/abc.py:1943:9: error[invalid-parameter-default] Default value of type `<class 'VoiceClient'>` is not assignable to annotated parameter type `(Client, Connectable, /) -> T@connect`
- discord/app_commands/commands.py:455:5: error[unresolved-attribute] Unresolved attribute `__discord_app_commands_base_function__` on type `F`.
+ discord/app_commands/commands.py:455:5: error[unresolved-attribute] Unresolved attribute `__discord_app_commands_base_function__` on type `F@mark_overrideable`.
+ discord/app_commands/commands.py:2537:16: error[invalid-return-type] Return type does not match returned value: expected `T@guild_only | ((T@guild_only, /) -> T@guild_only)`, found `def inner(f: T@guild_only) -> T@guild_only`
+ discord/app_commands/commands.py:2539:16: error[invalid-return-type] Return type does not match returned value: expected `T@guild_only | ((T@guild_only, /) -> T@guild_only)`, found `T@guild_only`
+ discord/app_commands/commands.py:2539:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@guild_only`, found `T@guild_only & ~None`
+ discord/app_commands/commands.py:2594:16: error[invalid-return-type] Return type does not match returned value: expected `T@private_channel_only | ((T@private_channel_only, /) -> T@private_channel_only)`, found `def inner(f: T@private_channel_only) -> T@private_channel_only`
+ discord/app_commands/commands.py:2596:16: error[invalid-return-type] Return type does not match returned value: expected `T@private_channel_only | ((T@private_channel_only, /) -> T@private_channel_only)`, found `T@private_channel_only`
+ discord/app_commands/commands.py:2596:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@private_channel_only`, found `T@private_channel_only & ~None`
+ discord/app_commands/commands.py:2649:16: error[invalid-return-type] Return type does not match returned value: expected `T@dm_only | ((T@dm_only, /) -> T@dm_only)`, found `def inner(f: T@dm_only) -> T@dm_only`
+ discord/app_commands/commands.py:2651:16: error[invalid-return-type] Return type does not match returned value: expected `T@dm_only | ((T@dm_only, /) -> T@dm_only)`, found `T@dm_only`
+ discord/app_commands/commands.py:2651:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@dm_only`, found `T@dm_only & ~None`
+ discord/app_commands/commands.py:2744:16: error[invalid-return-type] Return type does not match returned value: expected `T@guild_install | ((T@guild_install, /) -> T@guild_install)`, found `def inner(f: T@guild_install) -> T@guild_install`
+ discord/app_commands/commands.py:2746:16: error[invalid-return-type] Return type does not match returned value: expected `T@guild_install | ((T@guild_install, /) -> T@guild_install)`, found `T@guild_install`
+ discord/app_commands/commands.py:2746:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@guild_install`, found `T@guild_install & ~None`
+ discord/app_commands/commands.py:2795:16: error[invalid-return-type] Return type does not match returned value: expected `T@user_install | ((T@user_install, /) -> T@user_install)`, found `def inner(f: T@user_install) -> T@user_install`
+ discord/app_commands/commands.py:2797:16: error[invalid-return-type] Return type does not match returned value: expected `T@user_install | ((T@user_install, /) -> T@user_install)`, found `T@user_install`
+ discord/app_commands/commands.py:2797:22: error[invalid-argument-type] Argument to function `inner` is incorrect: Expected `T@user_install`, found `T@user_install & ~None`
- discord/app_commands/tree.py:1229:9: error[unresolved-attribute] Unresolved attribute `_cs_command` on type `Interaction[ClientT]`.
+ discord/app_commands/tree.py:1229:9: error[unresolved-attribute] Unresolved attribute `_cs_command` on type `Interaction[ClientT@CommandTree]`.
- discord/app_commands/tree.py:1286:9: error[unresolved-attribute] Unresolved attribute `_cs_command` on type `Interaction[ClientT]`.
+ discord/app_commands/tree.py:1286:9: error[unresolved-attribute] Unresolved attribute `_cs_command` on type `Interaction[ClientT@CommandTree]`.
- discord/app_commands/tree.py:1293:9: error[unresolved-attribute] Unresolved attribute `_cs_namespace` on type `Interaction[ClientT]`.
+ discord/app_commands/tree.py:1293:9: error[unresolved-attribute] Unresolved attribute `_cs_namespace` on type `Interaction[ClientT@CommandTree]`.
- discord/backoff.py:63:42: error[invalid-parameter-default] Default value of type `Literal[False]` is not assignable to annotated parameter type `T`
+ discord/backoff.py:63:42: error[invalid-parameter-default] Default value of type `Literal[False]` is not assignable to annotated parameter type `T@ExponentialBackoff`
- discord/client.py:342:16: error[invalid-return-type] Return type does not match returned value: expected `ConnectionState[Self]`, found `ConnectionState[Client]`
+ discord/client.py:342:16: error[invalid-return-type] Return type does not match returned value: expected `ConnectionState[Self@Client]`, found `ConnectionState[Client]`
- discord/client.py:2099:23: error[unresolved-attribute] Type `CoroT` has no attribute `__name__`
+ discord/client.py:2099:23: error[unresolved-attribute] Type `CoroT@event` has no attribute `__name__`
- discord/client.py:2100:71: error[unresolved-attribute] Type `CoroT` has no attribute `__name__`
+ discord/client.py:2100:71: error[unresolved-attribute] Type `CoroT@event` has no attribute `__name__`
- discord/ext/commands/cog.py:266:5: error[unresolved-attribute] Unresolved attribute `__cog_special_method__` on type `FuncT`.
+ discord/ext/commands/cog.py:266:5: error[unresolved-attribute] Unresolved attribute `__cog_special_method__` on type `FuncT@_cog_special_method`.
- discord/ext/commands/cog.py:495:24: error[unresolved-attribute] Type `FuncT` has no attribute `__func__`
+ discord/ext/commands/cog.py:495:24: error[unresolved-attribute] Type `FuncT@_get_overridden_method` has no attribute `__func__`
- discord/ext/commands/cog.py:525:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__cog_listener__` on type `(FuncT & ~staticmethod) | ((...) -> Unknown)`
+ discord/ext/commands/cog.py:525:13: error[invalid-assignment] Object of type `Literal[True]` is not assignable to attribute `__cog_listener__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`
- discord/ext/commands/cog.py:526:33: error[unresolved-attribute] Type `(FuncT & ~staticmethod) | ((...) -> Unknown)` has no attribute `__name__`
+ discord/ext/commands/cog.py:526:33: error[unresolved-attribute] Type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)` has no attribute `__name__`
- discord/ext/commands/cog.py:528:17: error[unresolved-attribute] Type `(FuncT & ~staticmethod) | ((...) -> Unknown)` has no attribute `__cog_listener_names__`
+ discord/ext/commands/cog.py:528:17: error[unresolved-attribute] Type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)` has no attribute `__cog_listener_names__`
- discord/ext/commands/cog.py:530:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT & ~staticmethod) | ((...) -> Unknown)`
+ discord/ext/commands/cog.py:530:17: error[invalid-assignment] Object of type `list[Unknown]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`
- discord/ext/commands/converter.py:1121:13: error[invalid-assignment] Object of type `tuple[(tuple[T] & ~tuple[Unknown, ...]) | (T & ~tuple[Unknown, ...])]` is not assignable to `tuple[T] | T`
+ discord/ext/commands/converter.py:1121:13: error[invalid-assignment] Object of type `tuple[(tuple[T@__class_getitem__] & ~tuple[Unknown, ...]) | (T@__class_getitem__ & ~tuple[Unknown, ...])]` is not assignable to `tuple[T@__class_getitem__] | T@__class_getitem__`
- discord/ext/commands/converter.py:1122:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `tuple[T] | T`
+ discord/ext/commands/converter.py:1122:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `tuple[T@__class_getitem__] | T@__class_getitem__`
- discord/ext/commands/converter.py:1124:21: error[non-subscriptable] Cannot subscript object of type `T` with no `__getitem__` method
+ discord/ext/commands/converter.py:1124:21: error[non-subscriptable] Cannot subscript object of type `T@__class_getitem__` with no `__getitem__` method
- discord/ext/commands/core.py:2103:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[Unknown]`
+ discord/ext/commands/core.py:2103:12: error[invalid-return-type] Return type does not match returned value: expected `(T@has_any_role, /) -> T@has_any_role`, found `Check[Unknown]`
- discord/ext/commands/core.py:2136:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[Unknown]`
+ discord/ext/commands/core.py:2136:12: error[invalid-return-type] Return type does not match returned value: expected `(T@bot_has_role, /) -> T@bot_has_role`, found `Check[Unknown]`
- discord/ext/commands/core.py:2165:12: error[invalid-return-type] Return type does not match returned value: expected `(T, /) -> T`, found `Check[Unknown]`
+ discord/ext/commands/core.py:2165:12: error[invalid-return-type] Return type does not match returned value: expected `(T@bot_has_any_role, /) -> T@bot_has_any_role`, found `Check[Unknown]`
- discord/ext/commands/help.py:222:5: error[unresolved-attribute] Unresolved attribute `__help_command_not_overridden__` on type `FuncT`.
+ discord/ext/commands/help.py:222:5: error[unresolved-attribute] Unresolved attribute `__help_command_not_overridden__` on type `FuncT@_not_overridden`.
- discord/interactions.py:386:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionResponse[ClientT]`, found `InteractionResponse[Client]`
+ discord/interactions.py:386:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionResponse[ClientT@Interaction]`, found `InteractionResponse[Client]`
- discord/interactions.py:862:20: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT] | None`, found `InteractionCallbackResponse[Client]`
+ discord/interactions.py:862:20: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT@InteractionResponse] | None`, found `InteractionCallbackResponse[Client]`
- discord/interactions.py:1044:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT]`, found `InteractionCallbackResponse[Client]`
+ discord/interactions.py:1044:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT@InteractionResponse]`, found `InteractionCallbackResponse[Client]`
- discord/interactions.py:1188:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT] | None`, found `InteractionCallbackResponse[Client]`
+ discord/interactions.py:1188:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT@InteractionResponse] | None`, found `InteractionCallbackResponse[Client]`
- discord/interactions.py:1241:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT]`, found `InteractionCallbackResponse[Client]`
+ discord/interactions.py:1241:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT@InteractionResponse]`, found `InteractionCallbackResponse[Client]`
- discord/interactions.py:1338:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT]`, found `InteractionCallbackResponse[Client]`
+ discord/interactions.py:1338:16: error[invalid-return-type] Return type does not match returned value: expected `InteractionCallbackResponse[ClientT@InteractionResponse]`, found `InteractionCallbackResponse[Client]`
- Found 518 diagnostics
+ Found 533 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/directive.py:134:25: error[unresolved-attribute] Type `(...) -> T` has no attribute `__name__`
+ strawberry/directive.py:134:25: error[unresolved-attribute] Type `(...) -> T@directive` has no attribute `__name__`
+ strawberry/directive.py:138:50: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((...) -> T@StrawberryDirectiveResolver) | staticmethod | classmethod`, found `(...) -> T@directive`
- strawberry/federation/object_type.py:90:9: error[invalid-argument-type] Argument to function `type` is incorrect: Argument type `T | None` does not satisfy upper bound of type variable `T`
+ strawberry/federation/object_type.py:90:9: error[invalid-argument-type] Argument to function `type` is incorrect: Argument type `T@_impl_type | None` does not satisfy upper bound of type variable `T`
+ strawberry/types/enum.py:236:16: error[invalid-return-type] Return type does not match returned value: expected `EnumType@enum | ((EnumType@enum, /) -> EnumType@enum)`, found `def wrap(cls: EnumType@enum) -> EnumType@enum`
+ strawberry/types/enum.py:238:12: error[invalid-return-type] Return type does not match returned value: expected `EnumType@enum | ((EnumType@enum, /) -> EnumType@enum)`, found `EnumType@enum`
+ strawberry/types/enum.py:238:17: error[invalid-argument-type] Argument to function `wrap` is incorrect: Expected `EnumType@enum`, found `EnumType@enum & ~AlwaysFalsy`
- strawberry/types/object_type.py:157:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[type]`, found `T`
+ strawberry/types/object_type.py:157:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[type]`, found `T@_process_type`
+ strawberry/types/object_type.py:321:16: error[invalid-return-type] Return type does not match returned value: expected `T@type | ((T@type, /) -> T@type)`, found `def wrap(cls: T@type) -> T@type`
- strawberry/types/object_type.py:397:9: error[invalid-argument-type] Argument to function `type` is incorrect: Argument type `T | None` does not satisfy upper bound of type variable `T`
+ strawberry/types/object_type.py:397:9: error[invalid-argument-type] Argument to function `type` is incorrect: Argument type `T@input | None` does not satisfy upper bound of type variable `T`
- strawberry/types/object_type.py:470:9: error[invalid-argument-type] Argument to function `type` is incorrect: Argument type `T | None` does not satisfy upper bound of type variable `T`
+ strawberry/types/object_type.py:470:9: error[invalid-argument-type] Argument to function `type` is incorrect: Argument type `T@interface | None` does not satisfy upper bound of type variable `T`
- Found 369 diagnostics
+ Found 374 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/utils/datatypes.py:76:16: error[invalid-return-type] Return type does not match returned value: expected `AnyStr`, found `str | bytes`
+ scrapy/utils/datatypes.py:76:16: error[invalid-return-type] Return type does not match returned value: expected `AnyStr@normkey`, found `str | bytes`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/_internal/_utils.py:307:70: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T`
+ pydantic/_internal/_utils.py:307:70: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@LazyClassAttribute`
- pydantic/config.py:1207:9: error[unresolved-attribute] Unresolved attribute `__pydantic_config__` on type `_TypeT`.
+ pydantic/config.py:1207:9: error[unresolved-attribute] Unresolved attribute `__pydantic_config__` on type `_TypeT@with_config`.
+ pydantic/config.py:1210:12: error[invalid-return-type] Return type does not match returned value: expected `(_TypeT@with_config, /) -> _TypeT@with_config`, found `def inner(class_: _TypeT@with_config, /) -> _TypeT@with_config`
- pydantic/functional_serializers.py:298:12: error[invalid-return-type] Return type does not match returned value: expected `((_FieldWrapSerializerT, /) -> _FieldWrapSerializerT) | ((_FieldPlainSerializerT, /) -> _FieldPlainSerializerT)`, found `def dec(f: @Todo(Support for `typing.TypeAlias`)) -> PydanticDescriptorProxy[Any]`
+ pydantic/functional_serializers.py:298:12: error[invalid-return-type] Return type does not match returned value: expected `((_FieldWrapSerializerT@field_serializer, /) -> _FieldWrapSerializerT@field_serializer) | ((_FieldPlainSerializerT@field_serializer, /) -> _FieldPlainSerializerT@field_serializer)`, found `def dec(f: @Todo(Support for `typing.TypeAlias`)) -> PydanticDescriptorProxy[Any]`
- pydantic/functional_serializers.py:416:16: error[invalid-return-type] Return type does not match returned value: expected `_ModelPlainSerializerT | ((_ModelWrapSerializerT, /) -> _ModelWrapSerializerT) | ((_ModelPlainSerializerT, /) -> _ModelPlainSerializerT)`, found `def dec(f: @Todo(Support for `typing.TypeAlias`)) -> PydanticDescriptorProxy[Any]`
+ pydantic/functional_serializers.py:416:16: error[invalid-return-type] Return type does not match returned value: expected `_ModelPlainSerializerT@model_serializer | ((_ModelWrapSerializerT@model_serializer, /) -> _ModelWrapSerializerT@model_serializer) | ((_ModelPlainSerializerT@model_serializer, /) -> _ModelPlainSerializerT@model_serializer)`, found `def dec(f: @Todo(Support for `typing.TypeAlias`)) -> PydanticDescriptorProxy[Any]`
- pydantic/functional_se...*[Comment body truncated]*

---

_Comment by @codspeed-hq[bot] on 2025-07-28 16:40_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Ftypevar-context?runnerMode=Instrumentation)

### Merging #19604 will **degrade performances by 8.27%**

<sub>Comparing <code>dcreager/typevar-context</code> (06bf2e3) with <code>main</code> (48d5bd1)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 41` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` DateType `` | 221 ms | 241 ms | -8.27% |


---

_Marked ready for review by @dcreager on 2025-07-28 21:36_

---

_Review requested from @carljm by @dcreager on 2025-07-28 21:36_

---

_Review requested from @AlexWaygood by @dcreager on 2025-07-28 21:36_

---

_Review requested from @sharkdp by @dcreager on 2025-07-28 21:36_

---

_Review requested from @MichaReiser by @dcreager on 2025-07-28 21:36_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6602 on 2025-07-29 07:05_

It took me a while to understand the difference between `definition` and `binding_context`. I think it could help to add an example and add some comment explaining the difference between the two.


---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-29 07:08_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6629 on 2025-07-29 07:09_

I'm not very familiar with the design, and I haven't looked through all usages, so I don't feel like I've a good sense to judge whether it makes the most sense to have the `binding_context` here or not. 

I'm also just speculating here. I haven't checked whether the number of `TypeVarInstances` is significantly increasing, and if that is the main cause of the perf regression. 

Do you think that this new field leads to significantly less reuse of `TypeVarInstance`s, meaning, it leads to many more interned salsa struct (which requires storing an extra name and the overhead of salsa tracking).

 Would it make sense to split the struct into a `TypeVarInstance` and a `BoundTypeVarInstance` 

* `TypeVarInstance`: The same as before this PR?
* `BoundTypeVarInstance`: A regular Rust struct that wraps the `TypeVarInstance` and has the extra `binding_context` field. 

I'm sure that this isn't possible because of some size constraints that we have but I thought it's worth considering as an alternative design

---

_@MichaReiser reviewed on 2025-07-29 07:16_

I lack a bit of context on this change but I skimmed over the changes. I'm a bit concerned about the perf and memory regression (surprise surprise) this introduces but maybe this is just a bullet we need to take?

Can you provide some more context on the motivation for:

> With that in place, we can now include the name of the binding context when rendering typevars (e.g. T@f instead of T)

I find the notation a bit confusing. E.g. I find it a bit strange to see it in completions. Is this essential information for users? What's the motivation for always showing it? Do you know what other type checkers do here? Feel free to ignore this part if it's obvious to everyone else working on ty's typing and it's just me being out of the loop



---

_Comment by @github-actions[bot] on 2025-07-29 07:18_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 0 | 2,893 |
| `invalid-argument-type` | 113 | 4 | 1,376 |
| `possibly-unbound-attribute` | 0 | 0 | 512 |
| `invalid-return-type` | 40 | 0 | 242 |
| `invalid-assignment` | 2 | 3 | 42 |
| `invalid-parameter-default` | 1 | 0 | 8 |
| `call-non-callable` | 2 | 0 | 6 |
| `no-matching-overload` | 0 | 7 | 0 |
| `invalid-super-argument` | 0 | 0 | 6 |
| `type-assertion-failure` | 6 | 0 | 0 |
| `unsupported-operator` | 0 | 0 | 6 |
| `inconsistent-mro` | 0 | 0 | 3 |
| `redundant-cast` | 0 | 0 | 3 |
| `non-subscriptable` | 0 | 0 | 1 |
| `unused-ignore-comment` | 0 | 1 | 0 |
| **Total** | **164** | **15** | **5,098** |

**[Full report with detailed diff](https://dcreager-typevar-context.ecosystem-663.pages.dev/diff)**


---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:459 on 2025-07-29 07:25_

Was thinking about whether it would be nice to show `U@C.generic_method`, but pyright also just shows `U@generic_method`, and there's a risk that `U@C.generic_method` might be misinterpreted as `(U@C).generic_method`? So.. probably best to leave everything as is.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:6415 on 2025-07-29 07:47_

> If the legacy typevar is still unbound after that search

More a question than a comment: When talking about "binding" a typevar, I typically think of the process where the type variable is associated with a concrete type. Here, the same word is used for associating a typevar with a surrounding generic context — which is a process that doesn't involve solving for a concrete type. And finally, there's an unrelated third mechanism where type variables can *have* a "bound", which should not be confused with the past-tense form of "to bind".

The risk of confusion is probably rather low, I'm mainly trying to see if my understanding is correct?

---

_@sharkdp approved on 2025-07-29 07:47_

Very nice — thank you!

Did you look at the added diagnostics here? There are some new diagnostics like *"Argument is incorrect: Expected `_TSource@choose`, found `_TSource@mapper`"* or *"Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`"*, which look concerning?

---

_Comment by @AlexWaygood on 2025-07-29 10:49_

> Can you provide some more context on the motivation for:
> 
> > With that in place, we can now include the name of the binding context when rendering typevars (e.g. T@f instead of T)
> 
> I find the notation a bit confusing. E.g. I find it a bit strange to see it in completions. Is this essential information for users? What's the motivation for always showing it? Do you know what other type checkers do here? Feel free to ignore this part if it's obvious to everyone else working on ty's typing and it's just me being out of the loop

@MichaReiser this is the same notation that pyright uses (in its display to users) to disambiguate the scope to which a legacy TypeVar is bound. Mypy increments an identifier instead (``T`1`` for `T` bound to one scope, ``T`2`` for the same TypeVar `T` bound to another scope, etc.), but I personally prefer the pyright display here as it's much easier for the user to see which scope the TypeVar is being bound to.

You can see their different displays here:
- https://mypy-play.net/?mypy=latest&python=3.12&gist=4096ef3ddd1d2f0240f01dc02f289281
- https://pyright-play.net/?pythonVersion=3.13&strict=true&enableExperimentalFeatures=true&code=E4UwbiCGA2D6AuBPADiAFPArs6IB0sskAJsYQJRA

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:6415 on 2025-07-29 13:29_

Yes that's all correct, and I agree with you in not liking having all three use basically the same word. The spec uses "bound" for this new third sense ("unbound type variables should not appear in the bodies of generic functions"), but not consistently. It also uses "used in" quite a bit ("a type variable used in a method of a generic class").

Happy to bikeshed on the name for a bit, or to just add more clarifying comments.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:459 on 2025-07-29 13:30_

`U@generic_method` also aligns with [pyright's rendering](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoDiApikSEgMYA0UAKokQGoCGIAsAFC1QC8dDLEAAoARLREBKTgFVe-BE1ajpkzhQA2zAM5aoAYSHFS5CgG1aAXQkAuTlHtQAJkWBQIRGAAswjoVqLqwDQArtaYKDASUAC0AHzhMLYcDilQIB7BIChQwXYOzq5oJGSUAPruXj5%2BAUGwYbQhYdJRcVDSSakO6TCZ2bkcnOkAbkTM6qXwCkJ6AHRFxmUV3o4SQA)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6629 on 2025-07-29 13:31_

> Would it make sense to split the struct into a `TypeVarInstance` and a `BoundTypeVarInstance`

I can give that a quick try to see if that helps

---

_@dcreager reviewed on 2025-07-29 13:31_

---

_Renamed from "[ty] Track difference uses of legacy typevars, including context when rendering typevars" to "[ty] Track different uses of legacy typevars, including context when rendering typevars" by @dcreager on 2025-07-29 13:37_

---

_@sharkdp reviewed on 2025-07-29 13:44_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:6415 on 2025-07-29 13:44_

No need, I think. Thanks for the clarification.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6629 on 2025-07-30 21:45_

Okay this "try" has not really been "quick", but I'm willing to say that this won't work, at least without fairly invasive changes. The sticking point wasn't size constraints, but instead that we would need separate `Type` variants for `TypeVarInstance` and `BoundTypeVarInstance`. There genuinely are places where we need to refer to a legacy typevar that has not yet been bound — in particular, in typevar defaults:

```py
T = TypeVar("T")
U = TypeVar("U", default=T)
```

That reference to `T` is not yet bound in a generic context, and in fact needs to be rebound so that the default of `U@C` is `T@C` in the following:

```py
class C(Generic[T, U]): ...
```

All in all, I don't see this being tenable, at least in the current PR. (To make this work, we would need the separate types you suggest, and also a separate `Type` variant for each.)

Regarding the performance regression,

> Do you think that this new field leads to significantly less reuse of TypeVarInstances, meaning, it leads to many more interned salsa struct (which requires storing an extra name and the overhead of salsa tracking).

Yes, that's right, and especially since the typeshed makes heavy use of legacy typevars.  The MRO of `list[int]`, for instance, contains all of the following:

```
list[_T = int]
MutableSequence[_T = int]
Sequence[_T_co = int]
Reversible[_T_co = int]
Collection[_T_co = int]
Iterable[_T_co = int]
Container[_T_co = int]
```

Before, that was two `TypeVarInstance`s, one for `_T` and one for `_T_co`. Now its nine: the two unbound ones that are created when `_T` and `_T_co` are instantiated, and one for each class that uses the typevar in its generic context.

Given that factoring out the binding is untenable (at least in this PR), I think we have to accept the performance regression.

---

_@dcreager reviewed on 2025-07-30 21:46_

---

_@MichaReiser reviewed on 2025-07-31 05:20_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:6629 on 2025-07-31 05:20_

Thanks for looking into this. Would you mind opening an issue that documents this problem as a future performance optimisation

---

_Comment by @dcreager on 2025-08-01 13:50_

Dug into the new ecosystem diagnostics, and they have one of two causes:

> Did you look at the added diagnostics here? There are some new diagnostics like _"Argument is incorrect: Expected `_TSource@choose`, found `_TSource@mapper`"_

This is due to the `@curry_flip` decorator changing the type of the `choose` function to `(...) -> (Unknown, /) -> Unknown`, which means that we don't see its type variables when looking for enclosing generic contexts while inferring `mapper`. I'm going to dig into this one more.

> _"Argument to bound method `__init__` is incorrect: Expected `_T_io@redirect_stdout`, found `None`"_, which look concerning?

This example is now hitting https://github.com/astral-sh/ty/issues/588, since we're now trying to instantiate a class `contextlib.redirect_stdout` that inherits its `__init__` method from a superclass.  I have a fix for that ready that I haven't pushed up and PRed yet, but that will be resolved by that once I do.

---

_Comment by @dcreager on 2025-08-01 14:23_

> This is due to the `@curry_flip` decorator changing the type of the `choose` function to `(...) -> (Unknown, /) -> Unknown`, which means that we don't see its type variables when looking for enclosing generic contexts while inferring `mapper`. I'm going to dig into this one more.

And for this, it's not that decorators in general are a problem — it works fine when I use a simpler decorator like

```py
def nothing[F](f: F) -> F:
    return f
```

The issue is that the `curry_flip` decorator that's being applied uses `ParamSpec` and `Concatenate` in a way that we don't yet support.

---

_@dcreager reviewed on 2025-08-01 15:02_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6629 on 2025-08-01 15:02_

https://github.com/astral-sh/ty/issues/926

---

_@dcreager reviewed on 2025-08-01 15:22_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:6602 on 2025-08-01 15:22_

Done

---

_Merged by @dcreager on 2025-08-01 16:20_

---

_Closed by @dcreager on 2025-08-01 16:20_

---

_Branch deleted on 2025-08-01 16:20_

---
