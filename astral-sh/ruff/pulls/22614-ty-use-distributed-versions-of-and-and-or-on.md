```yaml
number: 22614
title: "[ty] Use distributed versions of AND and OR on constraint sets"
type: pull_request
state: open
author: dcreager
labels:
  - internal
  - performance
  - ty
assignees: []
base: main
head: dcreager/distributed-ops
created_at: 2026-01-16T10:01:46Z
updated_at: 2026-01-21T01:18:35Z
url: https://github.com/astral-sh/ruff/pull/22614
synced_at: 2026-01-21T02:01:17Z
```

# [ty] Use distributed versions of AND and OR on constraint sets

---

_@dcreager_

There are some pathological examples where we create a constraint set which is the AND or OR of several smaller constraint sets. For example, when calling a function with many overloads, where the argument is a typevar, we create an OR of the typevar specializing to a type compatible with the respective parameter of each overload.

Most functions have a small number of overloads. But there are some examples of methods with 15-20 overloads (pydantic, numpy, our own auto-generated `__getitem__` for large tuple literals). For those cases, it is helpful to be more clever about how we construct the final result.

Before, we would just step through the `Iterator` of elements and accumulate them into a result constraint set. That results in an `O(n)` number of calls to the underlying `and` or `or` operator — each of which might have to construct a large temporary BDD tree.

AND and OR are both associative, so we can do better! We now invoke the operator in a "tree" shape (described in more detail in the doc comment). We still have to perform the same number of calls, but more of the calls operate on smaller BDDs, resulting in a much smaller amount of overall work.

---

_Label `internal` added by @dcreager on 2026-01-16 10:01_

---

_Label `performance` added by @dcreager on 2026-01-16 10:01_

---

_Label `ty` added by @dcreager on 2026-01-16 10:01_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 10:03_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-16 10:05_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
tornado (https://github.com/tornadoweb/tornado)
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:99:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 46 diagnostics
+ Found 47 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14465 diagnostics
+ Found 14464 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@dcreager reviewed on 2026-01-16 10:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:723 on 2026-01-16 10:08_

While we're here, I'm updating the BDD variable ordering to be less clever. For the pathological example from #21902 this has a huge benefit.

---

_@dcreager reviewed on 2026-01-16 10:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:4028 on 2026-01-16 10:08_

and also update the tree display to make it more obvious where we're sharing tree structure

---

_Marked ready for review by @dcreager on 2026-01-16 10:09_

---

_Review requested from @carljm by @dcreager on 2026-01-16 10:09_

---

_Review requested from @AlexWaygood by @dcreager on 2026-01-16 10:09_

---

_Review requested from @sharkdp by @dcreager on 2026-01-16 10:09_

---

_Comment by @codspeed-hq[bot] on 2026-01-16 10:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?utm_source=github&utm_medium=comment&utm_content=header)

### Merging this PR will **degrade performance by 45.74%**

<sub>Comparing <code>dcreager/distributed-ops</code> (18f5b2a) with <code>main</code> (d4123fc)</sub>



### Summary

