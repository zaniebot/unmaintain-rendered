```yaml
number: 16653
title: "[`ruff`] Stabilize `map-int-version-parsing` (`RUF048`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/ruf048-0.10
created_at: 2025-03-12T02:06:27Z
updated_at: 2025-03-12T12:34:20Z
url: https://github.com/astral-sh/ruff/pull/16653
synced_at: 2026-01-12T15:55:55Z
```

# [`ruff`] Stabilize `map-int-version-parsing` (`RUF048`)

---

_@ntBre_

Summary
--

Stabilizes RUF048 and moves its test to the right place. The docs look good.

Test Plan
--

0 closed or open issues. There was 1 [PR] related to an extension to the rule, but it was closed without comment.

[PR]: https://github.com/astral-sh/ruff/pull/14701


---

_Label `rule` added by @ntBre on 2025-03-12 02:06_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-12 02:06_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 02:11_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf048-0.10)

### Merging #16653 will **degrade performances by 10.81%**

<sub>Comparing <code>brent/ruf048-0.10</code> (0d01cda) with <code>micha/ruff-0.10</code> (b8f1284)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fruf048-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.81% |


---

_Comment by @github-actions[bot] on 2025-03-12 02:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-03-12 07:33_

---

_Merged by @ntBre on 2025-03-12 12:34_

---

_Closed by @ntBre on 2025-03-12 12:34_

---

_Branch deleted on 2025-03-12 12:34_

---
