```yaml
number: 19947
title: "[ty] Add a Todo-type branch for `type[P]` where `P` is a protocol class"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/proto-meta-todo
created_at: 2025-08-17T14:04:28Z
updated_at: 2025-08-18T20:38:48Z
url: https://github.com/astral-sh/ruff/pull/19947
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Add a Todo-type branch for `type[P]` where `P` is a protocol class

---

_Pull request opened by @AlexWaygood on 2025-08-17 14:04_

## Summary

"Meta-protocols" (the type implied by `type[P]` where `P` is a protocol class) are an interesting and under-specified concept. We'll have to support them in some form eventually, because they're used in real-world code, including in typeshed -- but we don't yet, and this causes false positives. They're also far from the highest priority thing we need to fix in our protocol implementation currently, and the missing support for them is complicating matters (adding lots of new false positives) in other protocol work I'm doing. For now, therefore, this PR just adds some Todo-type branches to get rid of the false positives in user code.

Fixes https://github.com/astral-sh/ty/issues/582. It obviously fixes it in a pretty hacky way, but we have https://github.com/astral-sh/ty/issues/903 also open, and that issue tracks proper support for meta-protocols.

## Test Plan

Mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-08-17 14:04_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-17 14:04_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-17 14:04_

---

_Comment by @github-actions[bot] on 2025-08-17 14:06_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-18 20:36:32.675991738 +0000
+++ new-output.txt	2025-08-18 20:36:35.144024283 +0000
@@ -734,8 +734,6 @@
 overloads_evaluation.py:234:5: error[type-assertion-failure] Argument does not have asserted type `int`
 overloads_evaluation.py:291:33: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@example6`
 overloads_evaluation.py:322:5: error[type-assertion-failure] Argument does not have asserted type `list[int]`
-protocols_class_objects.py:30:5: error[invalid-argument-type] Argument to function `fun` is incorrect: Expected `type[Proto]`, found `<class 'Concrete'>`
-protocols_class_objects.py:35:1: error[invalid-assignment] Object of type `<class 'Concrete'>` is not assignable to `type[Proto]`
 protocols_class_objects.py:58:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA1`
 protocols_class_objects.py:59:1: error[invalid-assignment] Object of type `<class 'ConcreteA'>` is not assignable to `ProtoA2`
 protocols_definition.py:79:1: error[invalid-assignment] Object of type `Concrete` is not assignable to `Template`
@@ -861,5 +859,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 862 diagnostics
+Found 860 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Label `ty` added by @AlexWaygood on 2025-08-17 14:06_

---

_@AlexWaygood reviewed on 2025-08-17 14:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2123 on 2025-08-17 14:07_

these two assertions don't pass on `main`, leading to false positives on user code.

---

_Comment by @AlexWaygood on 2025-08-17 14:09_

The typing conformance diff shows two false positives going away: lines 30 and 35 [here](https://github.com/python/typing/blob/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance/tests/protocols_class_objects.py#L30-L35).

---

_Comment by @github-actions[bot] on 2025-08-17 14:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- tests/test_annotations.py:48:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:49:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:50:36: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:78:48: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:79:52: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:93:37: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:122:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:124:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:126:50: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:127:48: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:129:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:130:33: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:132:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:134:42: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:415:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:517:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:519:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:520:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:521:36: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:539:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:541:56: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_annotations.py:547:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'D'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:549:37: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'D'>`
- tests/test_annotations.py:564:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:566:48: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:567:48: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:568:48: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:582:48: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:583:48: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:584:48: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:596:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:598:33: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:599:50: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:614:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:615:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'B'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:617:46: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:618:33: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- tests/test_annotations.py:620:46: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:621:33: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- tests/test_annotations.py:662:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:663:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'B'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:665:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:666:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- tests/test_annotations.py:678:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:680:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_annotations.py:682:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- tests/test_annotations.py:684:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_converters.py:297:29: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:33:31: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:34:46: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:47:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:48:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:52:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:60:25: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:67:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:68:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:72:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:80:25: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:93:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:94:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:98:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:106:25: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:113:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:114:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:118:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_filters.py:126:25: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_forward_references.py:20:19: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- tests/test_forward_references.py:22:19: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_functional.py:167:25: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1'>`
- tests/test_functional.py:218:26: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `type`
- tests/test_functional.py:545:42: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_functional.py:546:44: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_functional.py:547:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_functional.py:646:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_functional.py:647:35: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_hooks.py:216:40: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_hooks.py:234:54: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Base'>`
- tests/test_make.py:482:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- tests/test_make.py:484:26: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- tests/test_make.py:485:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- tests/test_make.py:487:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_make.py:488:26: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_make.py:489:27: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_make.py:856:45: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_make.py:893:26: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'BaseClass'>`
- tests/test_make.py:894:26: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'SubClass'>`
- tests/test_make.py:1454:32: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
- tests/test_make.py:1465:32: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
- tests/test_make.py:2227:34: error[invalid-argument-type] Argument to function `fields_dict` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Cases'>`
- tests/test_next_gen.py:41:36: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `type`
- tests/test_next_gen.py:149:39: error[invalid-argument-type] Argument to function `fields_dict` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NewSchool'>`
- tests/test_next_gen.py:164:39: error[invalid-argument-type] Argument to function `fields_dict` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NewSchool'>`
- tests/test_slots.py:103:24: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1Slots'>`
- tests/test_slots.py:103:48: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1'>`
- tests/test_slots.py:545:41: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- tests/test_validators.py:884:23: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
- tests/test_validators.py:956:23: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
- tests/test_validators.py:1029:23: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
- tests/typing_example.py:133:13: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'AliasExample'>`
- tests/typing_example.py:134:13: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'AliasExample'>`
- tests/typing_example.py:421:13: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- tests/typing_example.py:422:17: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- tests/typing_example.py:426:14: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- tests/typing_example.py:427:18: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- tests/typing_example.py:515:28: error[invalid-argument-type] Argument to function `resolve_types` is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
- Found 660 diagnostics
+ Found 555 diagnostics

