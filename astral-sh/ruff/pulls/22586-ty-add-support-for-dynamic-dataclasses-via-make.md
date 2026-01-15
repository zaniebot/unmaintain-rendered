```yaml
number: 22586
title: "[ty] Add support for dynamic dataclasses via `make_dataclass`"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
base: main
head: charlie/functional-dict
created_at: 2026-01-14T22:20:58Z
updated_at: 2026-01-15T03:26:36Z
url: https://github.com/astral-sh/ruff/pull/22586
synced_at: 2026-01-15T03:52:12Z
```

# [ty] Add support for dynamic dataclasses via `make_dataclass`

---

_@charliermarsh_

## Summary

Like #22327, but for dataclasses.


---

_Label `ty` added by @charliermarsh on 2026-01-14 22:21_

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 22:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->



## Typing Conformance

### Summary

| Metric | Old | New | Diff | Outcome |
|--------|-----|-----|------|---------|
| True Positives  | 0 | 0 | +0 | ⏬ (❌) |
| False Positives | 969 | 969 | +0 | ⏬ (✅) |
| False Negatives | 1155 | 1155 | +0 | ⏬ (✅) |
| Total Diagnostics | 969 | 969 | 0 | ⏬ |
| Precision | 0.00% | 0.00% | +0.00% | ⏬ (❌) |
| Recall | 0.00% | 0.00% | +0.00% | ⏬ (❌) |


The percentage of diagnostics emitted that were expected errors held steady at 0.00%, and the percentage of expected errors that received a diagnostic held steady at 0.00%.



[Typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)



---

_Comment by @astral-sh-bot[bot] on 2026-01-14 22:23_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/experimental/pydantic/error_type.py:149:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ strawberry/experimental/pydantic/object_type.py:255:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 348 diagnostics
+ Found 350 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
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
- Found 5403 diagnostics
+ Found 5408 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1823 diagnostics
+ Found 1824 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- tests/frame/test_groupby.py:229:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- tests/frame/test_groupby.py:625:15: error[type-assertion-failure] Type `Series[Any]` does not match asserted type `Series[str | bytes | int | ... omitted 12 union elements]`
- Found 4361 diagnostics
+ Found 4359 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @carljm by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @AlexWaygood by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @sharkdp by @charliermarsh on 2026-01-15 02:58_

---

_Review requested from @dcreager by @charliermarsh on 2026-01-15 02:58_

---

_Converted to draft by @charliermarsh on 2026-01-15 02:59_

---

_Marked ready for review by @charliermarsh on 2026-01-15 03:22_

---
