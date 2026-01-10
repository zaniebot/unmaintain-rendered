```yaml
number: 19449
title: Update salsa
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/update-salsa
created_at: 2025-07-21T03:34:11Z
updated_at: 2025-07-30T19:31:48Z
url: https://github.com/astral-sh/ruff/pull/19449
synced_at: 2026-01-10T17:58:13Z
```

# Update salsa

---

_Pull request opened by @ibraheemdev on 2025-07-21 03:34_

## Summary

Pulls in a bunch of salsa micro-optimizations.

---

_Label `internal` added by @ibraheemdev on 2025-07-21 03:34_

---

_Label `ty` added by @ibraheemdev on 2025-07-21 03:34_

---

_Comment by @codspeed-hq[bot] on 2025-07-21 03:43_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fupdate-salsa?runnerMode=Instrumentation)

### Merging #19449 will **improve performances by 5.07%**

<sub>Comparing <code>ibraheem/update-salsa</code> (d9b1a64) with <code>main</code> (7b4103b)</sub>



### Summary

`⚡ 1` improvements  
`✅ 41` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_check_file[incremental] `` | 5 ms | 4.7 ms | +5.07% |


---

_Comment by @codspeed-hq[bot] on 2025-07-21 03:45_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fupdate-salsa?runnerMode=WallTime)

### Merging #19449 will **improve performances by 7.78%**

<sub>Comparing <code>ibraheem/update-salsa</code> (d9b1a64) with <code>main</code> (7b4103b)</sub>



### Summary

`⚡ 7` improvements  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` large[sympy] `` | 43.3 s | 39.7 s | +8.98% |
| ⚡ | `` medium[colour-science] `` | 7.5 s | 6.9 s | +9.3% |
| ⚡ | `` medium[pandas] `` | 28.6 s | 26.3 s | +8.57% |
| ⚡ | `` small[altair] `` | 2.4 s | 2.2 s | +8.85% |
| ⚡ | `` small[freqtrade] `` | 4.1 s | 3.8 s | +8% |
| ⚡ | `` small[pydantic] `` | 2.3 s | 2.1 s | +9.57% |
| ⚡ | `` small[tanjun] `` | 1.7 s | 1.6 s | +7.78% |


---

_Comment by @github-actions[bot] on 2025-07-21 03:46_

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

_Comment by @github-actions[bot] on 2025-07-21 04:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-30 17:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Marked ready for review by @ibraheemdev on 2025-07-30 17:51_

---

_@MichaReiser approved on 2025-07-30 19:06_

---

_Merged by @ibraheemdev on 2025-07-30 19:31_

---

_Closed by @ibraheemdev on 2025-07-30 19:31_

---

_Branch deleted on 2025-07-30 19:31_

---
