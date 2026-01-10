```yaml
number: 19621
title: "[ty] fix a typo "
type: pull_request
state: merged
author: CodeMan62
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: typo
created_at: 2025-07-29T17:43:56Z
updated_at: 2025-07-29T17:54:49Z
url: https://github.com/astral-sh/ruff/pull/19621
synced_at: 2026-01-10T17:58:13Z
```

# [ty] fix a typo 

---

_Pull request opened by @CodeMan62 on 2025-07-29 17:43_

## Summary
I was working on a issue and trying to learn via PR's so i found this in #19435  I think the Member forget to check the review 

## Test Plan
N/A 


---

_Review requested from @carljm by @CodeMan62 on 2025-07-29 17:43_

---

_Review requested from @AlexWaygood by @CodeMan62 on 2025-07-29 17:43_

---

_Review requested from @sharkdp by @CodeMan62 on 2025-07-29 17:43_

---

_Review requested from @dcreager by @CodeMan62 on 2025-07-29 17:43_

---

_Comment by @github-actions[bot] on 2025-07-29 17:46_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-07-29 17:51:54.323653155 +0000
+++ new-output.txt	2025-07-29 17:51:54.386653500 +0000
@@ -246,8 +246,8 @@
 dataclasses_kwonly.py:61:9: error[invalid-argument-type] Argument is incorrect: Expected `int`, found `float`
 dataclasses_kwonly.py:61:14: error[parameter-already-assigned] Multiple values provided for parameter `b`
 dataclasses_order.py:50:4: error[unsupported-operator] Operator `<` is not supported for types `DC1` and `DC2`
-dataclasses_postinit.py:28:7: error[unresolved-attribute] Type `DC1` has no attribute `x`
-dataclasses_postinit.py:29:7: error[unresolved-attribute] Type `DC1` has no attribute `y`
+dataclasses_postinit.py:23:17: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[int]`, found `Literal[3]`
+dataclasses_postinit.py:23:23: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal[""]`
 dataclasses_slots.py:56:1: error[unresolved-attribute] Type `<class 'DC5'>` has no attribute `__slots__`
 dataclasses_slots.py:57:1: error[unresolved-attribute] Type `DC5` has no attribute `__slots__`
 dataclasses_slots.py:66:1: error[unresolved-attribute] Type `<class 'DC6'>` has no attribute `__slots__`
@@ -298,8 +298,8 @@
 dataclasses_usage.py:50:6: error[missing-argument] No argument provided for required parameter `unit_price`
 dataclasses_usage.py:51:28: error[invalid-argument-type] Argument is incorrect: Expected `int | float`, found `Literal["price"]`
 dataclasses_usage.py:52:36: error[too-many-positional-arguments] Too many positional arguments: expected 3, got 4
-dataclasses_usage.py:83:13: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
-dataclasses_usage.py:88:5: error[invalid-assignment] Object of type `dataclasses.Field[Literal[""]]` is not assignable to `int`
+dataclasses_usage.py:72:5: error[invalid-assignment] Object of type `Literal[0]` is not assignable to `InitVar[int]`
+dataclasses_usage.py:82:6: error[missing-argument] No argument provided for required parameter `b`
 dataclasses_usage.py:127:8: error[too-many-positional-arguments] Too many positional arguments: expected 1, got 2
 dataclasses_usage.py:130:1: error[missing-argument] No argument provided for required parameter `y` of bound method `__init__`
 dataclasses_usage.py:179:6: error[too-many-positional-arguments] Too many positional arguments to bound method `__init__`: expected 1, got 2
@@ -344,6 +344,7 @@
 enums_behaviors.py:32:1: error[type-assertion-failure] Argument does not have asserted type `Literal[Color.BLUE]`
 enums_behaviors.py:44:21: error[subclass-of-final-class] Class `ExtendedShape` cannot inherit from final class `Shape`
 enums_expansion.py:52:9: error[type-assertion-failure] Argument does not have asserted type `CustomFlags`
+enums_expansion.py:63:13: error[type-assertion-failure] Argument does not have asserted type `CustomFlags`
 enums_member_names.py:21:1: error[type-assertion-failure] Argument does not have asserted type `Literal["RED"]`
 enums_member_names.py:22:1: error[type-assertion-failure] Argument does not have asserted type `Literal["RED"]`
 enums_member_names.py:26:5: error[type-assertion-failure] Argument does not have asserted type `Literal["RED", "BLUE"]`
@@ -765,8 +766,6 @@
 overloads_evaluation.py:322:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
 protocols_class_objects.py:30:5: error[invalid-argument-type] Argument to function `fun` is incorrect: Expected `type[Proto]`, found `<class 'Concrete'>`
 protocols_class_objects.py:35:1: error[invalid-assignment] Object of type `<class 'Concrete'>` is not assignable to `type[Proto]`
-protocols_class_objects.py:58:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA1`
-protocols_class_objects.py:59:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA2`
 protocols_definition.py:79:1: error[invalid-assignment] Object of type `Concrete` is not assignable to `Template`
 protocols_definition.py:114:1: error[invalid-assignment] Object of type `Concrete2_Bad1` is not assignable to `Template2`
 protocols_definition.py:115:1: error[invalid-assignment] Object of type `Concrete2_Bad2` is not assignable to `Template2`
@@ -781,8 +780,6 @@
 protocols_merging.py:67:16: error[invalid-protocol] Protocol class `BadProto` cannot inherit from non-protocol class `SizedAndClosable3`
 protocols_merging.py:83:1: error[invalid-assignment] Object of type `SCConcrete1` is not assignable to `SizedAndClosable4`
 protocols_modules.py:26:1: error[invalid-assignment] Object of type `<module '_protocols_modules1'>` is not assignable to `Options2`
-protocols_modules.py:47:1: error[invalid-assignment] Object of type `<module '_protocols_modules2'>` is not assignable to `Reporter1`
-protocols_modules.py:48:1: error[invalid-assignment] Object of type `<module '_protocols_modules2'>` is not assignable to `Reporter2`
 protocols_modules.py:49:1: error[invalid-assignment] Object of type `<module '_protocols_modules2'>` is not assignable to `Reporter3`
 protocols_recursive.py:76:35: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T`
 protocols_recursive.py:81:1: error[type-assertion-failure] Argument does not have asserted type `list[int]`
@@ -892,4 +889,4 @@
 tuples_type_form.py:36:1: error[invalid-assignment] Object of type `tuple[Literal[1], Literal[2], Literal[3], Literal[""]]` is not assignable to `tuple[int, ...]`
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
-Found 893 diagnostics
+Found 890 diagnostics
```
</details>


---

_Comment by @github-actions[bot] on 2025-07-29 17:47_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood reviewed on 2025-07-29 17:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:10814 on 2025-07-29 17:50_

```suggestion
/// in the `TypeInference` builder already implicitly guarantees that each definition
```

---

_@AlexWaygood approved on 2025-07-29 17:50_

---

_Label `internal` added by @AlexWaygood on 2025-07-29 17:50_

---

_Label `ty` added by @AlexWaygood on 2025-07-29 17:50_

---

_Merged by @AlexWaygood on 2025-07-29 17:53_

---

_Closed by @AlexWaygood on 2025-07-29 17:53_

---

_Branch deleted on 2025-07-29 17:54_

---
