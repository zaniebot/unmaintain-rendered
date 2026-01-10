```yaml
number: 16632
title: "[`pyupgrade`] Stabilize `non-pep646-unpack` (`UP044`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/up044-0.10
created_at: 2025-03-11T16:12:40Z
updated_at: 2025-03-11T18:01:12Z
url: https://github.com/astral-sh/ruff/pull/16632
synced_at: 2026-01-10T19:49:01Z
```

# [`pyupgrade`] Stabilize `non-pep646-unpack` (`UP044`)

---

_Pull request opened by @ntBre on 2025-03-11 16:12_

Summary
--

Stabilizes UP044, renames the module to match the rule name, and removes the `PreviewMode` from the test settings.

Test Plan
--

2 closed issues in November, just after the rule was added, otherwise no issues


---

_Label `rule` added by @ntBre on 2025-03-11 16:12_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 16:12_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 16:17_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fup044-0.10)

### Merging #16632 will **degrade performances by 12.88%**

<sub>Comparing <code>brent/up044-0.10</code> (5dbfa88) with <code>micha/ruff-0.10</code> (5ec7c13)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fup044-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.8 ms | 5.5 ms | -12.88% |


---

_Comment by @github-actions[bot] on 2025-03-11 16:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-03-11 17:54_

---

_Merged by @ntBre on 2025-03-11 18:01_

---

_Closed by @ntBre on 2025-03-11 18:01_

---

_Branch deleted on 2025-03-11 18:01_

---
