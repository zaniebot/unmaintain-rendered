```yaml
number: 20963
title: "[ty] Improve error messages for unresolved attribute diagnostics"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/unresolved-attribute-error-messages
created_at: 2025-10-18T20:10:38Z
updated_at: 2025-10-19T10:09:31Z
url: https://github.com/astral-sh/ruff/pull/20963
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Improve error messages for unresolved attribute diagnostics

---

_Pull request opened by @AlexWaygood on 2025-10-18 20:10_

## Summary

- Type checkers (and type-checker authors) think in terms of types, but I think most Python users think in terms of values. Rather than saying that a _type_ `X` "has no attribute `foo`" (which I think sounds strange to many users), say that "an object of type `X` has no attribute `foo`"
- Special-case certain types so that the diagnostic messages read more like normal English: rather than saying "Type `<class 'Foo'>` has no attribute `bar`" or "Object of type `<class 'Foo'>` has no attribute `bar`", just say "Class `Foo` has no attribute `bar`"

## Test Plan

Mdtests and snapshots updated


---

_Label `ty` added by @AlexWaygood on 2025-10-18 20:10_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-18 20:10_

---

_Label `ty` added by @AlexWaygood on 2025-10-18 20:10_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-18 20:10_

---

_Comment by @github-actions[bot] on 2025-10-18 20:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-18 20:20:34.378128462 +0000
+++ new-output.txt	2025-10-18 20:20:37.737140240 +0000
@@ -240,10 +240,10 @@
 dataclasses_kwonly.py:61:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
 dataclasses_kwonly.py:61:14: error[parameter-already-assigned] Multiple values provided for parameter `b`
 dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported for types `DC1` and `DC2`
-dataclasses_postinit.py:28:7: error[unresolved-attribute] Type `DC1` has no attribute `x`
-dataclasses_postinit.py:29:7: error[unresolved-attribute] Type `DC1` has no attribute `y`
-dataclasses_slots.py:66:1: error[unresolved-attribute] Type `<class 'DC6'>` has no attribute `__slots__`
-dataclasses_slots.py:69:1: error[unresolved-attribute] Type `DC6` has no attribute `__slots__`
+dataclasses_postinit.py:28:7: error[unresolved-attribute] Object of type `DC1` has no attribute `x`
+dataclasses_postinit.py:29:7: error[unresolved-attribute] Object of type `DC1` has no attribute `y`
+dataclasses_slots.py:66:1: error[unresolved-attribute] Class `DC6` has no attribute `__slots__`
+dataclasses_slots.py:69:1: error[unresolved-attribute] Object of type `DC6` has no attribute `__slots__`
 dataclasses_transform_class.py:66:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_class.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_class.py:72:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
@@ -481,7 +481,7 @@
 generics_syntax_compatibility.py:26:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `M@method2 | K@method2`
 generics_syntax_declarations.py:17:1: error[invalid-generic-class] Cannot both inherit from `typing.Generic` and use PEP 695 type variables
 generics_syntax_declarations.py:25:20: error[invalid-generic-class] Cannot both inherit from subscripted `Protocol` and use PEP 695 type variables
-generics_syntax_declarations.py:32:9: error[unresolved-attribute] Type `T@ClassD` has no attribute `is_integer`
+generics_syntax_declarations.py:32:9: error[unresolved-attribute] Object of type `T@ClassD` has no attribute `is_integer`
 generics_syntax_declarations.py:48:17: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, int]`?
 generics_syntax_declarations.py:60:17: error[invalid-type-variable-constraints] TypeVar must have at least two constrained types
 generics_syntax_declarations.py:64:17: error[invalid-type-variable-constraints] TypeVar must have at least two constrained types
@@ -829,29 +829,29 @@
 specialtypes_none.py:21:7: error[invalid-argument-type] Argument to function `func1` is incorrect: Expected `None`, found `<class 'NoneType'>`
 specialtypes_none.py:27:1: error[invalid-assignment] Object of type `None` is not assignable to `Iterable[Unknown]`
 specialtypes_none.py:32:1: error[missing-argument] No argument provided for required parameter `value` of function `__eq__`
-specialtypes_promotions.py:13:5: warning[possibly-missing-attribute] Attribute `numerator` on type `int | float` may be missing
+specialtypes_promotions.py:13:5: warning[possibly-missing-attribute] Attribute `numerator` may be missing on object of type `int | float`
 specialtypes_type.py:44:1: error[type-assertion-failure] Argument does not have asserted type `TeamUser`
 specialtypes_type.py:56:7: error[invalid-argument-type] Argument to function `func4` is incorrect: Expected `type[BasicUser] | type[ProUser]`, found `<class 'TeamUser'>`
 specialtypes_type.py:76:17: error[invalid-type-form] type[...] must have exactly one type argument
 specialtypes_type.py:84:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
 specialtypes_type.py:98:5: error[type-assertion-failure] Argument does not have asserted type `tuple[type, ...]`
