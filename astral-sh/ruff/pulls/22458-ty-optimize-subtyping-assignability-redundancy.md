```yaml
number: 22458
title: "[ty] Optimize subtyping/assignability/redundancy checks against union types"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - performance
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: alex/union-set
created_at: 2026-01-08T14:12:30Z
updated_at: 2026-01-16T23:48:59Z
url: https://github.com/astral-sh/ruff/pull/22458
synced_at: 2026-01-17T00:08:11Z
```

# [ty] Optimize subtyping/assignability/redundancy checks against union types

---

_@AlexWaygood_

## Summary

If we store the elements list in a `UnionType` as an `FxOrderSet` rather than a `Vec`, we can add an `O(1)` fast path for `T :< U` where `T` is trivially contained in the elements of `U`. This provides a huge speedup on code that makes use of deeply recursive union such as pydantic (and all code that uses pydantic!).

## Test Plan

Existing tests all pass.


---

_Label `performance` added by @AlexWaygood on 2026-01-08 14:12_

---

_Label `ty` added by @AlexWaygood on 2026-01-08 14:12_

---

_Label `performance` added by @AlexWaygood on 2026-01-08 14:12_

---

_Label `ty` added by @AlexWaygood on 2026-01-08 14:12_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 14:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-08 14:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables | str | int | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/flow_engine.py:812:32: error[invalid-await] `Unknown | R@FlowRunEngine | Coroutine[Any, Any, R@FlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1401:24: error[invalid-await] `Unknown | R@AsyncFlowRunEngine | Coroutine[Any, Any, R@AsyncFlowRunEngine]` is not awaitable
- src/prefect/flow_engine.py:1482:43: error[invalid-argument-type] Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1490:21: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_sync`
- src/prefect/flow_engine.py:1524:44: warning[possibly-missing-attribute] Attribute `__anext__` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flow_engine.py:1531:25: warning[possibly-missing-attribute] Attribute `throw` may be missing on object of type `Unknown | R@run_generator_flow_async`
- src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:286:34: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
- src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `(**P@Flow) -> R@Flow` has no attribute `__name__`
+ src/prefect/flows.py:404:68: error[unresolved-attribute] Object of type `((**P@Flow) -> R@Flow) & ((*args: object, **kwargs: object) -> object)` has no attribute `__name__`
+ src/prefect/flows.py:1750:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | str | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | str | int | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any] | str | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables | str | int | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5411 diagnostics
+ Found 5406 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1823 diagnostics
+ Found 1825 diagnostics

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14490 diagnostics
+ Found 14491 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2026-01-08 14:25_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-set?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **not alter performance**

<sub>Comparing <code>alex/union-set</code> (7256a1e) with <code>main</code> (717d024)</sub>



### Summary

`✅ 23` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  




