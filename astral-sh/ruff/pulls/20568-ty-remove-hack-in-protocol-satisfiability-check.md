```yaml
number: 20568
title: "[ty] Remove hack in protocol satisfiability check"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/remove-protocol-satisfiability-hack
created_at: 2025-09-25T10:09:00Z
updated_at: 2025-09-25T11:35:48Z
url: https://github.com/astral-sh/ruff/pull/20568
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Remove hack in protocol satisfiability check

---

_Pull request opened by @sharkdp on 2025-09-25 10:09_

## Summary

This removes a hack in the protocol satisfiability check that was previously needed to work around missing assignability-modeling of inferable type variables. Assignability of type variables is not implemented fully, but some recent changes allow us to remove that hack with limited impact on the ecosystem (and the test suite). The change in the typing conformance test is favorable.

## Test Plan

* Adapted Markdown tests
* Made sure that this change works in combination with https://github.com/astral-sh/ruff/pull/20517

---

_Label `ty` added by @sharkdp on 2025-09-25 10:09_

---

_Comment by @github-actions[bot] on 2025-09-25 10:11_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-25 11:25:37.323644940 +0000
+++ new-output.txt	2025-09-25 11:25:40.505639553 +0000
@@ -430,6 +430,7 @@
 generics_self_basic.py:75:5: error[type-assertion-failure] Argument does not have asserted type `Container[str]`
 generics_self_basic.py:83:5: error[type-assertion-failure] Argument does not have asserted type `Container[T@object_with_generic_type]`
 generics_self_protocols.py:48:5: error[type-assertion-failure] Argument does not have asserted type `ShapeProtocol`
+generics_self_protocols.py:61:19: error[invalid-argument-type] Argument to function `accepts_shape` is incorrect: Expected `ShapeProtocol`, found `BadReturnType`
 generics_self_usage.py:73:14: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:73:23: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
 generics_self_usage.py:76:6: error[invalid-type-form] Variable of type `typing.Self` is not allowed in a type expression
@@ -860,5 +861,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 861 diagnostics
+Found 862 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-25 10:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
antidote (https://github.com/Finistere/antidote)
+ src/antidote/lib/interface_ext/_internal.py:132:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Predicate[Weight@create_conditions] | Weight@create_conditions | NeutralWeight | None | bool`, found `QualifiedBy`
+ src/antidote/lib/interface_ext/_internal.py:134:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Predicate[Weight@create_conditions] | Weight@create_conditions | NeutralWeight | None | bool`, found `QualifiedBy`
- tests/lib/interface/test_conditions.py:193:45: error[invalid-argument-type] Argument to bound method `when` is incorrect: Expected `Predicate[Weight | None] | Weight | None | NeutralWeight | bool`, found `OnlyIf`
- Found 304 diagnostics
+ Found 305 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 53 diagnostics

jax (https://github.com/google/jax)
+ jax/experimental/pallas/ops/gpu/attention_mgpu.py:461:9: error[invalid-argument-type] Argument to function `emit_pipeline_warp_specialized` is incorrect: Expected `ComputeContext | None`, found `def _compute_thread(pipeline_callback) -> Unknown`
+ jax/experimental/pallas/ops/gpu/attention_mgpu.py:568:7: error[invalid-argument-type] Argument to function `emit_pipeline_warp_specialized` is incorrect: Expected `ComputeContext | None`, found `def _compute_thread(pipeline_callback) -> Unknown`
+ jax/experimental/pallas/ops/gpu/attention_mgpu.py:755:9: error[invalid-argument-type] Argument to function `emit_pipeline_warp_specialized` is incorrect: Expected `ComputeContext | None`, found `def _compute_thread(pipeline_callback) -> Unknown`
- Found 2251 diagnostics
+ Found 2254 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo metadata = ~6MB
+     memo metadata = ~5MB

```
</details>


---

_Marked ready for review by @sharkdp on 2025-09-25 10:58_

---

_Review requested from @carljm by @sharkdp on 2025-09-25 10:58_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-09-25 10:58_

---

_Review requested from @dcreager by @sharkdp on 2025-09-25 10:58_

---

_@sharkdp reviewed on 2025-09-25 10:59_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1904 on 2025-09-25 10:59_

I can remove these `Self` annotations again in the type-of-self PR.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/protocol_class.rs`:573 on 2025-09-25 11:21_

this variable is now only used once, so it probably doesn't need to even be assigned; you can just inline it

---

_@AlexWaygood approved on 2025-09-25 11:21_

LGTM, but I'd love to add some of the tests I added in #20569 (see https://github.com/astral-sh/ruff/pull/20569#issuecomment-3333459036)

---

_Merged by @sharkdp on 2025-09-25 11:35_

---

_Closed by @sharkdp on 2025-09-25 11:35_

---

_Branch deleted on 2025-09-25 11:35_

---