-specialtypes_type.py:99:17: error[unresolved-attribute] Type `type` has no attribute `unknown`
-specialtypes_type.py:100:17: error[unresolved-attribute] Type `type` has no attribute `unknown`
+specialtypes_type.py:99:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
+specialtypes_type.py:100:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:102:5: error[type-assertion-failure] Argument does not have asserted type `tuple[type, ...]`
 specialtypes_type.py:106:5: error[type-assertion-failure] Argument does not have asserted type `tuple[type, ...]`
-specialtypes_type.py:107:17: error[unresolved-attribute] Type `type` has no attribute `unknown`
-specialtypes_type.py:108:17: error[unresolved-attribute] Type `type` has no attribute `unknown`
+specialtypes_type.py:107:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
+specialtypes_type.py:108:17: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:110:5: error[type-assertion-failure] Argument does not have asserted type `tuple[type, ...]`
-specialtypes_type.py:117:5: error[unresolved-attribute] Type `type` has no attribute `unknown`
-specialtypes_type.py:120:5: error[unresolved-attribute] Type `type` has no attribute `unknown`
+specialtypes_type.py:117:5: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
+specialtypes_type.py:120:5: error[unresolved-attribute] Object of type `type` has no attribute `unknown`
 specialtypes_type.py:127:5: error[type-assertion-failure] Argument does not have asserted type `ProUser`
 specialtypes_type.py:137:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
 specialtypes_type.py:138:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
 specialtypes_type.py:139:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
 specialtypes_type.py:140:5: error[type-assertion-failure] Argument does not have asserted type `type[Any]`
-specialtypes_type.py:143:1: error[unresolved-attribute] Type `typing.Type` has no attribute `unknown`
-specialtypes_type.py:145:1: error[unresolved-attribute] Type `<class 'type'>` has no attribute `unknown`
-specialtypes_type.py:146:1: error[unresolved-attribute] Type `GenericAlias` has no attribute `unknown`
+specialtypes_type.py:143:1: error[unresolved-attribute] Object of type `typing.Type` has no attribute `unknown`
+specialtypes_type.py:145:1: error[unresolved-attribute] Class `type` has no attribute `unknown`
+specialtypes_type.py:146:1: error[unresolved-attribute] Object of type `GenericAlias` has no attribute `unknown`
 specialtypes_type.py:160:1: error[type-assertion-failure] Argument does not have asserted type `int`
 specialtypes_type.py:161:1: error[type-assertion-failure] Argument does not have asserted type `int`
 specialtypes_type.py:169:5: error[invalid-assignment] Object of type `type` is not assignable to `type[int]`
@@ -896,7 +896,7 @@
 typeddicts_operations.py:29:42: error[invalid-argument-type] Invalid argument to key "year" with declared type `int` on TypedDict `Movie`: value of type `float`
 typeddicts_operations.py:32:36: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "other"
 typeddicts_operations.py:37:20: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
-typeddicts_operations.py:62:1: error[unresolved-attribute] Type `MovieOptional` has no attribute `clear`
+typeddicts_operations.py:62:1: error[unresolved-attribute] Object of type `MovieOptional` has no attribute `clear`
 typeddicts_readonly.py:24:4: error[invalid-assignment] Cannot assign to key "members" on TypedDict `Band`: key is marked read-only
 typeddicts_readonly.py:50:4: error[invalid-assignment] Cannot assign to key "title" on TypedDict `Movie1`: key is marked read-only
 typeddicts_readonly.py:51:4: error[invalid-assignment] Cannot assign to key "year" on TypedDict `Movie1`: key is marked read-only
```
</details>


---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-10-18 20:19_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-10-18 20:19_

---

_Comment by @github-actions[bot] on 2025-10-18 20:25_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 9 | 0 | 17,093 |
| `possibly-missing-attribute` | 49 | 0 | 6,393 |
| **Total** | **58** | **0** | **23,486** |

**[Full report with detailed diff](https://alex-unresolved-attribute-er.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-unresolved-attribute-er.ecosystem-663.pages.dev/timing))


---

_Marked ready for review by @AlexWaygood on 2025-10-18 21:26_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-18 21:26_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-18 21:26_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-18 21:26_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-18 21:26_

---

_@sharkdp approved on 2025-10-19 06:59_

Thank you, a clear improvement!


---

_Merged by @AlexWaygood on 2025-10-19 09:58_

---

_Closed by @AlexWaygood on 2025-10-19 09:58_

---

_Branch deleted on 2025-10-19 09:58_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-19 10:09_

---
