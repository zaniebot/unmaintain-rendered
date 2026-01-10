```yaml
number: 19827
title: Update salsa
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: update-salsa2
created_at: 2025-08-08T12:35:58Z
updated_at: 2025-08-08T15:51:52Z
url: https://github.com/astral-sh/ruff/pull/19827
synced_at: 2026-01-10T17:52:17Z
```

# Update salsa

---

_Pull request opened by @MichaReiser on 2025-08-08 12:35_

## Summary

Update salsa to pull in https://github.com/salsa-rs/salsa/pull/961

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2025-08-08 12:35_

---

_Comment by @github-actions[bot] on 2025-08-08 12:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-08 12:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-08-08 12:46_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/update-salsa2?runnerMode=Instrumentation)

### Merging #19827 will **degrade performances by 5.63%**

<sub>Comparing <code>update-salsa2</code> (f502a97) with <code>main</code> (50e1ecc)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 41` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` ty_check_file[incremental] `` | 4.7 ms | 4.9 ms | -5.63% |


---

_Comment by @github-actions[bot] on 2025-08-08 12:47_

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

_Comment by @MichaReiser on 2025-08-08 12:58_

The performance regression is a bit unfortunate but it's hard to tell if it's because the validation is now more correct or if the code is actually slower (it's fairly likely that we took some short-cuts in the old version that weren't safe and could lead to stale results)

---

_Merged by @MichaReiser on 2025-08-08 15:51_

---

_Closed by @MichaReiser on 2025-08-08 15:51_

---

_Branch deleted on 2025-08-08 15:51_

---
