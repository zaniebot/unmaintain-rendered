```yaml
number: 22402
title: "[ty] Optimize `Type::negate()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/optimize-negate-2
created_at: 2026-01-05T15:31:07Z
updated_at: 2026-01-06T19:18:01Z
url: https://github.com/astral-sh/ruff/pull/22402
synced_at: 2026-01-12T15:57:49Z
```

# [ty] Optimize `Type::negate()`

---

_@AlexWaygood_

## Summary

`Type::negate()` is a hot function that is called in many places across the codebase. Currently we always compute a type's negation by invoking the `IntersectionBuilder`, adding a single negative element, and calling `.build()` to create a new `Type`. But this is unnecessary overhead for many trivial cases where we know what the negation of a type is (and always will be). In these cases, we can create the negation directly instead of invoking the `IntersectionBuilder`.

This PR leads to speedups of 1-4% across a wide range of benchmarks according to codspeed: https://codspeed.io/astral-sh/ruff/branches/alex%2Foptimize-negate-2?utm_source=github&utm_medium=check&utm_content=details

## Test Plan

- Existing tests all pass
- `QUICKCHECK_TESTS=1000000 cargo test --profile=profiling -p ty_python_semantic -- --ignored types::property_tests::stable` passes
- The primer report is clean except for our standard set of flakes


---

_Label `performance` added by @AlexWaygood on 2026-01-05 15:31_

---

_Label `ty` added by @AlexWaygood on 2026-01-05 15:31_

---

_Label `performance` added by @AlexWaygood on 2026-01-05 15:31_

---

_Label `ty` added by @AlexWaygood on 2026-01-05 15:31_

---

_Comment by @astral-sh-bot[bot] on 2026-01-05 15:33_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-05 15:34_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
+ src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
+ src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
+ src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
- src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
- Found 5528 diagnostics
+ Found 5533 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 48 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Bus[Any]] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Top[Index[Any]] | Top[Series[Any, Any]] | TypeBlocks | ... omitted 6 union elements, object_]`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ pandas-stubs/_typing.pyi:1232:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5155 diagnostics
+ Found 5156 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@AlexWaygood reviewed on 2026-01-05 16:07_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1500 on 2026-01-05 16:07_

maybe it's a _bit_ risky to skip the `IntersectionBuilder` for some of the structural types, e.g. protocols and typeddicts? Is it plausible that we'd ever eagerly normalize `~TD` or `~P` to something that's not represented as an intersection with a single negative element...?

Not sure, but I'd be happy to move `Type::ProtocolInstance(_)` and/or `Type::TypedDict(_)` to this branch if we think it's risky

---

_Marked ready for review by @AlexWaygood on 2026-01-05 16:08_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-05 16:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-05 16:08_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-05 16:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1500 on 2026-01-06 15:42_

I would defer to you on whether it's risky — though if you're asking the question, that might be the answer? If being safer for those two variants doesn't affect the performance numbers too much, then I would lean towards the safer option.

(Or maybe add a property test that computing `~T` "by hand" via an `IntersectionBuilder` is always equivalent to what you get from `Type::negate`, and rely on that to tell us if there's a problem with protocols or `TypedDict`?)

---

_@dcreager approved on 2026-01-06 15:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1500 on 2026-01-06 18:47_

The property-test idea is great, thanks! I'll go that route.

---

_@AlexWaygood reviewed on 2026-01-06 18:47_

---

_Merged by @AlexWaygood on 2026-01-06 19:18_

---

_Closed by @AlexWaygood on 2026-01-06 19:18_

---

_Branch deleted on 2026-01-06 19:18_

---
