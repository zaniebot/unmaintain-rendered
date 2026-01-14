```yaml
number: 22089
title: "[ty] Avoid false positive for `not-iterable` with no-positive intersection types"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: main
head: charlie/neg
created_at: 2025-12-19T16:29:04Z
updated_at: 2026-01-14T02:09:54Z
url: https://github.com/astral-sh/ruff/pull/22089
synced_at: 2026-01-14T02:32:45Z
```

# [ty] Avoid false positive for `not-iterable` with no-positive intersection types

---

_@charliermarsh_

## Summary

When checking if `list[~str]` is iterable, we end up looking for a positive element to satisfy assignability to `Iterable[~str]`. But when there are no positive elements, we return `false`, rather than falling back to `object`.

Closes https://github.com/astral-sh/ty/issues/1880.


---

_Renamed from "Avoid false positive for `not-iterable` with no-positive intersection types" to "[ty] Avoid false positive for `not-iterable` with no-positive intersection types" by @charliermarsh on 2025-12-19 16:29_

---

_Label `ty` added by @charliermarsh on 2025-12-19 16:29_

---

_Comment by @astral-sh-bot[bot] on 2025-12-19 16:30_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-19 16:32_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
- tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]] | _T@cached_inject`
+ tanjun/dependencies/data.py:347:12: error[invalid-return-type] Return type does not match returned value: expected `_T@cached_inject`, found `_T@cached_inject | Coroutine[Any, Any, _T@cached_inject | Coroutine[Any, Any, _T@cached_inject]]`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/core/lending.py:811:22: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown | str, Unknown | str | ~AlwaysTruthy | dict[Unknown | str, Unknown] | int]`
- Found 1146 diagnostics
+ Found 1145 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/permission.py:163:35: error[not-iterable] Object of type `list[~AlwaysFalsy | Unknown]` is not iterable
- Found 348 diagnostics
+ Found 347 diagnostics

