```yaml
number: 22577
title: "[ty] Build constraint set sequent maps lazily"
type: pull_request
state: open
author: dcreager
labels:
  - internal
  - ty
assignees: []
base: main
head: dcreager/lazy-sequent-map
created_at: 2026-01-14T18:04:45Z
updated_at: 2026-01-17T18:05:20Z
url: https://github.com/astral-sh/ruff/pull/22577
synced_at: 2026-01-17T18:19:15Z
```

# [ty] Build constraint set sequent maps lazily

---

_@dcreager_

Before, when building a `SequentMap` for a constraint set, we would immediately iterate through all of the constraints in the set, and compare each pair of them looking for intersection/implication relationships. It turns out that we often don't need to examine every pair when walking the BDD tree of a constraint set. Instead, we can visit each constraint as we encounter it for the first time in our BDD walk. We do still need to collect all of the constraints in the BDD to ensure that they remain ordered in a consistent way, but we can track that separately and without having to immediate build up the actual sequents.

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 18:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-14 18:07_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:94:28: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/integrations/prefect-dbt/prefect_dbt/core/settings.py:99:28: error[invalid-assignment] Object of type `T@resolve_variables | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:86:21: error[invalid-assignment] Object of type `T@resolve_block_document_references | dict[str, Any]` is not assignable to `dict[str, Any]`
- src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `int | T@resolve_variables | float | ... omitted 4 union elements` is not assignable to `dict[str, Any]`
+ src/prefect/cli/deploy/_core.py:87:21: error[invalid-assignment] Object of type `T@resolve_variables` is not assignable to `dict[str, Any]`
- src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | (((...) -> Any) & ((*args: object, **kwargs: object) -> object))`
+ src/prefect/deployments/runner.py:795:70: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | ((...) -> Any)`
- src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/deployments/steps/core.py:137:38: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
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
- src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements` on object of type `dict[str, Any]`
+ src/prefect/utilities/templating.py:320:13: error[invalid-assignment] Invalid subscript assignment with key of type `object` and value of type `T@resolve_block_document_references | dict[str, Any]` on object of type `dict[str, Any]`
- src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | int | dict[str, Any] | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:323:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_block_document_references | dict[str, Any]`, found `list[T@resolve_block_document_references | dict[str, Any] | Unknown]`
- src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:437:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `dict[object, T@resolve_variables | Unknown]`
- src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[int | T@resolve_variables | float | ... omitted 5 union elements]`
+ src/prefect/utilities/templating.py:442:16: error[invalid-return-type] Return type does not match returned value: expected `T@resolve_variables`, found `list[T@resolve_variables | Unknown]`
- src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | int | dict[str, Any] | ... omitted 4 union elements`
+ src/prefect/workers/base.py:232:13: error[invalid-argument-type] Argument is incorrect: Expected `T@resolve_variables`, found `T@resolve_block_document_references | dict[str, Any]`
- src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `int | T@resolve_variables | float | ... omitted 4 union elements`
+ src/prefect/workers/base.py:234:20: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `T@resolve_variables`
- Found 5406 diagnostics
+ Found 5411 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | Bottom[Series[Any, Any]] | ndarray[Never, Never] | ... omitted 6 union elements, object_]`
+ static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | IndexHierarchy | Bottom[Series[Any, Any]] | ... omitted 7 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 7 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | IndexHierarchy | TypeBlocks | ... omitted 7 union elements, TVDtype@Series]`
- static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | ndarray[Never, Never] | TypeBlocks | ... omitted 7 union elements, TVDtype@SeriesHE]`
+ static_frame/core/series.py:4072:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[SeriesHE[Any, Any], TVDtype@SeriesHE]`, found `InterGetItemILocReduces[Bottom[Series[Any, Any]] | IndexHierarchy | TypeBlocks | ... omitted 8 union elements, TVDtype@SeriesHE]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | ndarray[Never, Never] | TypeBlocks | ... omitted 6 union elements, object_]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | IndexHierarchy | TypeBlocks | ... omitted 7 union elements, object_]`


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~49MB
+     struct metadata = ~52MB


```

</details>




---

_Label `ty` added by @AlexWaygood on 2026-01-14 18:10_

---

_Label `internal` added by @dcreager on 2026-01-14 18:14_

---

_Comment by @codspeed-hq[bot] on 2026-01-14 18:26_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **improve performance by 4.98%**




`⚡ 1` improved benchmark  
`✅ 22` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flazy-sequent-map?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 8.2 s | 7.8 s | +4.98% |
---

<sub>Comparing <code>dcreager/lazy-sequent-map</code> (6555491) with <code>main</code> (3608c62)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flazy-sequent-map?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flazy-sequent-map?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_@MichaReiser reviewed on 2026-01-15 14:19_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:3304 on 2026-01-15 14:19_

