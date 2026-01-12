```yaml
number: 16794
title: "[internal]: Upgrade salsa"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/salsa-gc
created_at: 2025-03-17T09:56:29Z
updated_at: 2025-03-17T10:08:57Z
url: https://github.com/astral-sh/ruff/pull/16794
synced_at: 2026-01-12T15:55:57Z
```

# [internal]: Upgrade salsa

---

_@MichaReiser_

## Summary

Another salsa upgrade. 

The main motivation is to stay on a recent salsa version because there are still a lot of breaking changes happening. 
The most significant changes in this update:

* Salsa no longer derives `Debug` by default. It now requires `interned(debug)` (or similar)
* This version ships the foundation for garbage collecting interned values. However, this comes at the cost that queries now track which interned values they created (or read). The micro benchmarks in the salsa repo showed a significant perf regression. Will see if this also visible in our benchmarks.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2025-03-17 09:56_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-17 09:56_

---

_Review requested from @sharkdp by @MichaReiser on 2025-03-17 09:56_

---

_Label `internal` added by @MichaReiser on 2025-03-17 09:56_

---

_Label `red-knot` added by @MichaReiser on 2025-03-17 09:56_

---

_@MichaReiser reviewed on 2025-03-17 09:57_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/db.rs`:63 on 2025-03-17 09:57_

I created a custom `Debug` implementation that only prints the path unless `{:#?}` is used which seems the better default and simplifies the log calls

---

_Comment by @codspeed-hq[bot] on 2025-03-17 10:04_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fsalsa-gc)

### Merging #16794 will **improve performances by 4.91%**

<sub>Comparing <code>micha/salsa-gc</code> (e7d216e) with <code>main</code> (dbdb46d)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[incremental] `` | 5.3 ms | 5 ms | +4.91% |


---

_Comment by @github-actions[bot] on 2025-03-17 10:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @MichaReiser on 2025-03-17 10:05_

---

_Closed by @MichaReiser on 2025-03-17 10:05_

---

_Branch deleted on 2025-03-17 10:05_

---

_Comment by @github-actions[bot] on 2025-03-17 10:08_

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
