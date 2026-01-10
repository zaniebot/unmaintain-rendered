```yaml
number: 22337
title: "[ty] Improve `UnionBuilder` performance by changing `Type::is_subtype_of` calls to `Type::is_redundant_with`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: alex/more-redundancy
created_at: 2026-01-02T16:19:20Z
updated_at: 2026-01-07T22:17:47Z
url: https://github.com/astral-sh/ruff/pull/22337
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Improve `UnionBuilder` performance by changing `Type::is_subtype_of` calls to `Type::is_redundant_with`

---

_Pull request opened by @AlexWaygood on 2026-01-02 16:19_

## Summary

This PR is stacked on top of https://github.com/astral-sh/ruff/pull/22339; review that one first.

`Type::is_redundant_with` has mostly the same semantics as `Type::is_subtype_of`, but:
- More aggressively simplifies unions in some situations
- Is cached!

There are several places in the `UnionBuilder` where it looks like we can safely used `Type::is_redundant_with` instead of `Type::is_subtype_of`. This significantly improves performance, has no impact on our test suite, and (according to mypy_primer) has no impact on any ecosystem diagnostics or ty's memory usage. (The reported changes in diagnostics are all just our standard ecosystem flakes.)


---

_Label `performance` added by @AlexWaygood on 2026-01-02 16:19_

---

_Label `ty` added by @AlexWaygood on 2026-01-02 16:19_

---

_Label `performance` added by @AlexWaygood on 2026-01-02 16:19_

---

_Label `ty` added by @AlexWaygood on 2026-01-02 16:19_

---

_Comment by @astral-sh-bot[bot] on 2026-01-02 16:21_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-02 16:22_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
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
- Found 5377 diagnostics
+ Found 5382 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:671:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[Bus[Any], object_]`, found `InterGetItemLocReduces[Bus[Any] | TypeBlocks | Batch | ... omitted 6 union elements, object_]`
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Self@iloc | Bus[Any], object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
- static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | TypeBlocks | Batch | ... omitted 6 union elements, TVDtype@Series]`
+ static_frame/core/series.py:772:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Series[Any, Any], TVDtype@Series]`, found `InterGetItemILocReduces[Series[Any, Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, TVDtype@Series]`
- static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1838 diagnostics
+ Found 1836 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14484 diagnostics
+ Found 14483 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     struct fields = ~11MB
+     struct fields = ~12MB


```

</details>




---

_Comment by @codspeed-hq[bot] on 2026-01-02 16:35_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
### Merging this PR will **improve performance by 17.32%**




### Summary

`⚡ 2` improved benchmarks  
`✅ 21` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  



### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ⚡ | WallTime | [`` altair ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmore-redundancy?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Aaltair&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 4.9 s | 4.6 s | +8.23% |
| ⚡ | WallTime | [`` multithreaded ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmore-redundancy?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amultithreaded&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 1.3 s | 1.1 s | +17.32% |
---

<sub>Comparing <code>alex/more-redundancy</code> (f3b9422) with <code>main</code> (c02d164)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/alex%2Fmore-redundancy?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>


[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmore-redundancy?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment-v2&utm_content=archive).


---

_Renamed from "[ty] Improve `UnionBuilder` performance" to "[ty] Improve `UnionBuilder` performance by changing `Type::is_subtype_of` calls to `Type::is_redundant_with`" by @AlexWaygood on 2026-01-02 17:07_

---

_Marked ready for review by @AlexWaygood on 2026-01-02 17:14_

---

_Review requested from @carljm by @AlexWaygood on 2026-01-02 17:14_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-02 17:14_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-02 17:14_

---

_@MichaReiser approved on 2026-01-03 10:24_

I'd prefer if @carljm reviews this PR too because I'm not very familiar with the semantic differences of the two methods but this now matches the pattern we use in the more general `push_type`

---

_@carljm approved on 2026-01-07 18:42_

Looks good!

It seems a bit subtle when we can use `is_redundant_with` and when we need `is_subtype_of` -- there are still several `is_subtype_of` checks here, one with enums and several when we are checking subsumption by a negated type. I wonder how we could improve this... but I don't think it needs to happen in this PR.

Pushing one doc-comment update.

---

_Comment by @AlexWaygood on 2026-01-07 22:16_

> Looks good!
> 
> It seems a bit subtle when we can use `is_redundant_with` and when we need `is_subtype_of` -- there are still several `is_subtype_of` checks here, one with enums and several when we are checking subsumption by a negated type. I wonder how we could improve this... but I don't think it needs to happen in this PR.
> 
> Pushing one doc-comment update.

I don't think our simplifications for intersections with negated gradual elements is quite right on `main`, and I suspect this is why switching to `is_redundant_with` for the negated-type-subsumption check causes some subtle behaviour changes. I have a very stale PR that attempts to fix some of these issues that I need to get back to at some point (the first version of that PR was incorrect, as David pointed out in review... https://github.com/astral-sh/ruff/pull/20651)

---

_Merged by @AlexWaygood on 2026-01-07 22:17_

---

_Closed by @AlexWaygood on 2026-01-07 22:17_

---

_Branch deleted on 2026-01-07 22:17_

---