altair (https://github.com/vega/altair)
+ altair/vegalite/v6/api.py:263:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SchemaBase | Mapping[str, Any] | UndefinedType`, found `object`
- altair/vegalite/v6/api.py:263:42: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `dict[Unknown, Unknown] | dict[Unknown | str, Unknown] | dict[~Literal["values"] | Unknown, object]`
+ altair/vegalite/v6/api.py:263:42: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `object`
- Found 1060 diagnostics
+ Found 1061 diagnostics

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
- Found 5370 diagnostics
+ Found 5365 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`

core (https://github.com/home-assistant/core)
- homeassistant/components/bluetooth/passive_update_coordinator.py:78:20: error[not-iterable] Object of type `GeneratorType[~None, None, None]` is not iterable
- homeassistant/helpers/update_coordinator.py:217:20: error[not-iterable] Object of type `GeneratorType[~None, None, None]` is not iterable
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14505 diagnostics
+ Found 14502 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @carljm by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @AlexWaygood by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @sharkdp by @charliermarsh on 2025-12-19 16:35_

---

_Review requested from @dcreager by @charliermarsh on 2025-12-19 16:35_

---

_Comment by @codspeed-hq[bot] on 2025-12-19 16:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **degrade performance by 28.9%**




`❌ 5` regressed benchmarks  
`✅ 18` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?utm_source=github&utm_medium=comment-v2&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 89.5 s | 125.9 s | -28.9% |
| ❌ | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 64.6 s | 70.7 s | -8.62% |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 21.8 s | 23.6 s | -7.4% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 8 s | 8.5 s | -5.81% |
| ❌ | Simulation | [`` ty_check_file[incremental] ``](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Acheck_file%3A%3Abenchmark_incremental%3A%3Aty_check_file%5Bincremental%5D&runnerMode=Instrumentation&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 6.2 ms | 6.5 ms | -5.41% |
---

<sub>Comparing <code>charlie/neg</code> (70859e4) with <code>main</code> (b5814b9)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/charlie%2Fneg?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Comment by @charliermarsh on 2025-12-19 17:15_

Ok, wasn't expecting that!

---

_Comment by @AlexWaygood on 2025-12-19 18:06_

This does look correct, but we obviously can't merge this if it causes the benchmarks to start timing out 🙃

I think we probably need to do similar for the `when_any` calls in the `Type::Intersection` branch above this one, too

---

_Comment by @charliermarsh on 2025-12-19 18:34_

(Looking into it...)

---

_Converted to draft by @charliermarsh on 2025-12-19 18:34_

---

_@carljm reviewed on 2025-12-19 18:56_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 18:56_

I expect this fallback to object is the source of the regression, and I'm surprised that we would need it here. If we are going to treat an intersection type with no positive elements as equivalent to `object`, then the only thing it can be a subtype of is `object` (and the only things it can be assignable to are `Any/Unknown` and `object`), and those cases should already be handled above. What was failing without this change? That is, in what form does the `list[~str]` iteration case reach here?

---

_@AlexWaygood reviewed on 2025-12-19 18:59_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 18:59_

`~str` can also be a subtype of a union, like `~str | int` -- but I guess that's also handled by a higher-up branch.

---

_@AlexWaygood reviewed on 2025-12-19 19:02_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 19:02_

oh, and `~str & ~int` is a subtype of both `~str` and `~str & ~int`, right?

---

_@carljm reviewed on 2025-12-19 19:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 19:06_

Yes, seems like we could add consideration of cases like that to get more precise results here, but that seems like a separate issue. We don't consider negative types in the intersection either before or after the current change in this PR. The only change here is to explicitly test `object`, which seems to me like it shouldn't be necessary (but clearly I'm missing something, if it makes the test in this PR pass.)

---

_@AlexWaygood reviewed on 2025-12-19 19:08_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-19 19:08_

I was just answering the immediate question, not trying to say that these cases were relevant. I think they're handled by the branch immediately above this one, anyway

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:2500 on 2025-12-20 02:10_

I am of course not an expert here but my understanding is that when we `try_iterate` over `list[~str]`, we eventually check `list.__iter__` which is `def __iter__(self) -> Iterator[_T]`, and then need to infer specialization to determine that `_T` is `~str`.

We then end up in `has_relation_to_impl` and in this big match on `(self, target)`, `self` is an intersection (`~str`) and `target` is a type var (`_T`), which don't match any of the above cases... So we land in this case, and at present, `intersection.positive(db).iter()` is empty, so we return false.

---

_@charliermarsh reviewed on 2025-12-20 02:10_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:2500 on 2026-01-14 00:53_

That makes sense. Sorry it took me a while to get back to this!

After thinking it over, I do feel like the change made here is correct and necessary. But the combination of benchmark and ecosystem results suggests that most of the time we still end up finding that `object` is not assignable to `target` (that is, in most cases this PR doesn't change our eventual behavior) -- but we now spend a fair amount of extra time reaching that conclusion.

Still, it would be great if we could bring down the perf impact more here. I feel like the most promising way to do that is probably more of what you already did? That is, find the next-most-common case (via profiling) where we spend a lot of time concluding that `object` is not assignable to something, and see if we can add a short-circuit test for that.

---

_@carljm reviewed on 2026-01-14 00:53_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types.rs`:2500 on 2026-01-14 01:15_

On it.

---

_@charliermarsh reviewed on 2026-01-14 01:15_

---

_@carljm reviewed on 2026-01-14 01:38_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/relation.rs`:872 on 2026-01-14 01:38_

Wait, this commit removed the `.positive_elements_or_object` here? So it will show full recovery of the regression, but should fail the added test?

---

_@charliermarsh reviewed on 2026-01-14 01:43_

---

_Review comment by @charliermarsh on `crates/ty_python_semantic/src/types/relation.rs`:872 on 2026-01-14 01:43_

The test is passing, at least for me locally. (I haven't dug in at all; I was just throwing things at the wall for now to see if there was an easy fix.)

---

_@carljm reviewed on 2026-01-14 01:46_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/relation.rs`:872 on 2026-01-14 01:46_

Oh my bad, I missed the extra special case that was added above for negative-only intersections.

I think throwing some fast-paths at the wall to see what helps is a great approach! I'll leave you alone now :)

---
