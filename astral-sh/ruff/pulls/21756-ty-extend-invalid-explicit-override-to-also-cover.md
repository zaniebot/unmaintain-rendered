```yaml
number: 21756
title: "[ty] Extend `invalid-explicit-override` to also cover properties decorated with `@override` that do not override anything"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/override-tests
created_at: 2025-12-02T16:15:29Z
updated_at: 2025-12-03T11:27:49Z
url: https://github.com/astral-sh/ruff/pull/21756
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Extend `invalid-explicit-override` to also cover properties decorated with `@override` that do not override anything

---

_Pull request opened by @AlexWaygood on 2025-12-02 16:15_

## Summary

Fixes some TODOs added in https://github.com/astral-sh/ruff/pull/21627

## Test Plan

Mdtests/snapshots


---

_Label `ty` added by @AlexWaygood on 2025-12-02 16:15_

---

_Comment by @astral-sh-bot[bot] on 2025-12-02 16:17_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-02 16:17:29.662991980 +0000
+++ new-output.txt	2025-12-02 16:17:33.242081570 +0000
@@ -218,6 +218,7 @@
 classes_override.py:65:9: error[invalid-explicit-override] Method `method4` is decorated with `@override` but does not override anything
 classes_override.py:79:9: error[invalid-explicit-override] Method `static_method1` is decorated with `@override` but does not override anything
 classes_override.py:84:9: error[invalid-explicit-override] Method `class_method1` is decorated with `@override` but does not override anything
+classes_override.py:89:9: error[invalid-explicit-override] Method `property1` is decorated with `@override` but does not override anything
 constructors_call_init.py:21:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `float`
 constructors_call_init.py:25:1: error[type-assertion-failure] Type `Class1[int | float]` does not match asserted type `Class1[float]`
 constructors_call_init.py:56:1: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Class4[int]`, found `Class4[str]`
@@ -1029,4 +1030,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1031 diagnostics
+Found 1032 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-12-02 16:19_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 496 diagnostics
+ Found 498 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @AlexWaygood on 2025-12-02 16:27_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-02 16:27_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-02 16:27_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-02 16:27_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/overrides.rs`:409 on 2025-12-03 06:26_

In case of a union where some members are decorated with `override` and others are not, should the diagnostic point to, say, the first one that is decorated with `override`, rather than just the first one, period -- which may not be decorated?

---

_@carljm approved on 2025-12-03 06:27_

Thanks!

---

_@AlexWaygood reviewed on 2025-12-03 11:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:409 on 2025-12-03 11:24_

hmm, yes, good point

---

_@AlexWaygood reviewed on 2025-12-03 11:27_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/overrides.rs`:409 on 2025-12-03 11:27_

I'll fix this in https://github.com/astral-sh/ruff/pull/21767, since that PR's the "but wait, what if there are multiple definitions??" PR

---

_Merged by @AlexWaygood on 2025-12-03 11:27_

---

_Closed by @AlexWaygood on 2025-12-03 11:27_

---

_Branch deleted on 2025-12-03 11:27_

---
