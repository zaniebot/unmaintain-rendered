```yaml
number: 21650
title: "[ty] Support `type[T]` with type variables"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/subclass-of-typevar
created_at: 2025-11-26T23:55:04Z
updated_at: 2025-11-28T08:20:26Z
url: https://github.com/astral-sh/ruff/pull/21650
synced_at: 2026-01-12T15:57:30Z
```

# [ty] Support `type[T]` with type variables

---

_@ibraheemdev_

## Summary

Adds support for `type[T]`, where `T` is a type variable.

- Resolves https://github.com/astral-sh/ty/issues/501
- Resolves https://github.com/astral-sh/ty/issues/783
- Resolves https://github.com/astral-sh/ty/issues/662

---

_Label `ty` added by @ibraheemdev on 2025-11-26 23:55_

---

_Review requested from @carljm by @ibraheemdev on 2025-11-26 23:55_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-26 23:55_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-26 23:55_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-26 23:55_

---

_Comment by @astral-sh-bot[bot] on 2025-11-26 23:57_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-28 01:51:32.931459963 +0000
+++ new-output.txt	2025-11-28 01:51:36.261472601 +0000
@@ -108,13 +108,7 @@
 annotations_generators.py:86:21: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.GeneratorType`
 annotations_generators.py:91:27: error[invalid-return-type] Return type does not match returned value: expected `int`, found `types.AsyncGeneratorType`
 annotations_generators.py:193:1: error[type-assertion-failure] Type `() -> AsyncIterator[int]` does not match asserted type `def generator30() -> AsyncIterator[int]`
-annotations_methods.py:31:1: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
-annotations_methods.py:36:1: error[type-assertion-failure] Type `B` does not match asserted type `Unknown`
 annotations_methods.py:42:1: error[type-assertion-failure] Type `A` does not match asserted type `B`
-annotations_methods.py:48:1: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
-annotations_methods.py:49:1: error[type-assertion-failure] Type `B` does not match asserted type `Unknown`
-annotations_methods.py:50:1: error[type-assertion-failure] Type `B` does not match asserted type `Unknown`
-annotations_methods.py:51:1: error[type-assertion-failure] Type `A` does not match asserted type `Unknown`
 annotations_typeexpr.py:88:9: error[invalid-type-form] Function calls are not allowed in type expressions
 annotations_typeexpr.py:89:9: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
 annotations_typeexpr.py:90:9: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
@@ -227,6 +221,7 @@
 constructors_call_init.py:130:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
 constructors_call_metaclass.py:39:1: error[type-assertion-failure] Type `int | Meta2` does not match asserted type `Class2`
 constructors_call_metaclass.py:39:13: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
+constructors_call_metaclass.py:46:16: error[invalid-super-argument] `T@__call__` is not an instance or subclass of `<class 'Meta3'>` in `super(<class 'Meta3'>, T@__call__)` call
 constructors_call_metaclass.py:54:1: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
 constructors_call_metaclass.py:68:1: error[missing-argument] No argument provided for required parameter `x` of function `__new__`
 constructors_call_new.py:21:13: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `int`, found `float`
@@ -248,6 +243,8 @@
 constructors_call_type.py:40:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of function `__new__`
 constructors_call_type.py:50:5: error[missing-argument] No arguments provided for required parameters `x`, `y` of bound method `__init__`
 constructors_call_type.py:59:9: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
+constructors_call_type.py:81:5: error[missing-argument] No argument provided for required parameter `y` of function `__new__`
+constructors_call_type.py:82:12: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str`, found `Literal[2]`
 constructors_callable.py:36:13: info[revealed-type] Revealed type: `(...) -> Unknown`
 constructors_callable.py:37:1: error[type-assertion-failure] Type `Class1` does not match asserted type `Unknown`
 constructors_callable.py:49:13: info[revealed-type] Revealed type: `(...) -> Unknown`
@@ -335,7 +332,7 @@
 dataclasses_transform_converter.py:121:35: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `Literal["1"]`
 dataclasses_transform_converter.py:121:40: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, str]`, found `tuple[tuple[Literal["a"], Literal["1"]], tuple[Literal["b"], Literal["2"]]]`
 dataclasses_transform_converter.py:130:31: error[invalid-argument-type] Argument to function `model_field` is incorrect: Expected `(Literal[1], /) -> Unknown`, found `def converter_simple(s: str) -> int`
-dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> @Todo(unsupported type[X] special form)`
+dataclasses_transform_field.py:49:43: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(type[T@create_model], /) -> type[T@create_model]`
 dataclasses_transform_field.py:64:16: error[unknown-argument] Argument `id` does not match any known parameter
 dataclasses_transform_field.py:75:1: error[missing-argument] No argument provided for required parameter `name`
 dataclasses_transform_field.py:75:16: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
@@ -396,8 +393,7 @@
 directives_version_platform.py:36:17: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_version_platform.py:40:17: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
 directives_version_platform.py:45:17: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `int`
-enums_behaviors.py:27:1: error[type-assertion-failure] Type `Color` does not match asserted type `Unknown`
-enums_behaviors.py:28:1: error[type-assertion-failure] Type `Literal[Color.RED]` does not match asserted type `Unknown`
+enums_behaviors.py:28:1: error[type-assertion-failure] Type `Literal[Color.RED]` does not match asserted type `Color`
 enums_behaviors.py:32:1: error[type-assertion-failure] Type `Literal[Color.BLUE]` does not match asserted type `Color`
 enums_behaviors.py:44:21: error[subclass-of-final-class] Class `ExtendedShape` cannot inherit from final class `Shape`
 enums_expansion.py:52:9: error[type-assertion-failure] Type `CustomFlags` does not match asserted type `Literal[CustomFlags.FLAG3]`
@@ -515,6 +511,7 @@
 generics_self_advanced.py:36:9: error[type-assertion-failure] Type `list[Self@method2]` does not match asserted type `list[typing.Self]`
 generics_self_advanced.py:37:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `typing.Self`
 generics_self_advanced.py:38:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `Unknown`
+generics_self_advanced.py:42:9: error[type-assertion-failure] Type `type[Self@method3]` does not match asserted type `Unknown`
 generics_self_advanced.py:43:9: error[type-assertion-failure] Type `list[Self@method3]` does not match asserted type `Unknown`
 generics_self_advanced.py:44:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
 generics_self_advanced.py:45:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
@@ -522,6 +519,7 @@
 generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
+generics_self_basic.py:27:9: error[type-assertion-failure] Type `type[Self@from_config]` does not match asserted type `Unknown`
 generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
 generics_self_basic.py:54:1: error[type-assertion-failure] Type `Shape` does not match asserted type `Unknown`
 generics_self_basic.py:55:1: error[type-assertion-failure] Type `Circle` does not match asserted type `Unknown`
@@ -876,6 +874,8 @@
 qualifiers_annotated.py:59:8: error[invalid-type-form] Special form `typing.Annotated` expected at least 2 arguments (one type and at least one metadata element)
 qualifiers_annotated.py:71:24: error[invalid-assignment] Object of type `<typing.Annotated special form>` is not assignable to `type[Any]`
 qualifiers_annotated.py:72:24: error[invalid-assignment] Object of type `<typing.Annotated special form>` is not assignable to `type[Any]`
+qualifiers_annotated.py:79:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[Unknown]`, found `<typing.Annotated special form>`
+qualifiers_annotated.py:80:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[Unknown]`, found `<typing.Annotated special form>`
 qualifiers_annotated.py:86:1: error[call-non-callable] Object of type `typing.Annotated` is not callable
 qualifiers_annotated.py:87:1: error[call-non-callable] Object of type `GenericAlias` is not callable
 qualifiers_annotated.py:88:1: error[call-non-callable] Object of type `GenericAlias` is not callable
@@ -905,8 +905,8 @@
 specialtypes_none.py:27:19: error[invalid-assignment] Object of type `None` is not assignable to `Iterable[Unknown]`
 specialtypes_none.py:32:1: error[missing-argument] No argument provided for required parameter `value` of function `__eq__`
 specialtypes_promotions.py:13:5: warning[possibly-missing-attribute] Attribute `numerator` may be missing on object of type `int | float`
-specialtypes_type.py:44:1: error[type-assertion-failure] Type `TeamUser` does not match asserted type `Unknown`
 specialtypes_type.py:56:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[BasicUser] | type[ProUser]`, found `<class 'TeamUser'>`