operator (https://github.com/canonical/operator)
- ops/framework.py:1164:40: error[invalid-argument-type] Argument to bound method `register_type` is incorrect: Expected `type[Serializable]`, found `<class 'StoredStateData'>`
- Found 98 diagnostics
+ Found 97 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_implementations.py:2252:47: error[invalid-argument-type] Argument to function `get_target_path` is incorrect: Expected `HasTarget`, found `(Importable@builds & ((...) -> object) & type[DataclassInstance] & ~partial[Unknown]) | (((...) -> R@builds) & ((...) -> object) & type[DataclassInstance] & ~partial[Unknown]) | (@Todo(unsupported nested subscript in type[X]) & ((...) -> object) & type[DataclassInstance] & ~partial[Unknown]) | (((...) -> Unknown) & ((...) -> object) & type[DataclassInstance]) | (@Todo(unsupported nested subscript in type[X]) & ((...) -> Unknown) & ((...) -> object) & type[DataclassInstance])`
- src/hydra_zen/structured_configs/_implementations.py:2523:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str | DataClass_ | Mapping[str, None | Sequence[str]]] | None`
+ src/hydra_zen/structured_configs/_implementations.py:2523:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str | DataClass_ | type[@Todo(type[T] for protocols)] | Mapping[str, None | Sequence[str]]] | None`
- src/hydra_zen/structured_configs/_implementations.py:3303:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str | DataClass_ | Mapping[str, None | Sequence[str]]] | None`
+ src/hydra_zen/structured_configs/_implementations.py:3303:54: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | list[str | DataClass_ | type[@Todo(type[T] for protocols)] | Mapping[str, None | Sequence[str]]] | None`
- src/hydra_zen/wrapper/_implementations.py:932:24: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | ListConfig | DictConfig`, found `(((...) -> Any) & type & ~type[HydraConf]) | (DataClass_ & type & ~type[HydraConf]) | (ListConfig & type & ~type[HydraConf]) | (DictConfig & type & ~type[HydraConf])`
- src/hydra_zen/wrapper/_implementations.py:941:16: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | ListConfig | DictConfig`, found `(((...) -> Any) & ~type) | (DataClass_ & ~type) | list[Any] | dict[Any, Any] | (ListConfig & ~type) | (DictConfig & ~type)`
+ src/hydra_zen/wrapper/_implementations.py:941:16: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | type[@Todo(type[T] for protocols)] | ListConfig | DictConfig`, found `(((...) -> Any) & ~type) | (DataClass_ & ~type) | list[Any] | dict[Any, Any] | (ListConfig & ~type) | (DictConfig & ~type)`
- src/hydra_zen/wrapper/_implementations.py:1479:9: error[invalid-parameter-default] Default value of type `def default_to_config(target: ((...) -> Any) | DataClass_ | list[Any] | dict[Any, Any] | ListConfig | DictConfig, CustomBuildsFn: @Todo(unsupported type[X] special form) = <class 'DefaultBuilds'>, **kw: Any) -> DataClass_ | ListConfig | DictConfig` is not assignable to annotated parameter type `(F@__call__, /) -> @Todo(Support for `typing.TypeAlias`)`
+ src/hydra_zen/wrapper/_implementations.py:1479:9: error[invalid-parameter-default] Default value of type `def default_to_config(target: ((...) -> Any) | DataClass_ | list[Any] | dict[Any, Any] | ListConfig | DictConfig, CustomBuildsFn: @Todo(unsupported type[X] special form) = <class 'DefaultBuilds'>, **kw: Any) -> DataClass_ | type[@Todo(type[T] for protocols)] | ListConfig | DictConfig` is not assignable to annotated parameter type `(F@__call__, /) -> @Todo(Support for `typing.TypeAlias`)`
- tests/annotations/declarations.py:434:26: error[invalid-argument-type] Argument to function `make_config` is incorrect: Expected `tuple[type[DataClass_], ...]`, found `tuple[type[DataClass], @Todo(unsupported nested subscript in type[X]), <class 'P3'>, @Todo(unsupported nested subscript in type[X])]`
+ tests/annotations/declarations.py:443:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 563 diagnostics
+ Found 561 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/cli/commands/export_schema.py:32:32: error[invalid-argument-type] Argument to function `print_schema` is incorrect: Expected `BaseSchema`, found `Schema`
- strawberry/cli/debug_server.py:25:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `BaseSchema`, found `Schema`
- strawberry/experimental/pydantic/utils.py:54:51: error[invalid-argument-type] Argument to function `fields` is incorrect: Expected `DataclassInstance`, found `type`
- strawberry/types/base.py:333:16: error[invalid-return-type] Return type does not match returned value: expected `type[WithStrawberryObjectDefinition]`, found `type`
- Found 377 diagnostics
+ Found 373 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- src/check_jsonschema/formats/__init__.py:55:5: error[invalid-assignment] Object of type `<class 'Draft202012Validator'>` is not assignable to `type[Validator]`
- Found 59 diagnostics
+ Found 58 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/_internal/_dataclasses.py:88:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/_internal/_generate_schema.py:1828:24: error[unresolved-attribute] Type `type[DataclassInstance]` has no attribute `__pydantic_fields_complete__`
- pydantic/_internal/_generate_schema.py:1834:80: error[unresolved-attribute] Type `type[DataclassInstance]` has no attribute `__pydantic_fields__`
- pydantic/_internal/_generate_schema.py:1842:33: error[invalid-argument-type] Argument to function `rebuild_dataclass_fields` is incorrect: Expected `type[PydanticDataclass]`, found `type[DataclassInstance]`
- pydantic/_internal/_mock_val_ser.py:217:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_validator__` of type `SchemaValidator | PluggableSchemaValidator`
- pydantic/_internal/_mock_val_ser.py:223:5: error[invalid-assignment] Object of type `MockValSer[Unknown]` is not assignable to attribute `__pydantic_serializer__` of type `SchemaSerializer`
- Found 770 diagnostics
+ Found 766 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/connectionpool.py:173:5: error[invalid-assignment] Object of type `<class 'HTTPConnection'>` is not assignable to `type[BaseHTTPConnection]`
- src/urllib3/connectionpool.py:978:5: error[invalid-assignment] Object of type `<class 'HTTPSConnection'>` is not assignable to `type[BaseHTTPSConnection]`
- src/urllib3/contrib/emscripten/__init__.py:13:5: error[invalid-assignment] Object of type `<class 'EmscriptenHTTPConnection'>` is not assignable to attribute `ConnectionCls` of type `type[BaseHTTPConnection]`
- src/urllib3/contrib/emscripten/__init__.py:14:5: error[invalid-assignment] Object of type `<class 'EmscriptenHTTPSConnection'>` is not assignable to attribute `ConnectionCls` of type `type[BaseHTTPSConnection]`
- src/urllib3/http2/__init__.py:35:5: error[invalid-assignment] Object of type `<class 'HTTP2Connection'>` is not assignable to attribute `ConnectionCls` of type `type[BaseHTTPSConnection]`
+ test/conftest.py:381:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/conftest.py:387:13: error[invalid-assignment] Object of type `None | <class 'HTTPConnection'>` is not assignable to attribute `ConnectionCls` of type `type[BaseHTTPConnection]`
+ test/conftest.py:387:13: error[invalid-assignment] Object of type `None | <class 'HTTPConnection'>` is not assignable to attribute `ConnectionCls` of type `type[@Todo(type[T] for protocols)]`
- Found 393 diagnostics
+ Found 389 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- com/win32com/demos/excelAddin.py:165:45: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'ExcelAddin'>`
- com/win32com/demos/excelRTDServer.py:434:45: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'TimeServer'>`
- com/win32com/demos/iebutton.py:211:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'PyWin32InternetExplorerButton'>`
- com/win32com/demos/ietoolbar.py:359:45: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'IEToolbar'>`
- com/win32com/demos/outlookAddin.py:131:45: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'OutlookAddin'>`
- com/win32com/servers/dictionary.py:129:27: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'DictionaryPolicy'>`
- com/win32com/servers/interp.py:54:52: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'Interpreter'>`
- com/win32com/servers/perfmon.py:36:29: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'PerfMonQuery'>`
- com/win32com/servers/test_pycomtest.py:180:45: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'PyCOMTest'>`
- com/win32com/servers/test_pycomtest.py:181:45: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'PyCOMTestMI'>`
- com/win32com/test/pippo_server.py:92:45: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'CPippo'>`
- com/win32comext/axscript/client/pyscript.py:432:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `Unknown | <class 'PyScript'>`
- com/win32comext/shell/demos/servers/column_provider.py:122:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'ColumnProvider'>`
- com/win32comext/shell/demos/servers/context_menu.py:119:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'ShellExtension'>`
- com/win32comext/shell/demos/servers/copy_hook.py:80:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'ShellExtension'>`
- com/win32comext/shell/demos/servers/empty_volume_cache.py:185:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'EmptyVolumeCache'>`
- com/win32comext/shell/demos/servers/folder_view.py:861:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'ShellFolder'>`
- com/win32comext/shell/demos/servers/folder_view.py:862:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'ContextMenu'>`
- com/win32comext/shell/demos/servers/icon_handler.py:78:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'ShellExtension'>`
- com/win32comext/shell/demos/servers/shell_view.py:967:9: error[invalid-argument-type] Argument to function `UseCommandLine` is incorrect: Expected `type[_RegisterClass]`, found `<class 'ShellFolderRoot'>`
- Found 2006 diagnostics
+ Found 1986 diagnostics