I think this is the same as setting `[no_eq]` on the query (salsa will not do any backdating, meaning all queries reading the `sequent_map` of a particular interior node will re-run even if it creates the exact same `SeqMap`. Are there any other fields that we could base `Eq` on (e.g., the ones that don't change :)).

Looks very straightforward otherwise :)

---

_@MichaReiser reviewed on 2026-01-15 14:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:73 on 2026-01-15 14:22_

We might need to use a parkinglot `Mutex` or have a way to not get stuck after Salsa cancelled a query by unwinding with  `Cancelled` 

---

_Comment by @MichaReiser on 2026-01-15 14:29_

Cool to see that the internal mutability is good for performance :)

---

_@dcreager reviewed on 2026-01-15 14:40_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:3304 on 2026-01-15 14:40_

I did confirm that this is equivalent to setting `#[no_eq]` on the query method. And if I do that, I can remove the `PartialEq` impl entirely.

But does that mean we would get a separate `SequentMap` each time we call the tracked query? My intent is that there will be one created for each interior node. (And the updated performance numbers suggests that's what's happening.) I'm okay with a _different_ `SequentMap` being created for that interior node if it appears again in a later revision, since I think it's correct to invalidate that cache then.

Although maybe we do want to reuse the cache in later revisions? The interior node should entirely determine the contents of the BDD, and walking the BDD later on should yield the same results. Okay I think you've convinced me. (Assuming I understand your suggestion correctly.) To do this I can add the `InteriorNode` as a field of `SequentMap`, to record which node the sequent map belongs to, and then have that be the only field that `PartialEq` checks.

---

_@dcreager reviewed on 2026-01-15 15:01_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:73 on 2026-01-15 15:01_

Done

---

_Comment by @astral-sh-bot[bot] on 2026-01-15 15:07_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_@MichaReiser reviewed on 2026-01-15 15:09_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:3304 on 2026-01-15 15:09_

> But does that mean we would get a separate SequentMap each time we call the tracked query? My intent is that there will be one created for each interior node. (And the updated performance numbers suggests that's what's happening.) I'm okay with a different SequentMap being created for that interior node if it appears again in a later revision, since I think it's correct to invalidate that cache then.

No, you get the same instance within the same revision and the instance is cached for as long as no data read by the `::sequent_map()` query changes. 

Taking `exported_names` as an example here because it's easier to explain the concept. Salsa re-exeuctes the `exported_names` query every time the file's AST changes. When Salsa's done, it compares the result from running `exported_names` the last time with the newly computed result. If the two results are equal, then the query didn't change (even though the AST changed). This allows Salsa to reuse the cached result for a query that only depends `exported_names` (or only depends on queries that all haven't changed).

If you set `no_eq`, then you opt out of this optimization and Salsa will always assume that the result changed when any of the query's inputs changed. Which is probably fine in your case. 


The one thing we need to be careful is that the internal mutability code doesn't access `db` because a query reading a cached result wouldn't see all its dependencies, breaking Salsa's cache invalidation. 

---

_Marked ready for review by @dcreager on 2026-01-15 15:19_

---

_Review requested from @carljm by @dcreager on 2026-01-15 15:19_

---

_Review requested from @AlexWaygood by @dcreager on 2026-01-15 15:19_

---

_Review requested from @sharkdp by @dcreager on 2026-01-15 15:19_

---

_@dcreager reviewed on 2026-01-15 15:34_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:3304 on 2026-01-15 15:34_

> The one thing we need to be careful is that the internal mutability code doesn't access `db` because a query reading a cached result wouldn't see all its dependencies, breaking Salsa's cache invalidation.

Is this part true in general? That might be a deal-breaker for this approach, because the interior mutability code will definitely need to access the `db`.

---

_@MichaReiser reviewed on 2026-01-15 15:39_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:3304 on 2026-01-15 15:39_

> Is this part true in general? That might be a deal-breaker for this approach, because the interior mutability code will definitely need to access the db.

It depends on what you read, but how salsa tracks dependencies is something I'd consider internal to Salsa (or at least requires a lot of documentation)

I don't think we add read dependencies for interned structs, but we used to (CC: @ibraheemdev). But calling any salsa query, reading a tracked field of a tracked struct, or reading any input makes this approach unsound. 

---

_@MichaReiser reviewed on 2026-01-15 15:43_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:3304 on 2026-01-15 15:43_

Creating any new interned values I think would be unsound. 

I guess so is reading because reading an interned value with low durability **must** propagate to the outer query. So it's not just about the dependencies, it's also about the query's metadata that need to be reflected accordingly

---

_@dcreager reviewed on 2026-01-15 15:49_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:3304 on 2026-01-15 15:49_

Okay that tells me I need to rethink this...

---

_@ibraheemdev reviewed on 2026-01-15 21:14_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/constraints.rs`:3304 on 2026-01-15 21:14_

Yeah, interior mutability will not play well with Salsa here. If the interior mutability code creates an interned value without the `sequent_map` query having a dependency on that interned value, the interned value may be garbage collected, and later calls to `sequent_map` will read stale data.

---

_Review request for @carljm removed by @carljm on 2026-01-16 00:49_

---