+specialtypes_type.py:70:7: error[invalid-argument-type] Argument to function `func5` is incorrect: Expected `type[Unknown]`, found `typing.Callable`
 specialtypes_type.py:76:17: error[invalid-type-form] type[...] must have exactly one type argument
 specialtypes_type.py:84:5: error[type-assertion-failure] Type `type[Any]` does not match asserted type `type`
 specialtypes_type.py:99:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
@@ -917,7 +917,6 @@
 specialtypes_type.py:110:5: error[type-assertion-failure] Type `tuple[type, ...]` does not match asserted type `Any`
 specialtypes_type.py:117:5: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:120:5: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
-specialtypes_type.py:127:5: error[type-assertion-failure] Type `ProUser` does not match asserted type `Unknown`
 specialtypes_type.py:137:5: error[type-assertion-failure] Type `type[Any]` does not match asserted type `type`
 specialtypes_type.py:139:5: error[type-assertion-failure] Type `type[Any]` does not match asserted type `type`
 specialtypes_type.py:143:1: error[unresolved-attribute] Object of type `typing.Type` has no attribute `unknown`
@@ -927,6 +926,7 @@
 specialtypes_type.py:160:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 specialtypes_type.py:161:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 specialtypes_type.py:169:21: error[invalid-assignment] Object of type `type` is not assignable to `type[int]`
+specialtypes_type.py:175:16: error[invalid-return-type] Return type does not match returned value: expected `type[T@ClassA]`, found `type`
 tuples_type_compat.py:15:27: error[invalid-assignment] Object of type `tuple[int | float, int | float | complex]` is not assignable to `tuple[int, int]`
 tuples_type_compat.py:29:10: error[invalid-assignment] Object of type `tuple[int, ...]` is not assignable to `tuple[int, *tuple[int, ...]]`
 tuples_type_compat.py:32:10: error[invalid-assignment] Object of type `tuple[int, *tuple[int, ...]]` is not assignable to `tuple[int]`

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-26 23:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_base.py:519:16: error[invalid-return-type] Return type does not match returned value: expected `BT@__ror__`, found `BidictBase[Any, Any]`
- Found 16 diagnostics
+ Found 15 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- src/pegen/parser.py:28:19: error[unresolved-attribute] Object of type `F@logger` has no attribute `__name__`
- src/pegen/parser.py:48:19: error[unresolved-attribute] Object of type `F@memoize` has no attribute `__name__`
- Found 50 diagnostics
+ Found 48 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- chess/__init__.py:1558:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `BaseBoard`
- chess/__init__.py:1842:20: error[invalid-return-type] Return type does not match returned value: expected `Self@root`, found `Board`
- chess/pgn.py:1050:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `Headers`
- chess/variant.py:876:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `CrazyhousePocket`
- Found 16 diagnostics
+ Found 12 diagnostics

