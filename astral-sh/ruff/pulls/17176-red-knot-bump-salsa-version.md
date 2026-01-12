```yaml
number: 17176
title: "[red-knot] bump Salsa version"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/bumpsalsa
created_at: 2025-04-03T14:16:49Z
updated_at: 2025-04-03T14:43:35Z
url: https://github.com/astral-sh/ruff/pull/17176
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] bump Salsa version

---

_@carljm_

Update to latest Salsa main branch, so as to get a baseline for measuring the perf effect of https://github.com/salsa-rs/salsa/pull/786 on red-knot in isolation from other recent changes in Salsa main branch.

---

_Comment by @github-actions[bot] on 2025-04-03 14:26_

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

_Marked ready for review by @carljm on 2025-04-03 14:29_

---

_Merged by @carljm on 2025-04-03 14:31_

---

_Closed by @carljm on 2025-04-03 14:31_

---

_Branch deleted on 2025-04-03 14:31_

---

_Renamed from "[DO NOT LAND] bump Salsa version" to "[red-knot] bump Salsa version" by @carljm on 2025-04-03 14:32_

---

_Comment by @codspeed-hq[bot] on 2025-04-03 14:34_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fbumpsalsa)

### Merging #17176 will **improve performances by 6.55%**

<sub>Comparing <code>cjm/bumpsalsa</code> (d235812) with <code>main</code> (6e2b8f9)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[incremental] `` | 5.1 ms | 4.8 ms | +6.55% |


---

_Label `red-knot` added by @carljm on 2025-04-03 14:43_

---