colour (https://github.com/colour-science/colour)
- colour/colorimetry/spectrum.py:1190:17: error[invalid-assignment] Object of type `<class 'SpragueInterpolator'>` is not assignable to `type[ProtocolInterpolator] | None`
- colour/colorimetry/spectrum.py:1192:17: error[invalid-assignment] Object of type `<class 'CubicSplineInterpolator'>` is not assignable to `type[ProtocolInterpolator] | None`
- colour/colorimetry/spectrum.py:1297:9: error[invalid-assignment] Object of type `type[ProtocolExtrapolator] | None | <class 'Extrapolator'>` is not assignable to `type[ProtocolExtrapolator] | None`
- colour/examples/algebra/examples_interpolation.py:107:5: error[invalid-argument-type] Argument to bound method `interpolate` is incorrect: Expected `type[ProtocolInterpolator] | None`, found `<class 'PchipInterpolator'>`
- Found 484 diagnostics
+ Found 480 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ examples/addons/contentview-interactive.py:20:18: error[invalid-argument-type] Argument to function `add` is incorrect: Expected `Contentview`, found `<class 'InteractiveSwapCase'>`
+ examples/addons/contentview.py:15:18: error[invalid-argument-type] Argument to function `add` is incorrect: Expected `Contentview`, found `<class 'SwapCase'>`
+ test/mitmproxy/contentviews/test__registry.py:67:23: error[invalid-argument-type] Argument to bound method `register` is incorrect: Expected `Contentview`, found `<class 'ExampleContentview'>`
- Found 1822 diagnostics
+ Found 1825 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_transformer.py:21:5: error[invalid-assignment] Object of type `<class 'Transformer'>` is not assignable to `type[Transformer]`
- psycopg/psycopg/pq/__init__.py:97:9: error[invalid-assignment] Object of type `Unknown | <class 'PGconn'>` is not assignable to `type[PGconn]`
- psycopg/psycopg/pq/__init__.py:98:9: error[invalid-assignment] Object of type `Unknown | <class 'PGresult'>` is not assignable to `type[PGresult]`
- psycopg/psycopg/pq/__init__.py:99:9: error[invalid-assignment] Object of type `Unknown | <class 'Conninfo'>` is not assignable to `type[Conninfo]`
- psycopg/psycopg/pq/__init__.py:100:9: error[invalid-assignment] Object of type `Unknown | <class 'Escaping'>` is not assignable to `type[Escaping]`
- psycopg/psycopg/pq/__init__.py:101:9: error[invalid-assignment] Object of type `Unknown | <class 'PGcancel'>` is not assignable to `type[PGcancel]`
- psycopg/psycopg/pq/__init__.py:102:9: error[invalid-assignment] Object of type `Unknown | <class 'PGcancelConn'>` is not assignable to `type[PGcancelConn]`
- psycopg/psycopg/types/array.py:342:12: error[invalid-return-type] Return type does not match returned value: expected `type[Loader]`, found `type`
- psycopg/psycopg/types/json.py:124:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[tuple[type, CodeType], type[Dumper]].__setitem__(key: tuple[type, CodeType], value: type[Dumper], /) -> None` cannot be called with a key of type `tuple[type, CodeType]` and a value of type `type` on object of type `dict[tuple[type, CodeType], type[Dumper]]`
- psycopg/psycopg/types/json.py:126:16: error[invalid-return-type] Return type does not match returned value: expected `type[Dumper]`, found `type`
- psycopg/psycopg/types/json.py:144:13: error[invalid-assignment] Method `__setitem__` of type `bound method dict[tuple[type, CodeType], type[Loader]].__setitem__(key: tuple[type, CodeType], value: type[Loader], /) -> None` cannot be called with a key of type `tuple[type, CodeType]` and a value of type `type` on object of type `dict[tuple[type, CodeType], type[Loader]]`
- psycopg/psycopg/types/json.py:146:16: error[invalid-return-type] Return type does not match returned value: expected `type[Loader]`, found `type`
- tests/fix_db.py:182:5: error[invalid-assignment] Object of type `<class 'PGconnDebug'>` is not assignable to attribute `PGconn` of type `type[PGconn]`
- Found 654 diagnostics
+ Found 641 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- tests/test_downloader_handler_twisted_http10.py:26:16: error[invalid-return-type] Return type does not match returned value: expected `type[DownloadHandlerProtocol]`, found `<class 'HTTP10DownloadHandler'>`
- tests/test_downloader_handler_twisted_http11.py:27:16: error[invalid-return-type] Return type does not match returned value: expected `type[DownloadHandlerProtocol]`, found `<class 'HTTP11DownloadHandler'>`
- tests/test_downloader_handler_twisted_http2.py:48:16: error[invalid-return-type] Return type does not match returned value: expected `type[DownloadHandlerProtocol]`, found `<class 'H2DownloadHandler'>`
- Found 1061 diagnostics
+ Found 1058 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/abc.py:689:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/app_commands/models.py:1225:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `Any | <class 'Member'> | <class 'Role'>`
- discord/audit_logs.py:140:67: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Role'>`
- discord/audit_logs.py:146:68: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'ForumTag'>`
- discord/audit_logs.py:147:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'ForumTag'>`
- discord/audit_logs.py:206:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Role'> | <class 'Member'>`
- discord/audit_logs.py:487:43: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Role'>`
- discord/audit_logs.py:803:55: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Role'>`
- discord/audit_logs.py:809:89: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'StageChannel'>`
- discord/audit_logs.py:813:88: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'PartialIntegration'>`
- discord/audit_logs.py:907:68: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Member'>`
- discord/audit_logs.py:910:71: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Role'>`
- discord/audit_logs.py:934:73: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Emoji'>`
- discord/audit_logs.py:943:68: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Member'>`
- discord/audit_logs.py:946:81: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'StageInstance'>`
- discord/audit_logs.py:949:75: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'GuildSticker'>`
- discord/audit_logs.py:952:73: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Thread'>`
- discord/audit_logs.py:955:82: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'ScheduledEvent'>`
- discord/audit_logs.py:958:70: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'PartialIntegration'>`
- discord/audit_logs.py:966:40: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'AppCommand'>`
- discord/audit_logs.py:986:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'PartialIntegration'> | <class 'AppCommand'>`
- discord/audit_logs.py:991:72: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'AutoModRule'>`
- discord/audit_logs.py:997:67: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'Webhook'>`
- discord/audit_logs.py:1000:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'OnboardingPrompt'>`
- discord/guild.py:4117:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'User'>`
- discord/guild.py:4118:45: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Snowflake]`, found `<class 'User'>`
- discord/utils.py:1458:5: error[invalid-assignment] Object of type `<class '_ZstdDecompressionContext'>` is not assignable to `type[_DecompressionContext]`
- discord/utils.py:1482:5: error[invalid-assignment] Object of type `<class '_ZlibDecompressionContext'>` is not assignable to `type[_DecompressionContext]`
- Found 567 diagnostics
+ Found 541 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/cli/work_pool.py:207:43: error[missing-argument] No argument provided for required parameter `self` of function `provision`
- src/prefect/cli/work_pool.py:208:42: error[invalid-argument-type] Argument to function `provision` is incorrect: Expected `dict[str, Any]`, found `dict[str, Any] | None | Any`
- src/prefect/cli/work_pool.py:499:43: error[missing-argument] No argument provided for required parameter `self` of function `provision`
- Found 2971 diagnostics
+ Found 2968 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @AlexWaygood on 2025-08-17 14:17_

That looks like roughly the mypy_primer report I'd expect here! Not totally sure what's up with the new hits on `mitmproxy`, but it looks very positive overall. Many diagnostics related to `dataclasses.fields()` are going away.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2122 on 2025-08-18 18:37_

The theory here is that this should disregard `x: int` on the protocol `Foo` because it describes an instance attribute? Should we mention that explicitly? It's not obvious that this should be the case, given that today we consider `x: int` to describe a class-or-instance attribute, in general.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/subclass_of.rs`:94 on 2025-08-18 18:41_

