```yaml
number: 17994
title: "[ty] WIP: Disambiguate legacy typevars"
type: pull_request
state: closed
author: dcreager
labels:
  - internal
  - ty
assignees: []
draft: true
base: main
head: dcreager/distinct-legacy-typevars
created_at: 2025-05-09T19:14:53Z
updated_at: 2025-07-28T21:37:29Z
url: https://github.com/astral-sh/ruff/pull/17994
synced_at: 2026-01-12T15:56:09Z
```

# [ty] WIP: Disambiguate legacy typevars

---

_@dcreager_

This addresses @carljm's comment https://github.com/astral-sh/ruff/pull/17956#pullrequestreview-2826559665 about wanting to disambiguate legacy typevars that are used in multiple generic contexts.

WIP, not ready for review yet

---

_Label `internal` added by @dcreager on 2025-05-09 19:14_

---

_Label `ty` added by @dcreager on 2025-05-09 19:14_

---

_Comment by @github-actions[bot] on 2025-05-09 19:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zulip (https://github.com/zulip/zulip)
- zerver/actions/message_send.py:1836:5: error[invalid-assignment] Invalid assignment to data descriptor attribute `type` on type `Message` with custom `__set__` method
- Found 7319 diagnostics
+ Found 7318 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~21MB
+     memo metadata = ~22MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~541MB
+ TOTAL MEMORY USAGE: ~568MB
-     struct metadata = ~23MB
+     struct metadata = ~25MB
-     memo metadata = ~73MB
+     memo metadata = ~76MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-05-09 19:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistinct-legacy-typevars?runnerMode=Instrumentation)

### Merging #17994 will **degrade performances by 6.88%**

<sub>Comparing <code>dcreager/distinct-legacy-typevars</code> (f786216) with <code>main</code> (1d21816)</sub>



### Summary

`❌ 6` regressions  
`✅ 35` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistinct-legacy-typevars?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` ty_check_file[cold] `` | 116.3 ms | 121.3 ms | -4.07% |
| ❌ | `` ty_check_file[incremental] `` | 4.9 ms | 5.3 ms | -6.88% |
| ❌ | `` ty_micro[many_tuple_assignments] `` | 116.8 ms | 122 ms | -4.27% |
| ❌ | `` anyio `` | 832.8 ms | 882.7 ms | -5.65% |
| ❌ | `` attrs `` | 364.8 ms | 383.9 ms | -4.99% |
| ❌ | `` hydra-zen `` | 792.1 ms | 834.9 ms | -5.12% |


---

_Comment by @codspeed-hq[bot] on 2025-07-24 16:16_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistinct-legacy-typevars?runnerMode=WallTime)

### Merging #17994 will **degrade performances by 10.88%**

<sub>Comparing <code>dcreager/distinct-legacy-typevars</code> (f786216) with <code>main</code> (1d21816)</sub>



### Summary

`❌ 7` regressions  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdistinct-legacy-typevars?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` large[sympy] `` | 43.4 s | 47 s | -7.63% |
| ❌ | `` medium[colour-science] `` | 7.2 s | 8 s | -9.99% |
| ❌ | `` medium[pandas] `` | 29.1 s | 31.9 s | -8.63% |
| ❌ | `` small[altair] `` | 2.4 s | 2.7 s | -10.88% |
| ❌ | `` small[freqtrade] `` | 4.1 s | 4.4 s | -6.65% |
| ❌ | `` small[pydantic] `` | 2.3 s | 2.5 s | -10.01% |
| ❌ | `` small[tanjun] `` | 1.7 s | 1.8 s | -8.93% |


---

_Comment by @dcreager on 2025-07-28 21:37_

superseded by #19604 

---

_Closed by @dcreager on 2025-07-28 21:37_

---

_Branch deleted on 2025-07-28 21:37_

---
