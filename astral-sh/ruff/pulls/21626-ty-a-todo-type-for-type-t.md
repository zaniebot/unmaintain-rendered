```yaml
number: 21626
title: "[ty] A todo type for type[T]"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: david/type-t-todo
created_at: 2025-11-25T10:28:59Z
updated_at: 2025-11-25T10:59:44Z
url: https://github.com/astral-sh/ruff/pull/21626
synced_at: 2026-01-10T16:48:02Z
```

# [ty] A todo type for type[T]

---

_Pull request opened by @sharkdp on 2025-11-25 10:28_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-11-25 10:29_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-25 10:29_

---

_Comment by @astral-sh-bot[bot] on 2025-11-25 10:31_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-25 10:30:51.353217013 +0000
+++ new-output.txt	2025-11-25 10:30:55.030226915 +0000
@@ -380,7 +380,6 @@
 enums_member_names.py:30:5: error[type-assertion-failure] Type `Literal["RED", "BLUE", "GREEN"]` does not match asserted type `Any`
 enums_member_values.py:30:5: error[type-assertion-failure] Type `Literal[1, 2, 3]` does not match asserted type `Any`
 enums_member_values.py:54:1: error[type-assertion-failure] Type `Literal[1]` does not match asserted type `tuple[Literal[1], float, float]`
-enums_member_values.py:85:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `_value_` of type `str`
 enums_member_values.py:96:1: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 enums_members.py:82:1: error[type-assertion-failure] Type `Unknown` does not match asserted type `Unknown | ((x) -> Unknown)`
 enums_members.py:82:37: error[invalid-type-form] Type arguments for `Literal` must be `None`, a literal value (int, bool, str, or bytes), or an enum member
@@ -405,7 +404,6 @@
 generics_base_class.py:45:5: error[type-assertion-failure] Type `Iterator[int]` does not match asserted type `Unknown`
 generics_base_class.py:49:22: error[too-many-positional-arguments] Too many positional arguments to class `LinkedList`: expected 1, got 2
 generics_base_class.py:61:18: error[too-many-positional-arguments] Too many positional arguments to class `MyDict`: expected 1, got 2
-generics_basic.py:34:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr@concat` and `AnyStr@concat`
 generics_basic.py:49:44: error[invalid-legacy-type-variable] A `TypeVar` cannot have exactly one constraint
 generics_basic.py:139:5: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
 generics_basic.py:140:5: error[type-assertion-failure] Type `int` does not match asserted type `Unknown`
@@ -490,9 +488,9 @@
 generics_scoping.py:75:11: error[invalid-generic-class] Generic class `Bad` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
 generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@prop1`
 generics_self_advanced.py:28:25: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@method1`
-generics_self_advanced.py:36:9: error[type-assertion-failure] Type `list[Self@method2]` does not match asserted type `list[typing.Self]`
-generics_self_advanced.py:37:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `typing.Self`
-generics_self_advanced.py:38:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `Unknown`
+generics_self_advanced.py:36:9: error[type-assertion-failure] Type `list[Self@method2]` does not match asserted type `@Todo(type[T] for a type variable T)`
+generics_self_advanced.py:37:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `@Todo(type[T] for a type variable T)`
+generics_self_advanced.py:38:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `@Todo(type[T] for a type variable T)`
 generics_self_advanced.py:43:9: error[type-assertion-failure] Type `list[Self@method3]` does not match asserted type `Unknown`
 generics_self_advanced.py:44:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
 generics_self_advanced.py:45:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
@@ -525,7 +523,6 @@
 generics_syntax_compatibility.py:26:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `M@method2 | K@method2`
 generics_syntax_declarations.py:17:1: error[invalid-generic-class] Cannot both inherit from `typing.Generic` and use PEP 695 type variables
 generics_syntax_declarations.py:25:20: error[invalid-generic-class] Cannot both inherit from subscripted `Protocol` and use PEP 695 type variables
-generics_syntax_declarations.py:32:9: error[unresolved-attribute] Object of type `T@ClassD` has no attribute `is_integer`
 generics_syntax_declarations.py:48:17: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, int]`?
 generics_syntax_declarations.py:60:17: error[invalid-type-variable-constraints] TypeVar must have at least two constrained types
 generics_syntax_declarations.py:64:17: error[invalid-type-variable-constraints] TypeVar must have at least two constrained types
@@ -545,7 +542,6 @@
 generics_syntax_infer_variance.py:56:35: error[invalid-assignment] Object of type `ShouldBeCovariant3[int | float]` is not assignable to `ShouldBeCovariant3[int]`
 generics_syntax_infer_variance.py:84:36: error[invalid-assignment] Object of type `ShouldBeCovariant5[int]` is not assignable to `ShouldBeCovariant5[int | float]`
 generics_syntax_infer_variance.py:85:34: error[invalid-assignment] Object of type `ShouldBeCovariant5[int | float]` is not assignable to `ShouldBeCovariant5[int]`
