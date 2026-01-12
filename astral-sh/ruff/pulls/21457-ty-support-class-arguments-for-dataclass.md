```yaml
number: 21457
title: "[ty] Support class-arguments for dataclass transformers"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/dataclass-transformer-class-args
created_at: 2025-11-14T15:18:24Z
updated_at: 2025-11-15T16:47:50Z
url: https://github.com/astral-sh/ruff/pull/21457
synced_at: 2026-01-12T15:57:25Z
```

# [ty] Support class-arguments for dataclass transformers

---

_@sharkdp_

## Summary

Allow metaclass-based and baseclass-based dataclass-transformers to overwrite the default behavior using class arguments:

```py
class Person(Model, order=True):
    # ...
```

## Conformance tests

Four new tests passing!

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-11-14 15:18_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-14 15:18_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 15:20_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-15 11:08:04.194581567 +0000
+++ new-output.txt	2025-11-15 11:08:07.641576022 +0000
@@ -273,10 +273,10 @@
 dataclasses_postinit.py:29:7: error[unresolved-attribute] Object of type `DC1` has no attribute `y`
 dataclasses_slots.py:66:1: error[unresolved-attribute] Class `DC6` has no attribute `__slots__`
 dataclasses_slots.py:69:1: error[unresolved-attribute] Object of type `DC6` has no attribute `__slots__`
+dataclasses_transform_class.py:63:1: error[invalid-assignment] Property `id` defined in `Customer1` is read-only
 dataclasses_transform_class.py:66:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_class.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_class.py:72:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
-dataclasses_transform_class.py:78:6: error[unsupported-operator] Operator `<` is not supported for types `Customer2` and `Customer2`
 dataclasses_transform_class.py:82:8: error[missing-argument] No argument provided for required parameter `id`
 dataclasses_transform_class.py:82:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_class.py:122:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
@@ -324,10 +324,10 @@
 dataclasses_transform_func.py:70:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_func.py:76:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `T@create_model_frozen`
 dataclasses_transform_func.py:96:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only
+dataclasses_transform_meta.py:63:1: error[invalid-assignment] Property `id` defined in `Customer1` is read-only
 dataclasses_transform_meta.py:66:8: error[missing-argument] No arguments provided for required parameters `id`, `name`
 dataclasses_transform_meta.py:66:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_meta.py:73:6: error[unsupported-operator] Operator `<` is not supported for types `Customer1` and `Customer1`
-dataclasses_transform_meta.py:79:6: error[unsupported-operator] Operator `<` is not supported for types `Customer2` and `Customer2`
 dataclasses_transform_meta.py:83:8: error[missing-argument] No argument provided for required parameter `id`
 dataclasses_transform_meta.py:83:18: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 2
 dataclasses_transform_meta.py:103:1: error[invalid-assignment] Property `id` defined in `Customer3` is read-only

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-14 15:21_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 15:25_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-dataclass-transformer.ecosystem-663.pages.dev/diff)** ([timing results](https://david-dataclass-transformer.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-15 11:08_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-15 11:08_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:421 on 2025-11-15 11:16_

The reason why this doesn't work is a bit tricky. For frozen models, we generate a `__setattr__` method that returns `Never`. This allows us to emit the `invalid-assignment` diagnostic. We set `frozen=False` on `Mutable`, and with this PR, this means that we do not generate a `__setattr__` method on `Mutable` *directly* anymore. But the implicit `__setattr__` call in the assignment to `m.name` below calls also looks up `__setattr__` on the meta type of `Mutable`, i.e. `DefaultFrozenMeta`. And since `DefaultFrozenMeta` has `frozen_default=True`, we *do* generate a `__setattr__` method on the meta class. This method is what causes the right behavior for the `Frozen` model. But it turns out this is not what actually happens at runtime. https://typing.python.org/en/latest/spec/dataclasses.html#dataclass-semantics describes the precise semantics for frozen dataclasses transformers. `DefaultFrozenMeta` is actually neither frozen nor non-frozen. The `__setattr__` method should instead only be generated on `Frozen` directly.

---

_@sharkdp reviewed on 2025-11-15 11:16_

---

_Comment by @sharkdp on 2025-11-15 11:19_

> ```diff
> - src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
> ```

Is this another flaky ecosystem diff? :confused: I can not reproduce this locally, neither can ecosystem-analyzer.

---

_Marked ready for review by @sharkdp on 2025-11-15 11:19_

---

_Review requested from @carljm by @sharkdp on 2025-11-15 11:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-15 11:19_

---

_Review requested from @dcreager by @sharkdp on 2025-11-15 11:19_

---

_Comment by @MichaReiser on 2025-11-15 12:05_

> > ```diff
> 
> > - src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
> 
> > ```
> 
> 
> 
> Is this another flaky ecosystem diff? :confused: I can not reproduce this locally, neither can ecosystem-analyzer.

Yes it is. It shows up on almost every PR

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:421 on 2025-11-15 16:22_

Should we file an issue for this?

---

_@carljm approved on 2025-11-15 16:30_

Nice!

---

_@sharkdp reviewed on 2025-11-15 16:47_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/dataclasses/dataclass_transform.md`:421 on 2025-11-15 16:47_

Added to https://github.com/astral-sh/ty/issues/1327

---

_Merged by @sharkdp on 2025-11-15 16:47_

---

_Closed by @sharkdp on 2025-11-15 16:47_

---

_Branch deleted on 2025-11-15 16:47_

---