I think this deserves a TODO comment, because I don't think either the previous code nor this new code is really correct here. I think the inferrability should probably depend on some outer context?

What real-world cases triggered the need for this change?

---

_@carljm approved on 2025-08-18 18:43_

---

_@AlexWaygood reviewed on 2025-08-18 19:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subclass_of.rs`:94 on 2025-08-18 19:20_

What on earth... I didn't mean to make any change here to do with `TypeVar` materialization! I think this was a mistake introduced when resolving merge conflicts. It's a bit scary that no tests failed as a result of this -- cc. @dcreager; looks like we might be missing some coverage here?

Sorry for the error!! Thanks v much for the catch -- I'll change it back

---

_@AlexWaygood reviewed on 2025-08-18 19:20_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/subclass_of.rs`:94 on 2025-08-18 19:20_

```suggestion
                    Type::NonInferableTypeVar(BoundTypeVarInstance::new(
```

---

_@AlexWaygood reviewed on 2025-08-18 19:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2122 on 2025-08-18 19:24_

> The theory here is that this should disregard `x: int` on the protocol `Foo` because it describes an instance attribute?

Right. If we have a class `Spam` that looks like this:


```py
class Spam:
    def __init__(self):
        self.x: int = 42
```

then we would consider instances of class `Spam` to inhabit the type `Foo` as well as the type `Spam`, and `x` would obviously not be available on the class object `Spam` here (nor would we consider it available on the class object `Spam` here). It follows that therefore we cannot assume or enforce that `x` is available on the meta-type of any type `T` just because `T` is a subtype of `Foo`.

I'll add some more prose explaining this.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:2122 on 2025-08-18 20:31_

Ah, but on second look, there is a bug in the test here. `type[Baz]` should not be assignable to `type[Foo]` because it fails the third test I gave in my prose intro to this snippet: instantiating `Baz` would not create an object that would inhabit `Foo`.

Thanks for the catch!

---

_@AlexWaygood reviewed on 2025-08-18 20:31_

---

_Merged by @AlexWaygood on 2025-08-18 20:38_

---

_Closed by @AlexWaygood on 2025-08-18 20:38_

---

_Branch deleted on 2025-08-18 20:38_

---