-generics_syntax_infer_variance.py:92:9: error[invalid-assignment] Cannot assign to final attribute `x` on type `Self@__init__`
 generics_syntax_infer_variance.py:95:36: error[invalid-assignment] Object of type `ShouldBeCovariant6[int]` is not assignable to `ShouldBeCovariant6[int | float]`
 generics_syntax_infer_variance.py:96:34: error[invalid-assignment] Object of type `ShouldBeCovariant6[int | float]` is not assignable to `ShouldBeCovariant6[int]`
 generics_syntax_infer_variance.py:112:38: error[invalid-assignment] Object of type `ShouldBeInvariant1[int]` is not assignable to `ShouldBeInvariant1[int | float]`
@@ -809,7 +805,6 @@
 protocols_definition.py:287:22: error[invalid-assignment] Object of type `Concrete5_Bad3` is not assignable to `Template5`
 protocols_definition.py:288:22: error[invalid-assignment] Object of type `Concrete5_Bad4` is not assignable to `Template5`
 protocols_definition.py:289:22: error[invalid-assignment] Object of type `Concrete5_Bad5` is not assignable to `Template5`
-protocols_explicit.py:56:9: error[invalid-assignment] Object of type `tuple[int, int, str]` is not assignable to attribute `rgb` of type `tuple[int, int, int]`
 protocols_generic.py:40:24: error[invalid-assignment] Object of type `Concrete1` is not assignable to `Proto1[int, str]`
 protocols_generic.py:56:20: error[invalid-assignment] Object of type `Box[int | float]` is not assignable to `Box[int]`
 protocols_generic.py:66:25: error[invalid-assignment] Object of type `Sender[int]` is not assignable to `Sender[int | float]`
@@ -856,8 +851,6 @@
 qualifiers_annotated.py:87:1: error[call-non-callable] Object of type `GenericAlias` is not callable
 qualifiers_annotated.py:88:1: error[call-non-callable] Object of type `GenericAlias` is not callable
 qualifiers_final_annotation.py:18:7: error[invalid-type-form] Type qualifier `typing.Final` expected exactly 1 argument, got 2
-qualifiers_final_annotation.py:54:9: error[invalid-assignment] Cannot assign to final attribute `ID5` in `__init__` because it already has a value at class level
-qualifiers_final_annotation.py:65:9: error[invalid-assignment] Cannot assign to final attribute `ID7` on type `Self@method1`
 qualifiers_final_annotation.py:71:1: error[invalid-assignment] Reassignment of `Final` symbol `RATE` is not allowed: Symbol later reassigned here
 qualifiers_final_annotation.py:81:1: error[invalid-assignment] Cannot assign to final attribute `DEFAULT_ID` on type `<class 'ClassB'>`
 qualifiers_final_annotation.py:118:9: error[invalid-type-form] Type qualifier `typing.Final` is not allowed in type expressions (only in annotation expressions)
@@ -1005,5 +998,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1007 diagnostics
+Found 1000 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-25 10:33_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
- zipp/__init__.py:94:16: error[unresolved-attribute] Object of type `Self@__getstate__` has no attribute `_saved___init__`
- zipp/__init__.py:94:43: error[unresolved-attribute] Object of type `Self@__getstate__` has no attribute `_saved___init__`
- zipp/__init__.py:436:16: error[no-matching-overload] No overload of bound method `format` matches arguments
- Found 4 diagnostics
+ Found 1 diagnostic