`❌ 4` regressed benchmarks  
`✅ 19` untouched benchmarks  
`⏩ 30` skipped benchmarks[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Performance Changes

|     | Mode | Benchmark | `BASE` | `HEAD` | Efficiency |
| --- | ---- | --------- | ------ | ------ | ---------- |
| ❌ | WallTime | [`` colour_science ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Acolour_science&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 87.2 s | 160.6 s | -45.74% |
| ❌ | WallTime | [`` pandas ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Apandas&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 61.8 s | 68.7 s | -10.06% |
| ❌ | WallTime | [`` static_frame ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Astatic_frame&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 20.8 s | 22.8 s | -8.81% |
| ❌ | WallTime | [`` freqtrade ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Afreqtrade&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 7.7 s | 8.1 s | -4.59% |

[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistributed-ops?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @MichaReiser on 2026-01-16 10:43_

From the summary, I expect this to improve performance and reduce memory usage. Both don't seem to be the case. Do you have an understanding where the performance regression comes from? Could we keep using the "old approach" when not dealing with many overloads?

---

_@MichaReiser reviewed on 2026-01-16 10:45_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:1828 on 2026-01-16 10:45_

Could this be a `FxIndexSet` or why is it important that `seen` has `Eq` and `PartialEq`?

---

_@MichaReiser reviewed on 2026-01-16 10:47_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 10:47_

Could it be that the collect calls are expensive? Given that `distributed_or` and `distributed_and` are very small methods, would it make sense to pass the `f` through and apply the mapping in `distributed_or`/and?

Another alternative is to use a `SmallVec` instead. But I wonder if part of the perf regression simply comes from writing all the constraints to a vec


---

_Comment by @dcreager on 2026-01-16 12:37_

> From the summary, I expect this to improve performance and reduce memory usage. Both don't seem to be the case. Do you have an understanding where the performance regression comes from? Could we keep using the "old approach" when not dealing with many overloads?

Me too! Maybe the collecting is the culprit, as you suggest? Or maybe because we're not short circuiting anymore? I have an idea that might help with both.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 13:25_

I've pushed up a new version that doesn't collect into a vec first. I want to see how that affects the perf numbers; if it works well I plan to add some better documentation comments describing how it works

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:1828 on 2026-01-16 13:25_

It can! Done (My muscle memory is to immediately reach for `FxOrderSet` first)

---

_@dcreager reviewed on 2026-01-16 13:25_

---

_Comment by @AlexWaygood on 2026-01-16 13:40_

Wow, nice job getting this from a 5x perf regression to a 4% perf improvement!!

---

_@dcreager reviewed on 2026-01-16 14:51_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:149 on 2026-01-16 14:51_

This seems to work well! Pushed up some documentation of the approach.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:150 on 2026-01-19 09:16_

This PR does improve the performance by a fair bit but it also regresses performance by about 1-2 on most other projects. 

It would be lovely if we could use specialization to specialize `when_any` and `when_all` for `ExactSizeIterator` so that we could use the old implementation if there are only very few items. But, that's unlikely an option any time soon unless we migrate to nightly Rust.

I went through some `when_any` usages and:

* We could implement `when_any` for `&[T]` and `&FxOrderSet`
* We could add a `when_any_exact` method (or rename `when_any` to `when_any_iter` to advertise the `ExactSizeIterator` version)


---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/constraints.rs`:723 on 2026-01-19 09:17_

Should we do this in a separate PR so that we better understand where the performance improvements are coming from?

---

_@MichaReiser approved on 2026-01-19 09:22_

Nice. 

It might make sense to specialize `distribute_or` and `distribute_and` (or `when_any`) for `&[T]` and `ExactSizeIterator` as we see a perf regression on many projects (while small). Unless the perf regression is related to the `ordering` change. I suggest splitting that change into its own PR so that we have a better understanding where the regression is coming from.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:723 on 2026-01-20 21:28_

Done: https://github.com/astral-sh/ruff/pull/22777

(I have not addressed the other comments below yet; did this first to see what the performance looks like before considering a fallback for smaller vecs/etc)

---

_@dcreager reviewed on 2026-01-20 21:28_

---

_@dcreager reviewed on 2026-01-20 23:03_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:150 on 2026-01-20 23:03_

An `ExactSizeIterator` is required to return the exact size from `size_hint`, too, so I added a fallback that checks if the max size hint is <= 4, and uses the old implementation if so.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/constraints.rs`:150 on 2026-01-20 23:14_

(That way I didn't have to worry about specialization or adding a new method for exact-sized things)

---

_@dcreager reviewed on 2026-01-20 23:14_

---

_Comment by @ibraheemdev on 2026-01-21 00:18_

Looks like the last commit regressed performance again? 

---

_@ibraheemdev reviewed on 2026-01-21 01:17_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/constraints.rs`:1167 on 2026-01-21 01:17_

It's unclear to me why this is algorithmically cheaper. If the constraint sets are all disjoint, this is equivalent (modulo node ordering, which we can really control here). The worst case is also equivalent. Otherwise, I don't see how this is better given that both BDDs operate on the same set of pairs of nodes — we end up ORing two medium sized constraint sets instead of a large constraint set with a small one. I may be missing something here, given that the benchmarks agree this is an improvement.

---

_@ibraheemdev reviewed on 2026-01-21 01:18_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types/constraints.rs`:1204 on 2026-01-21 01:18_

Doesn't this assume the input constraint sets are of equal or similar size (though I suppose that is mostly true currently)? Should we try sorting by size here?

---
