```yaml
number: 16655
title: "[`flake8-bugbear`] Stabilize `batched-without-explicit-strict` (`B911`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/b911-0.10
created_at: 2025-03-12T02:26:11Z
updated_at: 2025-03-12T12:36:03Z
url: https://github.com/astral-sh/ruff/pull/16655
synced_at: 2026-01-10T19:49:02Z
```

# [`flake8-bugbear`] Stabilize `batched-without-explicit-strict` (`B911`)

---

_Pull request opened by @ntBre on 2025-03-12 02:26_

Summary
--

Stabilizes B911. Tests and docs looked good.

Test Plan
--

0 issues or PRs, open or closed


---

_Label `rule` added by @ntBre on 2025-03-12 02:26_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-12 02:26_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 02:31_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fb911-0.10)

### Merging #16655 will **degrade performances by 10.81%**

<sub>Comparing <code>brent/b911-0.10</code> (50e83fb) with <code>micha/ruff-0.10</code> (b8f1284)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fb911-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.81% |


---

_Comment by @github-actions[bot] on 2025-03-12 02:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-03-12 07:45_

---

_Merged by @ntBre on 2025-03-12 12:36_

---

_Closed by @ntBre on 2025-03-12 12:36_

---

_Branch deleted on 2025-03-12 12:36_

---