pyp (https://github.com/hauntsaninja/pyp)
+ pyp.py:468:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5 diagnostics
+ Found 6 diagnostics

bidict (https://github.com/jab/bidict)
- bidict/_base.py:468:48: error[parameter-already-assigned] Multiple values provided for parameter `unwrites` of bound method `_write`
- bidict/_base.py:519:16: error[invalid-return-type] Return type does not match returned value: expected `BT@__ror__`, found `BidictBase[Any, Any]`
- bidict/_orderedbase.py:71:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Self@__init__` with custom `__set__` method
- bidict/_orderedbase.py:77:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbase.py:78:9: error[invalid-assignment] Object of type `Node` is not assignable to attribute `prv` on type `Node | WeakAttr[Node]`
- bidict/_orderedbase.py:82:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbase.py:82:24: error[invalid-assignment] Object of type `Self@relink` is not assignable to attribute `prv` on type `Node | WeakAttr[Node]`
- bidict/_orderedbase.py:110:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbidict.py:80:9: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbidict.py:81:9: error[invalid-assignment] Object of type `Node` is not assignable to attribute `prv` on type `Node | WeakAttr[Node]`
- bidict/_orderedbidict.py:86:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- bidict/_orderedbidict.py:87:24: error[invalid-assignment] Object of type `Node` is not assignable to attribute `nxt` on type `Unknown | Node`
- bidict/_orderedbidict.py:91:13: error[invalid-assignment] Invalid assignment to data descriptor attribute `nxt` on type `Node` with custom `__set__` method
- Found 16 diagnostics
+ Found 3 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.py:4065:16: warning[possibly-missing-attribute] Attribute `result` may be missing on object of type `Unknown | None`
- more_itertools/more.py:5420:12: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- more_itertools/more.py:5422:20: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
- more_itertools/more.py:5423:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- more_itertools/more.py:5424:21: error[invalid-assignment] Cannot assign to a subscript on an object of type `None`
- more_itertools/more.py:5425:28: error[not-iterable] Object of type `None` is not iterable
- Found 33 diagnostics
+ Found 27 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- src/pegen/grammar_parser.py:160:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str] | None`, found `tuple[str, None]`
+ src/pegen/grammar_parser.py:160:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str] | None`, found `tuple[@Todo, None]`
- src/pegen/grammar_parser.py:438:31: error[no-matching-overload] No overload of bound method `join` matches arguments
- src/pegen/parser.py:28:19: error[unresolved-attribute] Object of type `F@logger` has no attribute `__name__`
- src/pegen/parser.py:48:19: error[unresolved-attribute] Object of type `F@memoize` has no attribute `__name__`
- src/pegen/python_generator.py:270:51: error[unresolved-attribute] Object of type `GrammarVisitor` has no attribute `keywords`
- src/pegen/python_generator.py:271:56: error[unresolved-attribute] Object of type `GrammarVisitor` has no attribute `soft_keywords`
- Found 50 diagnostics
+ Found 45 diagnostics

attrs (https://github.com/python-attrs/attrs)
- src/attr/_make.py:797:13: error[invalid-assignment] Object of type `ReferenceType[Unknown]` is not assignable to attribute `__attrs_base_of_slotted__` on type `Unknown | type`
- src/attr/_make.py:842:13: error[invalid-assignment] Object of type `Literal[False]` is not assignable to attribute `__attrs_own_setattr__` on type `Unknown | type`
- src/attr/_make.py:1061:13: error[invalid-argument-type] Argument to function `_make_hash_script` is incorrect: Expected `list[Attribute | Unknown]`, found `Unknown | type`
- src/attr/_make.py:1107:26: error[not-iterable] Object of type `Unknown | type` may not be iterable
- src/attr/_make.py:1139:41: error[invalid-argument-type] Argument to function `_make_eq_script` is incorrect: Expected `list[Unknown]`, found `Unknown | type`
- src/attr/_make.py:1162:18: error[not-iterable] Object of type `Unknown | type` may not be iterable
- src/attr/_make.py:2598:65: error[unresolved-attribute] Object of type `Self@__getstate__` has no attribute `metadata`
- tests/test_cmp.py:321:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
- tests/test_cmp.py:329:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
- tests/test_cmp.py:389:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
- tests/test_cmp.py:397:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
- tests/test_cmp.py:407:18: warning[possibly-missing-attribute] Attribute `__lt__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:415:18: warning[possibly-missing-attribute] Attribute `__le__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:425:18: warning[possibly-missing-attribute] Attribute `__gt__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:435:18: warning[possibly-missing-attribute] Attribute `__ge__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:461:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
- tests/test_cmp.py:469:16: warning[possibly-missing-attribute] Attribute `strip` may be missing on object of type `Unknown | str | None`
- tests/test_cmp.py:479:18: warning[possibly-missing-attribute] Attribute `__lt__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:487:18: warning[possibly-missing-attribute] Attribute `__le__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:495:18: warning[possibly-missing-attribute] Attribute `__gt__` may be missing on object of type `Unknown | type`
- tests/test_cmp.py:503:18: warning[possibly-missing-attribute] Attribute `__ge__` may be missing on object of type `Unknown | type`
- tests/test_make.py:2174:24: warning[possibly-missing-attribute] Attribute `__qualname__` may be missing on object of type `Unknown | ((...) -> Unknown)`
- Found 596 diagnostics
+ Found 574 diagnostics

packaging (https://github.com/pypa/packaging)
+ src/packaging/metadata.py:575:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/packaging/requirements.py:47:13: error[invalid-assignment] Object of type `list[Divergent]` is not assignable to attribute `_markers` on type `Marker | None`
- Found 21 diagnostics
+ Found 23 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:3898:38: error[no-matching-overload] No overload of bound method `split` matches arguments
- aioredis/client.py:3898:38: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | bytes | memoryview[int] | ... omitted 3 union elements`
- aioredis/client.py:3898:53: error[invalid-argument-type] Argument to bound method `split` is incorrect: Expected `Buffer | None`, found `Literal[" "]`
- aioredis/client.py:3899:13: error[no-matching-overload] No overload of bound method `match` matches arguments
- aioredis/client.py:4250:27: error[no-matching-overload] No overload of bound method `get` matches arguments
- aioredis/client.py:4252:27: error[no-matching-overload] No overload of bound method `get` matches arguments
- aioredis/connection.py:206:20: error[invalid-return-type] Return type does not match returned value: expected `ResponseError`, found `Exception | @Todo`
+ aioredis/connection.py:352:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[int] | str | ... omitted 4 union elements`, found `(@Todo & ~bytes) | int | list[bytes | memoryview[int] | str | ... omitted 5 union elements] | ... omitted 4 union elements`
+ aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[int] | str | ... omitted 4 union elements`, found `@Todo | int | list[@Todo]`
+ aioredis/connection.py:803:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- aioredis/lock.py:251:19: error[call-non-callable] Object of type `None` is not callable
- aioredis/lock.py:279:19: error[call-non-callable] Object of type `None` is not callable
- aioredis/lock.py:299:23: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | int | float | None` and `Literal[1000]`
- aioredis/lock.py:301:19: error[call-non-callable] Object of type `None` is not callable
- Found 29 diagnostics
+ Found 20 diagnostics

parso (https://github.com/davidhalter/parso)
- parso/normalizer.py:56:13: error[unresolved-attribute] Object of type `type` has no attribute `feed_node`
- parso/normalizer.py:62:13: error[unresolved-attribute] Object of type `type` has no attribute `feed_node`
- parso/pgen2/generator.py:100:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, DFAState[Unknown]]`
- parso/pgen2/generator.py:105:17: error[invalid-assignment] Cannot assign to a subscript on an object of type `Mapping[str, DFAState[Unknown]]`
- parso/python/errors.py:415:18: warning[possibly-missing-attribute] Attribute `add_block` may be missing on object of type `Unknown | None | _Context`
- parso/python/errors.py:416:24: warning[possibly-missing-attribute] Attribute `blocks` may be missing on object of type `Unknown | None | _Context`
- parso/python/errors.py:431:28: warning[possibly-missing-attribute] Attribute `parent_context` may be missing on object of type `Unknown | None | _Context`
- parso/python/errors.py:432:13: warning[possibly-missing-attribute] Attribute `close_child_context` may be missing on object of type `Unknown | None`
- parso/python/errors.py:470:32: warning[possibly-missing-attribute] Attribute `add_context` may be missing on object of type `Unknown | None | _Context`
- parso/python/errors.py:489:9: warning[possibly-missing-attribute] Attribute `finalize` may be missing on object of type `Unknown | None | _Context`
- parso/python/pep8.py:120:13: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Literal[" "]`
- parso/python/pep8.py:258:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:259:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:263:17: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:270:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:271:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:276:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:277:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:432:25: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None`
- parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:441:51: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:444:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:449:29: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
- parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:631:33: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `None | Unknown`
- parso/python/pep8.py:651:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `None | Unknown`
- parso/python/pep8.py:656:41: warning[possibly-missing-attribute] Attribute `prefix` may be missing on object of type `Unknown | None`
- parso/python/pep8.py:657:41: warning[possibly-missing-attribute] Attribute `prefix` may be missing on object of type `Unknown | None`
- parso/python/pep8.py:679:21: error[unresolved-attribute] Object of type `Self@_analyse_non_prefix` has no attribute `add_issuadd_issue`
- parso/python/pep8.py:767:16: error[unresolved-attribute] Object of type `Self@is_issue` has no attribute `_newline_count`
- parso/python/tree.py:78:12: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `type`
- parso/python/tree.py:79:20: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `children`
- parso/python/tree.py:80:14: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `type`
- parso/python/tree.py:81:20: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `children`
- parso/python/tree.py:81:34: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `children`
- parso/python/tree.py:85:27: error[unresolved-attribute] Object of type `Self@get_doc_node` has no attribute `parent`
- parso/python/tree.py:110:18: error[unresolved-attribute] Object of type `Self@get_name_of_position` has no attribute `children`
- parso/python/tree.py:219:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:222:24: error[unresolved-attribute] Object of type `BaseNode | None` has no attribute `name`
- parso/python/tree.py:228:24: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:235:28: error[unresolved-attribute] Object of type `BaseNode` has no attribute `get_defined_names`
- parso/python/tree.py:306:20: error[unresolved-attribute] Object of type `Self@__eq__` has no attribute `value`
- parso/python/tree.py:311:21: error[unresolved-attribute] Object of type `Self@__hash__` has no attribute `value`
- parso/python/tree.py:371:20: error[unresolved-attribute] Object of type `Self@__repr__` has no attribute `name`
- parso/python/tree.py:453:12: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:454:25: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:456:12: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:457:16: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:458:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:458:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:460:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:554:31: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:558:13: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:561:16: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:767:23: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:785:41: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:806:20: error[unresolved-attribute] Object of type `Self@get_path_for_name` has no attribute `_aliases`
- parso/python/tree.py:810:21: error[unresolved-attribute] Object of type `Self@get_path_for_name` has no attribute `get_paths`
- parso/python/tree.py:844:20: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:856:30: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:869:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:876:23: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:917:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:923:25: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:924:27: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:931:23: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:971:16: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:1049:23: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:1057:20: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:1058:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:1060:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:1069:20: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:1072:21: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:1105:24: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/python/tree.py:1161:17: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:1163:34: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:1163:61: error[invalid-argument-type] Argument to bound method `index` is incorrect: Expected `NodeOrLeaf`, found `Literal["*"]`
- parso/python/tree.py:1170:34: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/python/tree.py:1170:61: error[invalid-argument-type] Argument to bound method `index` is incorrect: Expected `NodeOrLeaf`, found `Literal["/"]`
- parso/tree.py:63:28: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/tree.py:82:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/tree.py:94:17: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/tree.py:98:20: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `BaseNode | None`
- parso/tree.py:106:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- parso/tree.py:120:17: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
- parso/tree.py:124:20: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `BaseNode | None`
- parso/tree.py:132:24: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `Unknown | NodeOrLeaf`
- Found 203 diagnostics
+ Found 95 diagnostics

anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:184:20: error[invalid-return-type] Return type does not match returned value: expected `AbstractEventLoop`, found `AbstractEventLoop | None`
- src/anyio/_backends/_asyncio.py:258:17: warning[possibly-missing-attribute] Attribute `call_soon_threadsafe` may be missing on object of type `AbstractEventLoop | None`
+ src/anyio/_backends/_asyncio.py:571:36: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_backends/_asyncio.py:576:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:497:21: error[unresolved-attribute] Object of type `Task[Unknown]` has no attribute `uncancel`
- src/anyio/_backends/_asyncio.py:843:28: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_effectively_cancelled`
- src/anyio/_backends/_asyncio.py:861:40: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_host_task`
- src/anyio/_backends/_asyncio.py:864:28: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_host_task`
+ src/anyio/_backends/_asyncio.py:1069:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_asyncio.py:888:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `CancelScope | None`, found `CancelScope`
- src/anyio/_backends/_asyncio.py:890:9: error[unresolved-attribute] Object of type `CancelScope` has no attribute `_tasks`
+ src/anyio/_backends/_asyncio.py:2164:32: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_backends/_asyncio.py:2167:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_backends/_asyncio.py:2168:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/anyio/_backends/_trio.py:177:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_backends/_trio.py:938:9: warning[possibly-missing-attribute] Attribute `send_nowait` may be missing on object of type `MemoryObjectSendStream[Unknown] | None`
+ src/anyio/_backends/_trio.py:430:16: warning[possibly-unresolved-reference] Name `data` used when possibly not defined
+ src/anyio/_backends/_trio.py:431:24: warning[possibly-unresolved-reference] Name `data` used when possibly not defined
+ src/anyio/_backends/_trio.py:444:29: warning[possibly-unresolved-reference] Name `bytes_sent` used when possibly not defined
+ src/anyio/_backends/_trio.py:529:9: warning[possibly-unresolved-reference] Name `trio_socket` used when possibly not defined
+ src/anyio/_backends/_trio.py:530:29: warning[possibly-unresolved-reference] Name `trio_socket` used when possibly not defined
+ src/anyio/_backends/_trio.py:545:33: warning[possibly-unresolved-reference] Name `trio_socket` used when possibly not defined
+ src/anyio/_backends/_trio.py:554:32: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `tuple[bytes, tuple[str, int]]`
+ src/anyio/_backends/_trio.py:576:32: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `bytes`
+ src/anyio/_backends/_trio.py:597:32: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `tuple[bytes, str]`
+ src/anyio/_backends/_trio.py:621:32: error[invalid-return-type] Function can implicitly return `None`, which is not assignable to return type `bytes`
- src/anyio/_core/_fileio.py:407:51: error[unknown-argument] Argument `case_sensitive` does not match any known parameter of bound method `match`
- src/anyio/_core/_fileio.py:496:44: error[unknown-argument] Argument `case_sensitive` does not match any known parameter of bound method `glob`
+ src/anyio/_core/_fileio.py:510:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_fileio.py:511:17: error[unknown-argument] Argument `case_sensitive` does not match any known parameter of bound method `glob`
- src/anyio/_core/_fileio.py:512:17: error[unknown-argument] Argument `recurse_symlinks` does not match any known parameter of bound method `glob`
- src/anyio/_core/_fileio.py:647:57: error[unknown-argument] Argument `walk_up` does not match any known parameter of bound method `relative_to`
- src/anyio/_core/_fileio.py:687:45: error[unknown-argument] Argument `case_sensitive` does not match any known parameter of bound method `rglob`
+ src/anyio/_core/_fileio.py:701:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/anyio/_core/_fileio.py:702:17: error[unknown-argument] Argument `case_sensitive` does not match any known parameter of bound method `rglob`
- src/anyio/_core/_fileio.py:703:17: error[unknown-argument] Argument `recurse_symlinks` does not match any known parameter of bound method `rglob`
- src/anyio/_core/_tempfile.py:330:15: error[no-matching-overload] No overload of bound method `write` matches arguments
- src/anyio/streams/stapled.py:121:34: error[invalid-argument-type] Argument to bound method `extend` is incorrect: Expected `Iterable[Listener[T_Stream@MultiListener]]`, found `Sequence[Listener[object]]`

janus (https://github.com/aio-libs/janus)
- janus/__init__.py:714:18: error[invalid-argument-type] Argument to function `heappush` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- janus/__init__.py:717:24: error[invalid-argument-type] Argument to function `heappop` is incorrect: Argument type `T@PriorityQueue` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 3 diagnostics
+ Found 1 diagnostic

paroxython (https://github.com/laowantong/paroxython)
- paroxython/recommend_programs.py:263:35: error[invalid-argument-type] Argument to function `cost_bucket` is incorrect: Expected `int`, found `Unknown | int | float`
- Found 7 diagnostics
+ Found 6 diagnostics

nionutils (https://github.com/nion-software/nionutils)
- nion/utils/ListModel.py:632:59: error[unresolved-attribute] Object of type `object` has no attribute `listen`
- nion/utils/ListModel.py:633:57: error[unresolved-attribute] Object of type `object` has no attribute `listen`
- nion/utils/ListModel.py:792:55: error[unresolved-attribute] Object of type `object` has no attribute `listen`
- nion/utils/ListModel.py:793:53: error[unresolved-attribute] Object of type `object` has no attribute `listen`
- Found 23 diagnostics
+ Found 19 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- chess/__init__.py:1558:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `BaseBoard`
- chess/__init__.py:1842:20: error[invalid-return-type] Return type does not match returned value: expected `Self@root`, found `Board`
- chess/engine.py:289:9: error[invalid-assignment] Object of type `(InfoDict & ~AlwaysFalsy) | dict[Unknown, Unknown]` is not assignable to attribute `info` of type `InfoDict`
- chess/engine.py:506:20: error[unsupported-operator] Operator `<` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
- chess/engine.py:512:20: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
- chess/engine.py:518:20: error[unsupported-operator] Operator `>` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
- chess/engine.py:524:20: error[unsupported-operator] Operator `>=` is not supported for types `int` and `None`, in comparing `tuple[bool, bool, bool, int, int | None]` with `tuple[bool, bool, bool, int, int | None]`
- chess/pgn.py:1050:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `Headers`
+ chess/engine.py:878:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ chess/engine.py:905:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ chess/engine.py:908:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ chess/pgn.py:353:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ chess/syzygy.py:1009:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ chess/syzygy.py:1012:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ chess/syzygy.py:1015:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ chess/syzygy.py:1018:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- chess/variant.py:876:16: error[invalid-return-type] Return type does not match returned value: expected `Self@copy`, found `CrazyhousePocket`
- Found 16 diagnostics
+ Found 15 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- pyinstrument/context_manager.py:41:9: error[invalid-assignment] Object of type `dict[str, @Todo]` is not assignable to attribute `options` of type `ProfileContextOptions`
+ pyinstrument/magic/_utils.py:73:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/magic/_utils.py:74:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pyinstrument/vendor/decorator.py:165:31: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None`

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ tests/conftest.py:298:132: warning[unused-ignore-comment] Unused `ty: ignore` directive: 'invalid-argument-type'
- Found 169 diagnostics
+ Found 170 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/augmentation/container/augment.py:313:84: error[invalid-argument-type] Argument to bound method `_preproc_dict_data` is incorrect: Expected `dict[str, Unknown | list[Unknown] | Boxes | Keypoints]`, found `(Unknown & Top[dict[Unknown, Unknown]]) | (Boxes & Top[dict[Unknown, Unknown]]) | (Keypoints & Top[dict[Unknown, Unknown]]) | dict[str, Unknown | list[Unknown] | Boxes | Keypoints]`
+ kornia/augmentation/container/augment.py:343:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/augmentation/container/augment.py:405:35: error[unresolved-attribute] Object of type `object` has no attribute `type`
- kornia/augmentation/container/augment.py:414:35: error[unresolved-attribute] Object of type `object` has no attribute `type`
- kornia/augmentation/container/augment.py:439:84: error[invalid-argument-type] Argument to bound method `_preproc_dict_data` is incorrect: Expected `dict[str, Unknown | list[Unknown] | Boxes | Keypoints]`, found `(Unknown & Top[dict[Unknown, Unknown]]) | (Boxes & Top[dict[Unknown, Unknown]]) | (Keypoints & Top[dict[Unknown, Unknown]]) | dict[str, Unknown | list[Unknown] | Boxes | Keypoints]`
+ kornia/augmentation/container/augment.py:473:99: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/nerf/data_utils.py:100:23: warning[possibly-missing-attribute] Attribute `to` may be missing on object of type `Unknown | str`
+ kornia/nerf/data_utils.py:100:23: warning[possibly-missing-attribute] Attribute `to` may be missing on object of type `@Todo | str`
- Found 763 diagnostics
+ Found 761 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/mirror.py:264:13: error[invalid-argument-type] Argument to function `max` is incorrect: Argument type `Unknown | int | None` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- src/bandersnatch/mirror.py:353:52: error[invalid-argument-type] Argument to bound method `sync_index_page` is incorrect: Expected `int`, found `int | None`
- Found 80 diagnostics
+ Found 78 diagnostics

itsdangerous (https://github.com/pallets/itsdangerous)
+ src/itsdangerous/serializer.py:261:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/itsdangerous/serializer.py:263:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/itsdangerous/serializer.py:318:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/itsdangerous/serializer.py:320:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2 diagnostics
+ Found 6 diagnostics

stone (https://github.com/dropbox/stone)
- stone/backends/js_client.py:77:43: warning[possibly-missing-attribute] Attribute `filename` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:97:12: warning[possibly-missing-attribute] Attribute `attribute_comment` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:98:30: warning[possibly-missing-attribute] Attribute `attribute_comment` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:106:12: warning[possibly-missing-attribute] Attribute `class_name` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:107:51: warning[possibly-missing-attribute] Attribute `class_name` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:113:12: warning[possibly-missing-attribute] Attribute `wrap_response_in` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:114:43: warning[possibly-missing-attribute] Attribute `wrap_response_in` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:124:56: warning[possibly-missing-attribute] Attribute `wrap_error_in` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:167:27: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- stone/backends/js_client.py:170:29: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- stone/backends/js_types.py:77:43: warning[possibly-missing-attribute] Attribute `filename` may be missing on object of type `Unknown | None`
- stone/backends/js_types.py:81:54: warning[possibly-missing-attribute] Attribute `extra_arg` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:111:54: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:113:41: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:120:42: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:132:42: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:149:29: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:154:38: warning[possibly-missing-attribute] Attribute `module_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:158:38: warning[possibly-missing-attribute] Attribute `module_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:166:27: warning[possibly-missing-attribute] Attribute `module_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:168:39: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:173:33: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:176:27: warning[possibly-missing-attribute] Attribute `class_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:179:35: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:195:54: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:205:39: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:216:42: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:228:17: warning[possibly-missing-attribute] Attribute `class_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:231:38: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:241:58: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:247:35: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:252:40: warning[possibly-missing-attribute] Attribute `class_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:271:62: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:274:49: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:281:50: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:283:43: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:291:43: warning[possibly-missing-attribute] Attribute `style_to_request` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:298:42: warning[possibly-missing-attribute] Attribute `client_args` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:391:50: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:400:25: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:405:35: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:416:54: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:421:43: warning[possibly-missing-attribute] Attribute `style_to_request` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_client.py:428:42: warning[possibly-missing-attribute] Attribute `client_args` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_types.py:127:12: warning[possibly-missing-attribute] Attribute `documentation` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_types.py:169:20: warning[possibly-missing-attribute] Attribute `documentation` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_types.py:179:12: warning[possibly-missing-attribute] Attribute `documentation` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_types.py:221:16: warning[possibly-missing-attribute] Attribute `documentation` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_types.py:235:20: warning[possibly-missing-attribute] Attribute `documentation` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_types.py:257:16: warning[possibly-missing-attribute] Attribute `exclude_from_analysis` may be missing on object of type `Unknown | None`
- stone/backends/obj_c_types.py:269:16: warning[possibly-missing-attribute] Attribute `exclude_from_analysis` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:121:53: warning[possibly-missing-attribute] Attribute `module_name` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:137:45: warning[possibly-missing-attribute] Attribute `class_name` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:153:54: warning[possibly-missing-attribute] Attribute `types_package` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:175:12: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:176:85: warning[possibly-missing-attribute] Attribute `auth_type` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:385:12: warning[possibly-missing-attribute] Attribute `attribute_comment` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:386:30: warning[possibly-missing-attribute] Attribute `attribute_comment` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:489:53: warning[possibly-missing-attribute] Attribute `error_class_path` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:495:32: warning[possibly-missing-attribute] Attribute `error_class_path` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:513:26: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:514:44: warning[possibly-missing-attribute] Attribute `types_package` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:525:21: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- stone/backends/python_client.py:550:17: warning[possibly-missing-attribute] Attribute `types_package` may be missing on object of type `Unknown | None`
- stone/backends/python_rsrc/stone_base.py:78:13: warning[possibly-missing-attribute] Attribute `validate_type_only` may be missing on object of type `Unknown | None`
- stone/backends/python_rsrc/stone_base.py:80:21: warning[possibly-missing-attribute] Attribute `validate` may be missing on object of type `Unknown | None`
- stone/backends/python_rsrc/stone_validators.py:350:33: warning[possibly-missing-attribute] Attribute `match` may be missing on object of type `Unknown | None`
- stone/backends/python_types.py:208:16: warning[possibly-missing-attribute] Attribute `route_method` may be missing on object of type `Unknown | None`
- stone/backends/python_types.py:209:39: warning[possibly-missing-attribute] Attribute `route_method` may be missing on object of type `Unknown | None`
- stone/backends/python_types.py:210:24: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- stone/backends/swift_client.py:145:16: warning[possibly-missing-attribute] Attribute `objc` may be missing on object of type `Unknown | None`
- stone/backends/swift_client.py:150:42: warning[possibly-missing-attribute] Attribute `class_name` may be missing on object of type `Unknown | None`
- stone/backends/swift_client.py:153:53: warning[possibly-missing-attribute] Attribute `transport_client_name` may be missing on object of type `Unknown | None`
- stone/backends/swift_client.py:155:12: warning[possibly-missing-attribute] Attribute `objc` may be missing on object of type `Unknown | None`
- stone/backends/swift_client.py:160:70: warning[possibly-missing-attribute] Attribute `module_name` may be missing on object of type `Unknown | None`
- stone/backends/swift_client.py:166:67: warning[possibly-missing-attribute] Attribute `module_name` may

... (truncated 29124 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- TOTAL MEMORY USAGE: ~66MB
+ TOTAL MEMORY USAGE: ~63MB
-     memo metadata = ~8MB
+     memo metadata = ~7MB

trio (https://github.com/python-trio/trio)
-     memo metadata = ~33MB
+     memo metadata = ~31MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~301MB
+ TOTAL MEMORY USAGE: ~273MB
-     struct metadata = ~20MB
+     struct metadata = ~17MB
-     struct fields = ~20MB
+     struct fields = ~17MB
-     memo metadata = ~76MB
+     memo metadata = ~63MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~626MB
+ TOTAL MEMORY USAGE: ~596MB
-     struct metadata = ~40MB
+     struct metadata = ~38MB
-     struct fields = ~44MB
+     struct fields = ~42MB
-     memo metadata = ~138MB
+     memo metadata = ~131MB


```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-25 10:36_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 16,065 | 0 |
| `possibly-missing-attribute` | 8 | 3,513 | 49 |
| `invalid-argument-type` | 9 | 2,783 | 162 |
| `unused-ignore-comment` | 1,692 | 0 | 0 |
| `unsupported-operator` | 2 | 1,020 | 16 |
| `invalid-assignment` | 8 | 701 | 27 |
| `invalid-return-type` | 80 | 538 | 39 |
| `non-subscriptable` | 0 | 573 | 0 |
| `no-matching-overload` | 1 | 352 | 0 |
| `call-non-callable` | 0 | 271 | 0 |
| `not-iterable` | 0 | 160 | 2 |
| `too-many-positional-arguments` | 0 | 160 | 0 |
| `missing-argument` | 0 | 94 | 0 |
| `possibly-unresolved-reference` | 70 | 0 | 0 |
| `type-assertion-failure` | 13 | 0 | 57 |
| `invalid-key` | 0 | 56 | 0 |
| `unknown-argument` | 0 | 31 | 0 |
| `deprecated` | 0 | 28 | 0 |
| `redundant-cast` | 0 | 15 | 0 |
| `division-by-zero` | 4 | 9 | 0 |
| `parameter-already-assigned` | 2 | 11 | 0 |
| `invalid-await` | 0 | 10 | 0 |
| `index-out-of-bounds` | 0 | 5 | 1 |
| `invalid-context-manager` | 0 | 5 | 0 |
| `invalid-raise` | 0 | 4 | 0 |
| `invalid-attribute-access` | 0 | 2 | 0 |
| `missing-typed-dict-key` | 0 | 2 | 0 |
| `unsupported-base` | 0 | 2 | 0 |
| `invalid-type-form` | 0 | 1 | 0 |
| **Total** | **1,889** | **26,411** | **353** |

**[Full report with detailed diff](https://david-type-t-todo.ecosystem-663.pages.dev/diff)** ([timing results](https://david-type-t-todo.ecosystem-663.pages.dev/timing))




---

_Comment by @codspeed-hq[bot] on 2025-11-25 10:43_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-t-todo?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21626 will **improve performances by ×2.4**

<sub>Comparing <code>david/type-t-todo</code> (57ae02a) with <code>main</code> (747c39a)[^unexpected-base]</sub>
[^unexpected-base]: No successful run was found on <code>main</code> (b19ddca) during the generation of this report, so 747c39a was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.



### Summary

`⚡ 7` improvements  
`✅ 15` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Simulation | [`` anyio ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-t-todo?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Aanyio%3A%3Aproject%3A%3Aanyio&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.4 s | 1.1 s | +30.43% |
| ⚡ | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-t-todo?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 200.4 s | 83.1 s | ×2.4 |
| ⚡ | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-t-todo?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 58.2 s | 52.6 s | +10.57% |
| ⚡ | WallTime | [`` medium[pandas] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-t-todo?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bpandas%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 57.6 s | 54.7 s | +5.25% |
| ⚡ | WallTime | [`` medium[static-frame] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-t-todo?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bstatic-frame%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 17.6 s | 16 s | +9.72% |
| ⚡ | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-t-todo?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.2 s | 6.4 s | +12.94% |
| ⚡ | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-t-todo?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.5 s | 2.1 s | +18.53% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/david%2Ftype-t-todo?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Closed by @sharkdp on 2025-11-25 10:59_

---
