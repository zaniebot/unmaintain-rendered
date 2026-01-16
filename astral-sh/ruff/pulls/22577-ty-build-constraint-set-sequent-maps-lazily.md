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
updated_at: 2026-01-16T00:49:41Z
url: https://github.com/astral-sh/ruff/pull/22577
synced_at: 2026-01-16T01:04:07Z
```

# [ty] Build constraint set sequent maps lazily

---

_@dcreager_

Before, when building a `SequentMap` for a constraint set, we would immediately iterate through all of the constraints in the set, and compare each pair of them looking for intersection/implication relationships. It turns out that we often don't need to examine every pair when walking the BDD tree of a constraint set. Instead, we can visit each constraint as we encounter it for the first time in our BDD walk. We do still need to collect all of the constraints in the BDD to ensure that they remain ordered in a consistent way, but we can track that separately and without having to immediate build up the actual sequents.

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 18:06_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Typing conformance

No changes



[Typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)



---

_Comment by @astral-sh-bot[bot] on 2026-01-14 18:07_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 47 diagnostics
+ Found 48 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14509 diagnostics
+ Found 14508 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2026-01-14 18:10_

---

_Label `internal` added by @dcreager on 2026-01-14 18:14_

---

_Comment by @codspeed-hq[bot] on 2026-01-14 18:26_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## Merging this PR will **degrade performance by 18.13%**




`❌ 3` regressed benchmarks  
`✅ 50` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flazy-sequent-map?utm_source=github&utm_medium=comment-v2&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flazy-sequent-map?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 21.9 s | 23.4 s | -6.48% |
| ❌ | WallTime | [`` pydantic ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flazy-sequent-map?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apydantic&runnerMode=WallTime&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 10.3 s | 11.4 s | -10.15% |
| ❌ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flazy-sequent-map?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment-v2&utm_content=benchmark) | 1.2 s | 1.5 s | -18.13% |
---

<sub>Comparing <code>dcreager/lazy-sequent-map</code> (5b240fb) with <code>main</code> (fd7cc1f)</sub>

<a href="https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flazy-sequent-map?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>



---

_@MichaReiser reviewed on 2026-01-15 14:19_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:3308 on 2026-01-15 14:19_

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

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:3308 on 2026-01-15 14:40_

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

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:3308 on 2026-01-15 15:09_

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

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:3308 on 2026-01-15 15:34_

> The one thing we need to be careful is that the internal mutability code doesn't access `db` because a query reading a cached result wouldn't see all its dependencies, breaking Salsa's cache invalidation.

Is this part true in general? That might be a deal-breaker for this approach, because the interior mutability code will definitely need to access the `db`.

---

_@MichaReiser reviewed on 2026-01-15 15:39_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:3308 on 2026-01-15 15:39_

> Is this part true in general? That might be a deal-breaker for this approach, because the interior mutability code will definitely need to access the db.

It depends on what you read, but how salsa tracks dependencies is something I'd consider internal to Salsa (or at least requires a lot of documentation)

I don't think we add read dependencies for interned structs, but we used to (CC: @ibraheemdev). But calling any salsa query, reading a tracked field of a tracked struct, or reading any input makes this approach unsound. 

---

_@MichaReiser reviewed on 2026-01-15 15:43_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:3308 on 2026-01-15 15:43_

Creating any new interned values I think would be unsound. 

I guess so is reading because reading an interned value with low durability **must** propagate to the outer query. So it's not just about the dependencies, it's also about the query's metadata that need to be reflected accordingly

---

_@dcreager reviewed on 2026-01-15 15:49_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:3308 on 2026-01-15 15:49_

Okay that tells me I need to rethink this...

---

_@ibraheemdev reviewed on 2026-01-15 21:14_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/constraints.rs`:3308 on 2026-01-15 21:14_

Yeah, interior mutability will not play well with Salsa here. If the interior mutability code creates an interned value without the `sequent_map` query having a dependency on that interned value, the interned value may be garbage collected, and later calls to `sequent_map` will read stale data.

---

_Review request for @carljm removed by @carljm on 2026-01-16 00:49_

---
