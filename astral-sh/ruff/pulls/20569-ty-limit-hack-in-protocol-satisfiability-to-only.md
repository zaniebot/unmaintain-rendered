```yaml
number: 20569
title: "[ty] Limit hack in protocol satisfiability to only apply to subtyping (not assignability)"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/proto-hack-reduce
created_at: 2025-09-25T10:35:09Z
updated_at: 2025-09-25T11:26:02Z
url: https://github.com/astral-sh/ruff/pull/20569
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Limit hack in protocol satisfiability to only apply to subtyping (not assignability)

---

_Pull request opened by @AlexWaygood on 2025-09-25 10:35_

## Summary

Following https://github.com/astral-sh/ruff/commit/742f8a4ee6d0730c7eaec081c80babdabc62a8c6, we now do sometimes consider `Type::TypeVar`s to be assignable to other types, and other types to be assignable to `Type::TypeVar`s. That means that this hack in our protocol satisfiability logic is needed _less_ than it used to be: all tests (including some new ones) continue to pass if we apply the hack _only_ when the `relation` passed is `TypeRelation::Subtyping`

## Test Plan

`cargo test -p ty_python_semantic`


---

_Label `internal` added by @AlexWaygood on 2025-09-25 10:35_

---

_Label `ty` added by @AlexWaygood on 2025-09-25 10:35_

---

_Comment by @github-actions[bot] on 2025-09-25 10:37_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-25 11:10:05.624991134 +0000
+++ new-output.txt	2025-09-25 11:10:08.788015435 +0000
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

_Comment by @github-actions[bot] on 2025-09-25 10:37_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
antidote (https://github.com/Finistere/antidote)
+ src/antidote/lib/interface_ext/_internal.py:132:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Predicate[Weight@create_conditions] | Weight@create_conditions | NeutralWeight | None | bool`, found `QualifiedBy`
+ src/antidote/lib/interface_ext/_internal.py:134:27: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Predicate[Weight@create_conditions] | Weight@create_conditions | NeutralWeight | None | bool`, found `QualifiedBy`
- Found 304 diagnostics
+ Found 306 diagnostics

jax (https://github.com/google/jax)
+ jax/experimental/pallas/ops/gpu/attention_mgpu.py:461:9: error[invalid-argument-type] Argument to function `emit_pipeline_warp_specialized` is incorrect: Expected `ComputeContext | None`, found `def _compute_thread(pipeline_callback) -> Unknown`
+ jax/experimental/pallas/ops/gpu/attention_mgpu.py:568:7: error[invalid-argument-type] Argument to function `emit_pipeline_warp_specialized` is incorrect: Expected `ComputeContext | None`, found `def _compute_thread(pipeline_callback) -> Unknown`
+ jax/experimental/pallas/ops/gpu/attention_mgpu.py:755:9: error[invalid-argument-type] Argument to function `emit_pipeline_warp_specialized` is incorrect: Expected `ComputeContext | None`, found `def _compute_thread(pipeline_callback) -> Unknown`
- Found 2251 diagnostics
+ Found 2254 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-25 10:46_

---

_Comment by @github-actions[bot] on 2025-09-25 10:51_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 5 | 0 | 0 |
| `no-matching-overload` | 0 | 1 | 0 |
| **Total** | **5** | **1** | **0** |

**[Full report with detailed diff](https://alex-proto-hack-reduce.ecosystem-663.pages.dev/diff)**


---

_Comment by @sharkdp on 2025-09-25 11:01_

It took me quite a bit of time (and your help) to understand what was going on on the type-of-self PR, so I would be in favor of a solution that *completely* removes this hack (https://github.com/astral-sh/ruff/pull/20568). The ecosystem impact seems to be the same.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-09-25 11:02_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-25 11:02_

---

_Comment by @AlexWaygood on 2025-09-25 11:14_

> It took me quite a bit of time (and your help) to understand what was going on on the type-of-self PR, so I would be in favor of a solution that _completely_ removes this hack (#20568). The ecosystem impact seems to be the same.

I'm okay with that. Would you mind copying over the test additions from this version? (Explicit tests that assignability is not affected by the removal of the hack (only subtyping), and a couple of new regression tests for the fixed issue in the typing conformance suite)

---

_Closed by @AlexWaygood on 2025-09-25 11:19_

---

_Branch deleted on 2025-09-25 11:19_

---
