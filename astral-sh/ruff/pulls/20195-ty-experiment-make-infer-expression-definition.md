```yaml
number: 20195
title: "[ty] Experiment: make `infer_{expression,definition}_types` non-queries"
type: pull_request
state: closed
author: sharkdp
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: david/fix-1111
created_at: 2025-09-01T14:48:09Z
updated_at: 2025-09-03T11:19:06Z
url: https://github.com/astral-sh/ruff/pull/20195
synced_at: 2026-01-12T15:56:56Z
```

# [ty] Experiment: make `infer_{expression,definition}_types` non-queries

---

_@sharkdp_

## Summary

For the [snippet in this comment](https://github.com/astral-sh/ty/issues/1111#issuecomment-3241694867), I observe the following execution times (release mode):

| Version   | `member_look…` | `infer_{expr,def}…_types` | `infer_local_place_load` | Execution time [s] |
|-----------|-------------------|---------------------------------------|--------------------------|-------------------:|
| main      | x                 | x                                     |                          |               27.0 |
| `fee980a` | x                 |                                       | x                        |                2.7 |
| `c6b661f` | x                 |                                       |                          |               0.34 |

The version without any cycle handling except for `member_lookup_…` (last row) is almost two orders of magnitude faster, but it leads to stack overflows on snippets like `a: a` with deferred name lookups. Adding cycle handling to `infer_local_place_load` fixes that, but is only 10x faster than `main`. Unfortunately, it also leads to regressions in all kinds of benchmarks, so this is not the solution that we're looking for.

see https://github.com/astral-sh/ty/issues/1111

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-09-01 14:48_

---

_Label `internal` added by @sharkdp on 2025-09-01 14:48_

---

_Renamed from "[ty] Experiment: make infer_expression_types/infer_definition_types n…" to "[ty] Experiment: make `infer_{expression,definition}_types` non-queries" by @sharkdp on 2025-09-01 14:48_

---

_Comment by @github-actions[bot] on 2025-09-01 14:50_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-09-02 07:23:49.023499898 +0000
+++ new-output.txt	2025-09-02 07:23:53.468536771 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(b8e9)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/a3ffa22/src/function/execute.rs:228:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_local_place_load(Id(c87c)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-02 07:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~4MB
+     struct fields = ~5MB
-     memo metadata = ~5MB
+     memo metadata = ~7MB
-     memo fields = ~57MB
+     memo fields = ~54MB

trio (https://github.com/python-trio/trio)
- TOTAL MEMORY USAGE: ~167MB
+ TOTAL MEMORY USAGE: ~176MB
-     struct metadata = ~8MB
+     struct metadata = ~10MB
-     struct fields = ~12MB
+     struct fields = ~13MB
-     memo metadata = ~22MB
+     memo metadata = ~30MB
-     memo fields = ~125MB
+     memo fields = ~119MB

sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~260MB
+ TOTAL MEMORY USAGE: ~273MB
-     struct metadata = ~12MB
+     struct metadata = ~17MB
-     struct fields = ~17MB
+     struct fields = ~20MB
-     memo metadata = ~38MB
+     memo metadata = ~54MB
-     memo fields = ~194MB
+     memo fields = ~176MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~596MB
+ TOTAL MEMORY USAGE: ~626MB
-     struct metadata = ~27MB
+     struct metadata = ~40MB
-     struct fields = ~42MB
+     struct fields = ~49MB
-     memo metadata = ~80MB
+     memo metadata = ~119MB
-     memo fields = ~445MB
+     memo fields = ~403MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-09-02 07:34_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1111?runnerMode=Instrumentation)

### Merging #20195 will **degrade performances by 61.95%**

<sub>Comparing <code>david/fix-1111</code> (fee980a) with <code>main</code> (bbfcf6e)</sub>



### Summary

`❌ 8` regressions  
`✅ 35` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1111?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_check_file[cold] `` | 118.6 ms | 144.2 ms | -17.73% |
| ❌ | `` ty_check_file[incremental] `` | 4.8 ms | 7.4 ms | -34.14% |
| ❌ | `` ty_micro[many_enum_members] `` | 94 ms | 102.6 ms | -8.38% |
| ❌ | `` ty_micro[many_string_assignments] `` | 73.1 ms | 77.4 ms | -5.52% |
| ❌ | `` anyio `` | 849.9 ms | 1,081.2 ms | -21.39% |
| ❌ | `` attrs `` | 366.2 ms | 492.7 ms | -25.67% |
| ❌ | `` DateType `` | 267.6 ms | 381.3 ms | -29.82% |
| ❌ | `` hydra-zen `` | 729 ms | 1,916.2 ms | -61.95% |


---

_Comment by @codspeed-hq[bot] on 2025-09-02 07:38_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1111?runnerMode=WallTime)

### Merging #20195 will **degrade performances by 54.25%**

<sub>Comparing <code>david/fix-1111</code> (fee980a) with <code>main</code> (bbfcf6e)</sub>



### Summary

`❌ 8` regressions  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1111?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` large[sympy] `` | 39.9 s | 81.6 s | -51.15% |
| ❌ | `` medium[colour-science] `` | 6.8 s | 11.6 s | -41.22% |
| ❌ | `` medium[pandas] `` | 25.8 s | 47.1 s | -45.3% |
| ❌ | `` medium[static-frame] `` | 7.8 s | 12.8 s | -39.05% |
| ❌ | `` small[altair] `` | 2.5 s | 5.4 s | -54.25% |
| ❌ | `` small[freqtrade] `` | 4 s | 7.5 s | -46.24% |
| ❌ | `` small[pydantic] `` | 2.3 s | 4.3 s | -47.04% |
| ❌ | `` small[tanjun] `` | 1.7 s | 2.6 s | -35.82% |


---

_Closed by @sharkdp on 2025-09-03 11:19_

---