[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Funion-set?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2026-01-08 14:39_

---

_Comment by @astral-sh-bot[bot] on 2026-01-08 14:44_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-parameter-default` | 0 | 0 | 7 |
| `invalid-argument-type` | 0 | 1 | 5 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-return-type` | 1 | 0 | 3 |
| `possibly-missing-attribute` | 0 | 3 | 1 |
| `invalid-await` | 0 | 2 | 0 |
| `unresolved-attribute` | 0 | 0 | 2 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **2** | **6** | **23** |


**[Full report with detailed diff](https://7441b4ad.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://7441b4ad.ty-ecosystem-ext.pages.dev/timing))



---

_Renamed from "[ty] Optimize assignability checks against union types" to "[ty] Optimize subtyping/assignability/redundancy checks against union types" by @AlexWaygood on 2026-01-10 14:10_

---

_@MichaReiser reviewed on 2026-01-10 14:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:11722 on 2026-01-10 14:13_

Could we store the `IndexMap` `Slice` type here instead of a full order set?

It might help mitigate the memory regression

---

_@AlexWaygood reviewed on 2026-01-10 14:15_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11722 on 2026-01-10 14:15_

before optimising memory usage, I first need to understand why there's a bunch of diagnostics unexpectedly going away on this PR :P

There's either a bug on `main` that's unexpectedly been fixed here, or there's a bug in this PR...

---

_@AlexWaygood reviewed on 2026-01-10 14:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11722 on 2026-01-10 14:17_

> Could we store the `IndexMap` `Slice` type here instead of a full order set?

If you expect that to be just as fast, should we do that for `IntersectionType`? We store a full order set for `IntersectionType::positive` and in the `NegativeIntersectionElements::multiple` variant

---

_@MichaReiser reviewed on 2026-01-10 14:20_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:11722 on 2026-01-10 14:20_

I'm not aware of a reason why https://docs.rs/indexmap/latest/indexmap/map/struct.IndexMap.html#method.into_boxed_slice should be slower (other than that it requires removing the excess capacity)

---

_@AlexWaygood reviewed on 2026-01-10 14:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:11722 on 2026-01-10 14:24_

The whole point of the optimisation in this PR is the fast paths being added in `Type::has_relation_to_impl`, where we make use of the fact that `FxOrderSet::contains()` is `O(1)`. But `ordermap::set::Slice` does not have a `contains()` method, let alone an `O(1)` `contains()` method. So I don't think your suggestion here works.

---

_@MichaReiser reviewed on 2026-01-10 14:41_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:11722 on 2026-01-10 14:41_

Huh, that's surprising, because the map has [`contains_key`](https://docs.rs/indexmap/latest/indexmap/map/struct.IndexMap.html#method.contains_key)

---

_Comment by @AlexWaygood on 2026-01-10 14:58_

With Claude's help, I minimized the behaviour change on this PR to:

```py
from typing import Callable, overload

class Box[T]:
    def get(self) -> T:
        raise NotImplementedError

@overload
def my_iter[T](f: Callable[[], T | None], sentinel: None) -> Box[T]: ...
@overload
def my_iter[T](f: Callable[[], T]) -> Box[T]: ...
def my_iter(f: Callable[[], object], sentinel: object = ...) -> Box[object]:
    return Box()

def get_int() -> int | None: ...

# reveals `Box[int | None]` on `main`, `Box[int]` on this branch
reveal_type(my_iter(get_int, None))
```

The behaviour on this branch seems like an improvement, so it seems like there's a pre-existing bug on `main` that this PR accidentally fixes.

---

_Comment by @AlexWaygood on 2026-01-16 17:51_

> With Claude's help, I minimized the behaviour change on this PR to:
> 
> ```python
> from typing import Callable, overload
> 
> class Box[T]:
>     def get(self) -> T:
>         raise NotImplementedError
> 
> @overload
> def my_iter[T](f: Callable[[], T | None], sentinel: None) -> Box[T]: ...
> @overload
> def my_iter[T](f: Callable[[], T]) -> Box[T]: ...
> def my_iter(f: Callable[[], object], sentinel: object = ...) -> Box[object]:
>     return Box()
> 
> def get_int() -> int | None: ...
> 
> # reveals `Box[int | None]` on `main`, `Box[int]` on this branch
> reveal_type(my_iter(get_int, None))
> ```
> 
> The behaviour on this branch seems like an improvement, so it seems like there's a pre-existing bug on `main` that this PR accidentally fixes.

Even smaller:

```py
from typing import Callable

class Box[T]:
    def get(self) -> T:
        raise NotImplementedError

def my_iter[T](f: Callable[[], T | None]) -> Box[T]:
    return Box()

def get_int() -> int | None: ...

# reveals `Box[int | None]` on `main`, `Box[int]` on this branch
reveal_type(my_iter(get_int))
```

---

_Comment by @AlexWaygood on 2026-01-16 17:54_

Planning on opening https://github.com/astral-sh/ruff/pull/22495 for review first. If that lands, I'll rebase this on top of `main` to see if switching to a set instead of a vec still provides any meaningful speedup (and then we can debate whether it's worth the memory-usage regression, if so).

---

_Closed by @AlexWaygood on 2026-01-16 23:48_

---

_Branch deleted on 2026-01-16 23:48_

---
