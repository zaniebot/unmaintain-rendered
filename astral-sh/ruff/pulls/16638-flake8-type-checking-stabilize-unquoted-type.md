```yaml
number: 16638
title: "[`flake8-type-checking`] Stabilize `unquoted-type-alias` (`TC007`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/tc007-0.10
created_at: 2025-03-11T17:22:29Z
updated_at: 2025-03-12T12:08:54Z
url: https://github.com/astral-sh/ruff/pull/16638
synced_at: 2026-01-12T15:55:55Z
```

# [`flake8-type-checking`] Stabilize `unquoted-type-alias` (`TC007`)

---

_@ntBre_

Summary
--

Stabilizes TC007. The test was already in the right place.

Test Plan
--

No open issues or PRs.

---

_Label `rule` added by @ntBre on 2025-03-11 17:22_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-11 17:22_

---

_Comment by @codspeed-hq[bot] on 2025-03-11 17:27_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Ftc007-0.10)

### Merging #16638 will **degrade performances by 12.88%**

<sub>Comparing <code>brent/tc007-0.10</code> (70ed16c) with <code>micha/ruff-0.10</code> (5ec7c13)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Ftc007-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.8 ms | 5.5 ms | -12.88% |


---

_Comment by @github-actions[bot] on 2025-03-11 17:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-03-12 08:03_

---

_Merged by @ntBre on 2025-03-12 12:08_

---

_Closed by @ntBre on 2025-03-12 12:08_

---

_Branch deleted on 2025-03-12 12:08_

---
