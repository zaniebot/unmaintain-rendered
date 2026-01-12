```yaml
number: 19833
title: "[ty] Handle cycles when finding implicit attributes"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/flax-hang
created_at: 2025-08-08T18:06:01Z
updated_at: 2025-08-10T13:08:14Z
url: https://github.com/astral-sh/ruff/pull/19833
synced_at: 2026-01-12T15:56:48Z
```

# [ty] Handle cycles when finding implicit attributes

---

_@dcreager_

The [minimal reproduction](https://gist.github.com/dcreager/fc53c59b30d7ce71d478dcb2c1c56444) of https://github.com/astral-sh/ty/issues/948 is an example of a class with implicit attributes whose types end up depending on themselves. Our existing cycle detection for `infer_expression_types` is usually enough to handle this situation correctly, but when there are very many of these implicit attributes, we get a combinatorial explosion of running time and memory usage.

Adding a separate cycle handler for `ClassLiteral::implicit_attribute` lets us catch and recover from this situation earlier.

Closes https://github.com/astral-sh/ty/issues/948

---

_Review requested from @carljm by @dcreager on 2025-08-08 18:06_

---

_Label `ty` added by @dcreager on 2025-08-08 18:06_

---

_Review requested from @AlexWaygood by @dcreager on 2025-08-08 18:06_

---

_Review requested from @sharkdp by @dcreager on 2025-08-08 18:06_

---

_Comment by @github-actions[bot] on 2025-08-08 18:08_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 18:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~159MB
+ TOTAL MEMORY USAGE: ~167MB
-     struct fields = ~9MB
+     struct fields = ~10MB
-     memo metadata = ~22MB
+     memo metadata = ~23MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     struct metadata = ~11MB
+     struct metadata = ~12MB
-     memo metadata = ~38MB
+     memo metadata = ~40MB

prefect (https://github.com/PrefectHQ/prefect)
-     struct metadata = ~25MB
+     struct metadata = ~26MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-08-08 18:15_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fflax-hang?runnerMode=Instrumentation)

### Merging #19833 will **improve performances by 7.15%**

<sub>Comparing <code>dcreager/flax-hang</code> (cac8c77) with <code>main</code> (0095ff4)</sub>



### Summary

`⚡ 2` improvements  
`✅ 40` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_micro[many_enum_members] `` | 103.2 ms | 96.3 ms | +7.15% |
| ⚡ | `` ty_micro[many_string_assignments] `` | 73.3 ms | 69.8 ms | +5.04% |


---

_Comment by @codspeed-hq[bot] on 2025-08-08 18:16_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fflax-hang?runnerMode=WallTime)

### Merging #19833 will **improve performances by 9.43%**

<sub>Comparing <code>dcreager/flax-hang</code> (cac8c77) with <code>main</code> (0095ff4)</sub>



### Summary

`⚡ 1` improvements  
`✅ 6` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` medium[pandas] `` | 26.9 s | 24.6 s | +9.43% |


---

_@carljm approved on 2025-08-08 20:56_

Nice find! I suspect the win here is not "catching the cycle sooner" but that we are now memoizing a chunk of work that we did not memoize before, and presumably that work was previously being repeated a lot in cycle iteration, and now is not.

This is a good reminder that there are potentially significant performance wins hiding in "add memoization" that aren't necessarily obvious until we hit a pathological case.

---

_Merged by @dcreager on 2025-08-08 21:01_

---

_Closed by @dcreager on 2025-08-08 21:01_

---

_Branch deleted on 2025-08-08 21:01_

---

_Comment by @MichaReiser on 2025-08-09 16:27_

This does seem to increae memory usage a fair bit. I'm not suggesting that this wasn't the right fix. But it might have been interesting to measure the total memory usage on a large project and comparing it to main (by using `TY_MEMORY_REPORT=full`). 

---

_Comment by @AlexWaygood on 2025-08-10 13:08_

> The [minimal reproduction](https://gist.github.com/dcreager/fc53c59b30d7ce71d478dcb2c1c56444) of [astral-sh/ty#948](https://github.com/astral-sh/ty/issues/948) is an example of a class with implicit attributes whose types end up depending on themselves.

Is it worth adding something like this as a micro-benchmark, to ensure that we don't regress on this in the future?

---
