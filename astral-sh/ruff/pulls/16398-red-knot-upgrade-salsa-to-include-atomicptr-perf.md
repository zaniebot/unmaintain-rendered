```yaml
number: 16398
title: "[red-knot] Upgrade salsa to include `AtomicPtr` perf improvement"
type: pull_request
state: merged
author: MichaReiser
labels:
  - performance
  - ty
assignees: []
merged: true
base: main
head: micha/update-salsa-atomic-ptr
created_at: 2025-02-26T15:01:19Z
updated_at: 2025-02-26T16:02:08Z
url: https://github.com/astral-sh/ruff/pull/16398
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Upgrade salsa to include `AtomicPtr` perf improvement

---

_Pull request opened by @MichaReiser on 2025-02-26 15:01_

## Summary

Pulling in @ibraheemdev's perf improvement to use `AtomicPtr` in salsa

https://github.com/salsa-rs/salsa/pull/737


---

_Label `red-knot` added by @MichaReiser on 2025-02-26 15:01_

---

_Comment by @codspeed-hq[bot] on 2025-02-26 15:07_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fupdate-salsa-atomic-ptr)

### Merging #16398 will **improve performances by 8.96%**

<sub>Comparing <code>micha/update-salsa-atomic-ptr</code> (538964f) with <code>main</code> (dd6f623)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[incremental] `` | 6 ms | 5.5 ms | +8.96% |


---

_Comment by @MichaReiser on 2025-02-26 15:09_

Nice

---

_Label `performance` added by @MichaReiser on 2025-02-26 15:09_

---

_Comment by @github-actions[bot] on 2025-02-26 15:10_

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

_@AlexWaygood approved on 2025-02-26 15:45_

---

_Merged by @MichaReiser on 2025-02-26 16:02_

---

_Closed by @MichaReiser on 2025-02-26 16:02_

---

_Branch deleted on 2025-02-26 16:02_

---