kornia (https://github.com/kornia/kornia)
+ kornia/constants.py:36:16: error[invalid-return-type] Return type does not match returned value: expected `Iterator[Enum]`, found `Iterator[object]`
+ kornia/contrib/models/efficient_vit/nn/act.py:31:51: error[invalid-assignment] Object of type `dict[str, type[Unknown] | partial[Unknown]]` is not assignable to `dict[str, type[Unknown]]`
- Found 763 diagnostics
+ Found 765 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/headers.py:88:16: error[invalid-return-type] Return type does not match returned value: expected `str | tuple[str, str] | Self@__getitem__`, found `Headers`
- src/werkzeug/datastructures/headers.py:569:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `Headers`
- src/werkzeug/datastructures/structures.py:394:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `MultiDict[K@MultiDict, V@MultiDict]`
- src/werkzeug/datastructures/structures.py:398:16: error[invalid-return-type] Return type does not match returned value: expected `Self@deepcopy`, found `MultiDict[K@MultiDict, V@MultiDict]`
- tests/test_datastructures.py:560:29: error[unresolved-attribute] Object of type `BaseException` has no attribute `get_description`
- tests/test_datastructures.py:561:9: error[unresolved-attribute] Unresolved attribute `show_exception` on type `BaseException`.
- tests/test_datastructures.py:562:25: error[unresolved-attribute] Object of type `BaseException` has no attribute `get_description`
- tests/test_datastructures.py:567:9: error[unresolved-attribute] Unresolved attribute `show_exception` on type `BaseException`.
- tests/test_datastructures.py:568:25: error[unresolved-attribute] Object of type `BaseException` has no attribute `get_description`
- tests/test_datastructures.py:570:29: error[unresolved-attribute] Object of type `BaseException` has no attribute `get_description`
- tests/test_routing.py:39:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:44:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:49:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:54:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:59:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:106:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:111:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:116:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:149:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:185:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `code`
- tests/test_routing.py:188:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:542:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:551:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:565:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:1101:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:1156:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:1164:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:1225:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `get_response`
- tests/test_routing.py:1237:9: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:1247:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:1266:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- tests/test_routing.py:1271:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `new_url`
- Found 397 diagnostics
+ Found 365 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/directives.py:298:15: error[invalid-assignment] Object of type `list[Unknown | ~str]` is not assignable to `((...) -> None) | str | list[((...) -> None) | str] | None`
+ lib/spack/spack/directives.py:298:15: error[invalid-assignment] Object of type `list[Unknown | ~str]` is not assignable to `((type[Unknown] | Unknown, /) -> None) | str | list[((type[Unknown] | Unknown, /) -> None) | str] | None`
- lib/spack/spack/directives.py:299:37: error[not-iterable] Object of type `((...) -> None) | str | list[((...) -> None) | str] | None` may not be iterable
+ lib/spack/spack/directives.py:299:37: error[not-iterable] Object of type `((type[Unknown] | Unknown, /) -> None) | str | list[((type[Unknown] | Unknown, /) -> None) | str] | None` may not be iterable
- lib/spack/spack/directives.py:323:26: error[not-iterable] Object of type `((...) -> None) | str | list[((...) -> None) | str] | None` may not be iterable
+ lib/spack/spack/directives.py:323:26: error[not-iterable] Object of type `((type[Unknown] | Unknown, /) -> None) | str | list[((type[Unknown] | Unknown, /) -> None) | str] | None` may not be iterable
+ lib/spack/spack/vendor/jinja2/sandbox.py:121:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/utils.py:157:23: error[unresolved-attribute] Object of type `F@internalcode` has no attribute `__code__`
+ lib/spack/spack/vendor/jinja2/utils.py:43:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/jinja2/utils.py:60:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/jinja2/utils.py:73:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/jinja2/utils.py:85:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/jsonschema/exceptions.py:240:26: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 7960 diagnostics
+ Found 7965 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/rich/repr.py:90:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/pip/_vendor/rich/repr.py:93:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/pip/_vendor/rich/repr.py:95:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 603 diagnostics
+ Found 600 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/utils.py:100:23: error[unresolved-attribute] Object of type `F@internalcode` has no attribute `__code__`
+ src/jinja2/sandbox.py:111:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/jinja2/utils.py:51:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/jinja2/utils.py:68:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/jinja2/utils.py:81:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/jinja2/utils.py:93:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/jinja2/utils.py:472:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `LRUCache`
- Found 178 diagnostics
+ Found 181 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/language/test_lexer.py:38:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/language/test_lexer.py:39:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `description`
- tests/language/test_lexer.py:40:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `locations`
- tests/language/test_lexer.py:552:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/language/test_lexer.py:555:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `locations`
- tests/language/test_parser.py:62:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/language/test_parser.py:63:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `description`
- tests/language/test_parser.py:64:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `locations`
- tests/language/test_parser.py:71:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/language/test_parser.py:72:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `description`
- tests/language/test_parser.py:73:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `locations`
- tests/language/test_parser.py:81:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/language/test_parser.py:82:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `positions`
- tests/language/test_parser.py:83:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `locations`
- tests/language/test_schema_parser.py:57:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/language/test_schema_parser.py:58:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `description`
- tests/language/test_schema_parser.py:59:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `locations`
- tests/type/test_definition.py:823:15: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:827:15: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:832:15: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:837:15: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:842:15: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:846:15: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:862:15: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:867:15: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:873:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:879:15: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:884:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/type/test_definition.py:891:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/utilities/test_ast_from_value.py:193:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/utilities/test_ast_from_value.py:199:13: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/utilities/test_coerce_input_value.py:462:20: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- tests/utilities/test_coerce_input_value.py:471:20: error[unresolved-attribute] Object of type `BaseException` has no attribute `message`
- Found 471 diagnostics
+ Found 438 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
- test_htmlgen/attribute.py:271:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test_htmlgen/attribute.py:279:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 24 diagnostics
+ Found 22 diagnostics

black (https://github.com/psf/black)
- src/blib2to3/pgen2/grammar.py:151:16: error[invalid-return-type] Return type does not match returned value: expected `_P@copy`, found `Grammar`
- Found 63 diagnostics
+ Found 62 diagnostics

starlette (https://github.com/encode/starlette)
- tests/test_config.py:20:5: error[type-assertion-failure] Type `str` does not match asserted type `Unknown`
- tests/test_config.py:22:5: error[type-assertion-failure] Type `str | None` does not match asserted type `None`
- tests/test_config.py:23:5: error[type-assertion-failure] Type `str` does not match asserted type `Literal[""]`
- tests/test_config.py:25:5: error[type-assertion-failure] Type `bool` does not match asserted type `Unknown`
- tests/test_config.py:26:5: error[type-assertion-failure] Type `bool` does not match asserted type `Literal[False]`
- tests/test_config.py:27:5: error[type-assertion-failure] Type `bool | None` does not match asserted type `None`
- Found 222 diagnostics
+ Found 216 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ boostedblob/path.py:284:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ boostedblob/path.py:288:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 16 diagnostics
+ Found 18 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/http/headers.py:128:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__copy__`, found `Headers`
- scrapy/item.py:128:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `Item`
- scrapy/utils/datatypes.py:70:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__copy__`, found `CaselessDict`
- scrapy/utils/misc.py:144:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_extension_throttle.py:186:5: error[invalid-assignment] Implicit shadowing of function `_adjust_delay`
+ tests/test_extension_throttle.py:188:29: error[invalid-argument-type] Argument to bound method `_response_downloaded` is incorrect: Expected `Response`, found `None`
- Found 1739 diagnostics
+ Found 1737 diagnostics

alerta (https://github.com/alerta/alerta)
- alerta/database/base.py:470:9: error[unresolved-attribute] Unresolved attribute `alerts` on type `type[QueryBuilder]`.
+ alerta/database/base.py:470:9: error[unresolved-attribute] Unresolved attribute `alerts` on type `type[Self@init_app]`.
- alerta/database/base.py:471:9: error[unresolved-attribute] Unresolved attribute `blackouts` on type `type[QueryBuilder]`.
+ alerta/database/base.py:471:9: error[unresolved-attribute] Unresolved attribute `blackouts` on type `type[Self@init_app]`.
- alerta/database/base.py:472:9: error[unresolved-attribute] Unresolved attribute `heartbeats` on type `type[QueryBuilder]`.
+ alerta/database/base.py:472:9: error[unresolved-attribute] Unresolved attribute `heartbeats` on type `type[Self@init_app]`.
- alerta/database/base.py:473:9: error[unresolved-attribute] Unresolved attribute `keys` on type `type[QueryBuilder]`.
+ alerta/database/base.py:473:9: error[unresolved-attribute] Unresolved attribute `keys` on type `type[Self@init_app]`.
- alerta/database/base.py:474:9: error[unresolved-attribute] Unresolved attribute `users` on type `type[QueryBuilder]`.
+ alerta/database/base.py:474:9: error[unresolved-attribute] Unresolved attribute `users` on type `type[Self@init_app]`.
- alerta/database/base.py:475:9: error[unresolved-attribute] Unresolved attribute `groups` on type `type[QueryBuilder]`.
+ alerta/database/base.py:475:9: error[unresolved-attribute] Unresolved attribute `groups` on type `type[Self@init_app]`.
- alerta/database/base.py:476:9: error[unresolved-attribute] Unresolved attribute `perms` on type `type[QueryBuilder]`.
+ alerta/database/base.py:476:9: error[unresolved-attribute] Unresolved attribute `perms` on type `type[Self@init_app]`.
- alerta/database/base.py:477:9: error[unresolved-attribute] Unresolved attribute `customers` on type `type[QueryBuilder]`.
+ alerta/database/base.py:477:9: error[unresolved-attribute] Unresolved attribute `customers` on type `type[Self@init_app]`.

beartype (https://github.com/beartype/beartype)
- beartype/_check/code/codemain.py:401:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_decor/_nontype/_decordescriptor.py:153:9: error[invalid-argument-type] Argument to function `beartype_func` is incorrect: Argument type `((Any, /) -> Any) | None` does not satisfy upper bound `type | ((...) -> Any) | classmethod[Unknown, Unknown, Unknown] | property | staticmethod[Unknown, Unknown]` of type variable `BeartypeableT`
+ beartype/_util/api/standard/utilfunctools.py:308:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_util/cache/utilcachecall.py:180:34: error[unresolved-attribute] Object of type `CallableT@callable_cached & (() -> object)` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:417:34: error[unresolved-attribute] Object of type `CallableT@method_cached_arg_by_id & (() -> object)` has no attribute `__name__`
- beartype/_util/cache/utilcachecall.py:568:44: error[unresolved-attribute] Object of type `CallableT@property_cached & (() -> object)` has no attribute `__name__`
- beartype/_util/cache/utilcachemeta.py:79:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/typing/_typingpep544.py:217:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/typing/_typingpep544.py:237:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 507 diagnostics
+ Found 500 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:1242:29: error[unresolved-attribute] Object of type `FixtureFunction@__call__ & ~type & ~FixtureFunctionDefinition` has no attribute `__name__`
- src/_pytest/nodes.py:110:48: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/_pytest/nodes.py:125:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/_pytest/raises.py:446:50: error[invalid-assignment] Object of type `type` is not assignable to `type[BaseException] | None`
+ testing/python/metafunc.py:57:9: error[invalid-assignment] Object of type `FuncFixtureInfoMock` is not assignable to attribute `_fixtureinfo` of type `FuncFixtureInfo`
+ testing/python/metafunc.py:58:9: error[invalid-assignment] Object of type `SessionMock` is not assignable to attribute `session` of type `Session`
- testing/python/raises.py:38:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/python/raises.py:51:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/python/raises.py:182:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/python/raises.py:350:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/python/raises.py:372:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/python/raises.py:384:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/python/raises.py:391:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/python/raises_group.py:41:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/python/raises_group.py:46:25: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/python/raises_group.py:1082:28: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/test_assertrewrite.py:2265:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `errno`
- testing/test_mark_expression.py:140:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `offset`
- testing/test_mark_expression.py:141:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_pytester.py:502:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_pytester.py:503:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_pytester.py:515:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_pytester.py:516:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_skipping.py:119:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_skipping.py:122:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_skipping.py:141:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_skipping.py:142:70: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_skipping.py:143:29: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/test_skipping.py:895:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `msg`
- testing/typing_checks.py:54:5: error[type-assertion-failure] Type `ExceptionInfo[RuntimeError] | None` does not match asserted type `ExceptionInfo[BaseException] | None`
- testing/typing_raises_group.py:83:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((BaseException, /) -> bool) | None`, found `def check_filenotfound(exc: FileNotFoundError) -> bool`
- testing/typing_raises_group.py:86:49: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((BaseException, /) -> bool) | None`, found `def check_filenotfound(exc: FileNotFoundError) -> bool`
- testing/typing_raises_group.py:207:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- testing/typing_raises_group.py:218:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 457 diagnostics
+ Found 429 diagnostics

rich (https://github.com/Textualize/rich)
- rich/repr.py:90:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rich/repr.py:93:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- rich/repr.py:95:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 350 diagnostics
+ Found 347 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/tests/utils.py:102:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 185 diagnostics
+ Found 184 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- tests/test_runserver_serve.py:42:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_runserver_serve.py:63:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_runserver_serve.py:82:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_runserver_serve.py:95:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_runserver_serve.py:106:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_runserver_watch.py:95:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 36 diagnostics
+ Found 30 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ tests/BadAttributes.py:69:13: warning[possibly-missing-attribute] Attribute `args` may be missing on object of type `Exception | None`
+ tests/Exceptions.py:164:26: warning[possibly-missing-attribute] Attribute `get` may be missing on object of type `dict[str, str] | None`
+ tests/GithubRetry.py:400:71: warning[possibly-missing-attribute] Attribute `args` may be missing on object of type `BaseException | None`
+ tests/GithubRetry.py:402:35: warning[possibly-missing-attribute] Attribute `__cause__` may be missing on object of type `BaseException | None`
+ tests/GithubRetry.py:405:17: warning[possibly-missing-attribute] Attribute `__cause__` may be missing on object of type `BaseException | None`
+ tests/GithubRetry.py:405:17: warning[possibly-missing-attribute] Attribute `args` may be missing on object of type `BaseException | None`
+ tests/GraphQl.py:113:32: error[invalid-argument-type] Argument to bound method `assertRaises` is incorrect: Expected `type[Unknown] | tuple[type[Unknown], ...]`, found `<module 'github.GithubException'>`
+ tests/GraphQl.py:149:32: error[invalid-argument-type] Argument to bound method `assertRaises` is incorrect: Expected `type[Unknown] | tuple[type[Unknown], ...]`, found `<module 'github.GithubException'>`
- Found 288 diagnostics
+ Found 296 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/core/registries.py:18:21: error[unresolved-attribute] Object of type `T@Registry` has no attribute `__name__`
+ src/schemathesis/schemas.py:679:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

poetry (https://github.com/python-poetry/poetry)
- tests/installation/test_chooser.py:358:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `get_text`
- tests/utils/test_authenticator.py:351:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `response`
- tests/utils/test_authenticator.py:352:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `response`
- tests/utils/test_authenticator.py:353:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `response`
- tests/utils/test_isolated_build.py:116:16: error[unresolved-attribute] Object of type `BaseException` has no attribute `requirements`
- tests/vcs/git/test_backend.py:283:12: error[unresolved-attribute] Object of type `BaseException` has no attribute `get_text`
- Found 970 diagnostics
+ Found 964 diagnostics

nox (https://github.com/wntrblm/nox)
- nox/_decorators.py:53:9: error[unresolved-attribute] Object of type `T@_copy_func` has no attribute `__code__`
- nox/_decorators.py:54:9: error[unresolved-attribute] Object of type `T@_copy_func` has no attribute `__globals__`
- nox/_decorators.py:55:22: error[unresolved-attribute] Object of type `T@_copy_func` has no attribute `__name__`
- nox/_decorators.py:56:17: error[unresolved-attribute] Object of type `T@_copy_func` has no attribute `__defaults__`
- nox/_decorators.py:57:17: error[unresolved-attribute] Object of type `T@_copy_func` has no attribute `__closure__`
- nox/_decorators.py:61:26: error[unresolved-attribute] Object of type `T@_copy_func` has no attribute `__kwdefaults__`
- Found 29 diagnostics
+ Found 23 diagnostics

artigraph (https://github.com/artigraph/artigraph)
- src/arti/internal/utils.py:97:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__add__`, found `_int`
- src/arti/internal/utils.py:100:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__and__`, found `_int`
- src/arti/internal/utils.py:103:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__ceil__`, found `_int`
- src/arti/internal/utils.py:106:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__floor__`, found `_int`
- src/arti/internal/utils.py:109:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__floordiv__`, found `_int`
- src/arti/internal/utils.py:112:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__invert__`, found `_int`
- src/arti/internal/utils.py:115:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__lshift__`, found `_int`
- src/arti/internal/utils.py:118:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__mod__`, found `_int`
- src/arti/internal/utils.py:121:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__mul__`, found `_int`
- src/arti/internal/utils.py:124:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__neg__`, found `_int`
- src/arti/internal/utils.py:127:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__or__`, found `_int`
- src/arti/internal/utils.py:130:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__pos__`, found `_int`
- src/arti/internal/utils.py:133:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__radd__`, found `_int`
- src/arti/internal/utils.py:136:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__rand__`, found `_int`
- src/arti/internal/utils.py:139:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__rfloordiv__`, found `_int`
- src/arti/internal/utils.py:142:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__rlshift__`, found `_int`
- src/arti/internal/utils.py:145:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__rmod__`, found `_int`
- src/arti/internal/utils.py:148:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__rmul__`, found `_int`
- src/arti/internal/utils.py:151:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__ror__`, found `_int`
- src/arti/internal/utils.py:154:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__round__`, found `_int`
- src/arti/internal/utils.py:157:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__rrshift__`, found `_int`
- src/arti/internal/utils.py:160:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__rshift__`, found `_int`
- src/arti/internal/utils.py:163:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__rsub__`, found `_int`
- src/arti/internal/utils.py:166:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__rxor__`, found `_int`
- src/arti/internal/utils.py:169:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__sub__`, found `_int`
- src/arti/internal/utils.py:172:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__trunc__`, found `_int`
- src/arti/internal/utils.py:175:16: error[invalid-return-type] Return type does not match returned value: expected `Self@__xor__`, found `_int`
+ src/arti/storage/__init__.py:221:13: error[invalid-argument-type] Argument is incorrect: Expected `tuple[str, ...]`, found `str | Any`
- Found 173 diagnostics
+ Found 147 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/proxy/layers/http/test_http.py:44:40: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:44:40: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:46:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:46:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:48:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:48:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:50:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:50:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:52:13: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:52:13: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:54:41: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:54:41: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:56:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:56:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:57:34: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:57:34: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
+ test/mitmproxy/proxy/layers/http/test_http.py:63:12: error[call-non-callable] Object of type `Server` is not callable
- test/mitmproxy/proxy/layers/http/test_http.py:88:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:88:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:100:40: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:100:40: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:102:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:102:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:106:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:106:36: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:110:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:110:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:112:13: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:112:13: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:114:41: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:114:41: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:116:34: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:116:34: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:161:31: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:161:31: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:163:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:163:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:165:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:165:19: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:167:9: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:167:9: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
+ test/mitmproxy/proxy/layers/http/test_http.py:175:16: error[call-non-callable] Object of type `Server` is not callable
+ test/mitmproxy/proxy/layers/http/test_http.py:177:16: error[call-non-callable] Object of type `Server` is not callable
- test/mitmproxy/proxy/layers/http/test_http.py:200:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:200:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:202:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:202:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:203:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:203:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:214:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:214:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:216:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:216:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:217:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:217:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
+ test/mitmproxy/proxy/layers/http/test_http.py:220:12: error[call-non-callable] Object of type `Server` is not callable
+ test/mitmproxy/proxy/layers/http/test_http.py:221:12: error[call-non-callable] Object of type `Server` is not callable
- test/mitmproxy/proxy/layers/http/test_http.py:289:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:289:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:291:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:291:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:292:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:292:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:293:29: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:293:29: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:294:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:294:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:314:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:314:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:316:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:316:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:318:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
+ test/mitmproxy/proxy/layers/http/test_http.py:318:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:324:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:324:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:325:29: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:325:29: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:326:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:326:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:328:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:328:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:330:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:330:21: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:331:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:331:25: error[invalid-argument-type] Argument is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
+ test/mitmproxy/proxy/layers/http/test_http.py:334:12: error[call-non-callable] Object of type `Server` is not callable
+ test/mitmproxy/proxy/layers/http/test_http.py:334:25: error[call-non-callable] Object of type `Server` is not callable
+ test/mitmproxy/proxy/layers/http/test_http.py:335:12: error[call-non-callable] Object of type `HTTPFlow` is not callable
+ test/mitmproxy/proxy/layers/http/test_http.py:335:34: error[call-non-callable] Object of type `Server` is not callable
+ test/mitmproxy/proxy/layers/http/test_http.py:336:16: error[call-non-callable] Object of type `HTTPFlow` is not callable
- test/mitmproxy/proxy/layers/http/test_http.py:362:40: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:362:40: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `HTTPFlow | _Placeholder[HTTPFlow]`
- test/mitmproxy/proxy/layers/http/test_http.py:364:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:364:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Server | _Placeholder[Server]`
- test/mitmproxy/proxy/layers/http/test_http.py:367:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:367:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expec

... (truncated 3113 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~159MB
+ TOTAL MEMORY USAGE: ~167MB
-     struct fields = ~11MB
+     struct fields = ~12MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct metadata = ~20MB
+     struct metadata = ~21MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~40MB
+     struct metadata = ~42MB


```

</details>




---

_@ibraheemdev reviewed on 2025-11-27 00:01_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/generics.rs`:1571 on 2025-11-27 00:01_

This is pretty trivial so I don't think it will overlap much with the constraint solver work, but still flagging it (cc @dcreager).

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-11-27 00:03_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-27 00:04_

---

_@ibraheemdev reviewed on 2025-11-27 00:12_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:64 on 2025-11-27 00:12_

This doesn't currently work because we can only treat `type[T]` as a class constructor if `T` has an upper bound of a single concrete class. If there are multiple constraints, we fall back to the call binding logic, which splits `type[T]` into the union of its constraints.

I think we have a general problem of not unifying operations on constrained type variables very well (e.g. [this failing test](https://github.com/astral-sh/ruff/blob/40886963d1c01bd00f27f2b67913504a52d2d85f/crates/ty_python_semantic/resources/mdtest/generics/pep695/functions.md?plain=1#L250)), so I'm not sure this issue can be addressed by this PR.

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 00:13_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 41 | 36 | 757 |
| `unresolved-attribute` | 39 | 293 | 30 |
| `invalid-return-type` | 17 | 256 | 2 |
| `call-non-callable` | 209 | 0 | 0 |
| `unused-ignore-comment` | 42 | 114 | 0 |
| `possibly-missing-attribute` | 71 | 8 | 33 |
| `type-assertion-failure` | 3 | 43 | 65 |
| `invalid-type-form` | 19 | 0 | 69 |
| `invalid-parameter-default` | 33 | 0 | 1 |
| `invalid-assignment` | 16 | 5 | 4 |
| `not-iterable` | 9 | 0 | 3 |
| `non-subscriptable` | 6 | 1 | 0 |
| `unsupported-operator` | 2 | 0 | 5 |
| `redundant-cast` | 5 | 1 | 0 |
| `no-matching-overload` | 4 | 0 | 0 |
| `unsupported-base` | 3 | 0 | 0 |
| `invalid-super-argument` | 1 | 1 | 0 |
| `invalid-generic-class` | 1 | 0 | 0 |
| `missing-argument` | 1 | 0 | 0 |
| **Total** | **522** | **758** | **969** |

**[Full report with detailed diff](https://ibraheem-subclass-of-typevar.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-subclass-of-typevar.ecosystem-663.pages.dev/timing))




---

_Comment by @AlexWaygood on 2025-11-27 13:30_

From the ecosystem hits, it looks like typevars with upper bounds are working great but that there might be some issues with value-constrained type variables on this branch:

```py
def f[T: (int, str), U: int](x: T, y: U) -> tuple[T, U]:
    reveal_type(type(y))  # type[U@f] (good)
    reveal_type(type(y)())  # U@f (good)

    reveal_type(type(x))  # type[T@f] (good)
    reveal_type(type(x)())  # int | Literal[""] (bad -- should be `T@f`)

    return type(x)(), type(y)()  # error[invalid-return-type]: Return type does not match returned value: expected `tuple[T@f, U@f]`, found `tuple[int | Literal[""], U@f]
```

Edit: Ah, I see you already have failing tests for this in https://github.com/astral-sh/ruff/pull/21650/files#r2566765667

---

_Comment by @AlexWaygood on 2025-11-27 13:41_

This ecosystem diagnostic looks odd, because I would expect both `type & Unknown` and `type[_T@AppKey]` to have a `__qualname__` attribute, since all instances of `type` have a `__qualname__` attribute, and all `type[]` types are subtypes of `type`

---

### aiohttp/helpers.py

[warning] possibly-missing-attribute - [:879:26](https://github.com/aio-libs/aiohttp/blob/72fadb8f1a56e8bff0fef23ccf02e2067cd87e41/aiohttp/helpers.py#L879) - Attribute `__qualname__` may be missing on object of type `(Unknown & type) | type[_T@AppKey]`

---

It looks like a minimal repro is:

```py
def f[T](x: type[T]) -> T:
    reveal_type(x.__qualname__)  # error[unresolved-attribute]: Object of type `type[T@f]` has no attribute `__qualname__`
    return x()
```


---

_Comment by @AlexWaygood on 2025-11-27 14:13_

Another false positive:

```py
from ty_extensions import Intersection, Unknown

def g[T: int](x: type | type[T]) -> T:
    return x()  # fine

def f[T: int](x: Intersection[type, Unknown] | type[T]) -> T:
    # error[invalid-return-type]: Return type does not match returned value: expected `T@f`, found `@Todo(Type::Intersection.call()) | int`
    return x()
```

I would expect the inferred type of `x()` to be `@Todo | T`, which would be assignable to `T`, rather than `@Todo | int` (which is not)

---

_Comment by @AlexWaygood on 2025-11-27 14:30_

Here's a minimal repro of one of the `cwltool` diagnostics:

```py
from typing import Callable, Any

def needs_a_callable(x: Callable[..., Any]): ...

def f[T](x: T) -> T:
    needs_a_callable(type(x))  # error: [invalid-argument-type]  "Argument to function `needs_a_callable` is incorrect: Expected `(...) -> Any`, found `type[T@f]`"
    return x
```

but that's a false positive: `type[T]` should be assignable to `Callable[..., T]`, and therefore also to `Callable[..., Any]`

---

_Comment by @AlexWaygood on 2025-11-27 14:34_

Here's a minimal repro of some of the ddtrace diagnostics:

```py
class Foo:
    def method(self):
        super(self.__class__, self)  # error[invalid-super-argument] "`type[Self@method]` is not a valid class"
```

You'll need to edit some code in https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types/bound_super.rs to get rid of that false positive

---

_Comment by @AlexWaygood on 2025-11-27 14:36_

Here's a repro of one of the discord.py diagnostics:

```py
from typing import Any

class Foo[T]: ...

def f[T: Foo[Any]](x: type[T] = Foo): ...
```

that one looks like it could possibly be tricky to fix? Not sure. Could be fine to just add a failing test for it with a TODO.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2113 on 2025-11-27 15:27_

I suspect the reason why you're running into issues with `type[]` types not being assignable to `Callable` types is that these branches are above the `Type::Callable` branches (but naively moving them below the `Type::Callable` branches causes other issues 🙃)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subclass_of.rs`:98 on 2025-11-27 15:31_

eh, I think we can just delete this if we're not using it -- it's pretty trivial :P

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subclass_of.rs`:303 on 2025-11-27 15:32_

I can never remember how this trait works, but `type[]` types are covariant

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subclass_of.rs`:328 on 2025-11-27 15:34_

An alternative design might be to just have it be its own `Type` variant (`Type::MetaTypeVar`?). That might save you from a few of your `.unwrap()` calls in `Type::has_relation_to_impl`, for example -- there are quite a few places where you need to treat this `SubclassOfInner` variant differently to all the other ones.

I don't have a strong opinion, though. That design would probably bring its own complications.

---

_@AlexWaygood reviewed on 2025-11-27 15:35_

really great work overall -- thank you!

---

_@ibraheemdev reviewed on 2025-11-27 18:21_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/subclass_of.rs`:98 on 2025-11-27 18:21_

This is currently used in property tests. I can change it to `cfg(test)`, but seems fine either way.

---

_@ibraheemdev reviewed on 2025-11-27 18:23_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/subclass_of.rs`:303 on 2025-11-27 18:23_

`type[]` doesn't have a type variable that we can get the variance of, so I think this is correct. `variance_of` is asking for the variance of the type variable on the generic context, not the assigned type.

---

_@AlexWaygood reviewed on 2025-11-27 18:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subclass_of.rs`:98 on 2025-11-27 18:26_

ah okay -- I'd be inclined to switch it to `#[cfg(test)]` as then readers of the code won't be confused about why it's still there (like I was, when reading the PR diff 😅)

---

_@AlexWaygood reviewed on 2025-11-27 18:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2114 on 2025-11-27 18:31_

I think there's a similar issue due to the `Type::ProtocolInstance()` branch appearing below this one -- there are two false positives on this snippet on your branch:

```py
from typing import Protocol

class Callback(Protocol):
    def __call__(self, *args, **kwargs) -> int: ...

class HasName(Protocol):
    __name__: str

def needs_callback(c: Callback): ...
def needs_something_named(obj: HasName): ...

def f[T: int](x: T) -> T:
    needs_callback(type(x))
    needs_callback(type(x))
    return x
```

---

_Comment by @ibraheemdev on 2025-11-27 18:32_

Thank you for the ecosystem triage @AlexWaygood! I think I addressed all of the issues you reproduced, except this one.
```py
class Foo[T]: ...
def f[T: Foo[Any]](x: type[T] = Foo): ...
```

We emit a similar error without the `type[T]`, because we treat the `T` as a non-inferable type variable. We may have to lazily check the assignability of default types after specialization, but either way I think the issue is unrelated to this PR.
```py
def f[T: int](x: T = 1): ...
```

---

_@AlexWaygood reviewed on 2025-11-27 18:34_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:95 on 2025-11-27 18:34_

nit: all our other tests like this are in https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/class/super.md, so I'd be inclined to put this one there too

---

_@ibraheemdev reviewed on 2025-11-27 18:35_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:2114 on 2025-11-27 18:35_

Hmm.. right. Unfortunately I think we just have to special case these, or alternatively bring the `Type::Callable` or `Type::Protocol` branches up (at the risk of making the match-arm ordering even more load-bearing :upside_down_face:).

Moving the `type[T]` downcasting branch down breaks things because we want to downcast *before* union expansion, so that `type[T]` is a subtype of `type[T] | None` (the ordering requirements are similar to a bare `T`).

---

_@AlexWaygood reviewed on 2025-11-27 18:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2114 on 2025-11-27 18:38_

Ideally I do indeed think that the structural-type branches would be as high as possible, to avoid issues like this! IIRC, though, weird things start happening if you move them above the set-theoretic and type-variable branches 🙃 So you may well be right that special-casing is the best we can do here.

---

_Comment by @AlexWaygood on 2025-11-27 18:42_

> We emit a similar error without the `type[T]`, because we treat the `T` as a non-inferable type variable. We may have to lazily check the assignability of default types after specialization, but either way I think the issue is unrelated to this PR.

ah, that makes sense -- thanks! It did some to come up a _fair_ bit in the ecosystem, so it might be worth adding some failing tests with TODOs?

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-11-27 19:26_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-27 19:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:114 on 2025-11-27 19:28_

`Callback` isn't a generic type here, so the tests you've added are passing for the wrong reasons -- you've accidentally created lots of `@Todo` types. (We _should_ emit an error on a non-generic class being subscripted.)

```suggestion
    static_assert(is_assignable_to(type[T], Callback[))
    static_assert(not is_disjoint_from(type[T], Callback))
```

If you wanted to make `Callback` generic, you'd have to write it as

```py
T_co = TypeVar("T_co", covariant=True)

class Callback(Protocol[T_co]):
    def __call__(self, *args, **kwargs) -> T_co: ...
```

---

_@AlexWaygood reviewed on 2025-11-27 19:29_

---

_Comment by @ibraheemdev on 2025-11-27 19:38_

I did a run through of the typing conformance suite changes. Most of the changes seem correct. There are a couple assertions `assert_type(cls, type[Self])` that are now failing, because we now recognize `type[Self]` but still do not recognize `cls` -- this is tracked in https://github.com/astral-sh/ty/issues/159.

There is another failure [here](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/tests/specialtypes_type.py#L175), because we consider `type` to be equivalent to `type[object]` and not `type[Any]`, so I believe that one is an intentional divergence from the spec.

Finally, there is one new false positive that seems incorrect,
```py
from typing import TypeVar

T = TypeVar("T")

class Meta3(type):
    def __call__(cls: type[T], *args, **kwargs) -> T:
        # error[invalid-super-argument]: `T@__call__` is not an instance or subclass of `<class 'Meta3'>` in `super(<class 'Meta3'>, T@__call__)` call
        return super().__call__(cls, *args, **kwargs)
```

but I'm not sure if that's an existing issue or not.

---

_Comment by @AlexWaygood on 2025-11-27 20:08_

> Finally, there is one new false positive that seems incorrect,
> 
> ```python
> from typing import TypeVar
> 
> T = TypeVar("T")
> 
> class Meta3(type):
>     def __call__(cls: type[T], *args, **kwargs) -> T:
>         # error[invalid-super-argument]: `T@__call__` is not an instance or subclass of `<class 'Meta3'>` in `super(<class 'Meta3'>, T@__call__)` call
>         return super().__call__(cls, *args, **kwargs)
> ```
> 
> but I'm not sure if that's an existing issue or not.

you could actually argue that's a true positive. If you change the typevar so that it has an upper bound, the error goes away:

```py
from typing import TypeVar

T = TypeVar("T", bound="Meta3")

class Meta3(type):
    def __call__(cls: type[T], *args, **kwargs) -> T:
        return super().__call__(cls, *args, **kwargs)
```

---

_@ibraheemdev reviewed on 2025-11-27 20:17_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:114 on 2025-11-27 20:17_

Good catch, thanks. I assume it's a known issue that we don't error on the subscript there?

---

_@AlexWaygood reviewed on 2025-11-27 20:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_of/generics.md`:114 on 2025-11-27 20:20_

yeah. It might actually not be too hard to implement it now, we might have just not got round to it

---

_Comment by @AlexWaygood on 2025-11-27 20:20_

There's an ecosystem false positive on https://github.com/DataDog/dd-trace-py/blob/84e0f46f4da9a26aa0f741712cf2f9303794ea78/ddtrace/internal/_exceptions.py#L22, a minimal repro of which is:

```py
from typing import TypeVar

E = TypeVar("E", bound=BaseException)


def find_exception( exc: BaseException, exception_type: type[E]) -> E | None:
    if isinstance(exc, exception_type):
        return exc  # error [invalid-return-type] "Return type does not match returned value: expected `E@find_exception | None`, found `BaseException`""
    raise NotImplementedError("no")
```

you can fix the false positive with this patch

```diff
diff --git a/crates/ty_python_semantic/src/types/narrow.rs b/crates/ty_python_semantic/src/types/narrow.rs
index 5ff8b615af..0259e94877 100644
--- a/crates/ty_python_semantic/src/types/narrow.rs
+++ b/crates/ty_python_semantic/src/types/narrow.rs
@@ -171,8 +171,11 @@ impl ClassInfoConstraintFunction {
                 // e.g. `isinstance(x, list[int])` fails at runtime.
                 SubclassOfInner::Class(ClassType::Generic(_)) => None,
                 SubclassOfInner::Dynamic(dynamic) => Some(Type::Dynamic(dynamic)),
-                SubclassOfInner::TypeVar(_) => None,
-            },
+                SubclassOfInner::TypeVar(var) => match self {
+                    ClassInfoConstraintFunction::IsInstance => Some(Type::TypeVar(var)),
+                    ClassInfoConstraintFunction::IsSubclass => Some(classinfo),
+                }
+            }
             Type::Dynamic(_) => Some(classinfo),
             Type::Intersection(intersection) => {
                 if intersection.negative(db).is_empty() {
```

---

_@AlexWaygood reviewed on 2025-11-27 20:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:191 on 2025-11-27 20:24_

this change doesn't look correct -- I think you should keep this an exhaustive match. If a variable is of type `type[U]`, where `U` has an upper bound of `int`, `all_members` should return the same set of members as it would for `type[int]`

---

_@ibraheemdev reviewed on 2025-11-27 20:26_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/ide_support.rs`:191 on 2025-11-27 20:26_

`into_class` does that conversion internally. The only case this could return `None` is for `type[T]` where `T` has multiple constraints, so there is no single class to return.

EDIT: It actually returns `object` in that case, so this should never return `None`.

---

_@AlexWaygood reviewed on 2025-11-27 20:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/ide_support.rs`:191 on 2025-11-27 20:31_

Ah I see! Yes, we do want to fallback to `type` there, I think. You could also add a couple of tests to `all_members.md` to prove I'm wrong ;)

---

_Comment by @AlexWaygood on 2025-11-27 20:34_

yessss 
<img width="1598" height="750" alt="image" src="https://github.com/user-attachments/assets/29571b9d-c74f-42a7-9f1e-b1d8ee201c8e" />


---

_Comment by @AlexWaygood on 2025-11-27 20:48_

This PR fixes https://github.com/astral-sh/ty/issues/662, so it could be worth adding the MRE in that issue as a standalone test

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-11-28 01:52_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-28 01:52_

---

_@AlexWaygood approved on 2025-11-28 08:15_

Let's go!

---

_Merged by @sharkdp on 2025-11-28 08:20_

---

_Closed by @sharkdp on 2025-11-28 08:20_

---

_Branch deleted on 2025-11-28 08:20_

---
