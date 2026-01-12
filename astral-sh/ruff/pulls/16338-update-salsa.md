```yaml
number: 16338
title: Update Salsa
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/update-salsa
created_at: 2025-02-24T08:04:18Z
updated_at: 2025-02-24T08:46:35Z
url: https://github.com/astral-sh/ruff/pull/16338
synced_at: 2026-01-12T15:55:54Z
```

# Update Salsa

---

_@MichaReiser_

Update Salsa to fix stale query results for multi-argument queries where one argument is a tracked struct. 

The performance regression isn't unexpected. https://github.com/astral-sh/ruff/pull/15763 introduced coarse grained dependencies and the version used in that branch removed adding dependencies for any tracked fields (not even to the tracked struct itself). It turned out, that this was incorrect in the case where tracked structs (and there IDs) get reused. The upstream fix now records a dependency on the tracked struct. This is still better than without coarse grained dependencies where salsa recorded a dependency for each tracked field but it isn't free (but required for correctness).



---

_Label `internal` added by @MichaReiser on 2025-02-24 08:04_

---

_Comment by @codspeed-hq[bot] on 2025-02-24 08:09_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fupdate-salsa)

### Merging #16338 will **degrade performances by 5.29%**

<sub>Comparing <code>micha/update-salsa</code> (5295d99) with <code>main</code> (5eaf225)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_check_file[incremental] `` | 5.5 ms | 5.8 ms | -5.29% |


---

_Label `red-knot` added by @AlexWaygood on 2025-02-24 08:12_

---

_Comment by @github-actions[bot] on 2025-02-24 08:13_

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

_Marked ready for review by @MichaReiser on 2025-02-24 08:44_

---

_Merged by @MichaReiser on 2025-02-24 08:44_

---

_Closed by @MichaReiser on 2025-02-24 08:44_

---

_Branch deleted on 2025-02-24 08:44_

---
