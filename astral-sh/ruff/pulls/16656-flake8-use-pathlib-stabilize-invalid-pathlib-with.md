```yaml
number: 16656
title: "[`flake8-use-pathlib`] Stabilize `invalid-pathlib-with-suffix` (`PTH210`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: brent/pth210-0.10
created_at: 2025-03-12T02:43:01Z
updated_at: 2025-03-12T17:07:07Z
url: https://github.com/astral-sh/ruff/pull/16656
synced_at: 2026-01-10T19:49:02Z
```

# [`flake8-use-pathlib`] Stabilize `invalid-pathlib-with-suffix` (`PTH210`)

---

_Pull request opened by @ntBre on 2025-03-12 02:43_

Summary
--

Stabilizes PTH210. Tests and docs looked good.

Test Plan
--

Mentioned in 1 open issue around Python 3.14 support (`"."` becomes a valid suffix in 3.14). Otherwise no issues or PRs since 2024-12-12, 6 days after the rule was added.

---

_Label `rule` added by @ntBre on 2025-03-12 02:43_

---

_Added to milestone `v0.10` by @ntBre on 2025-03-12 02:43_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 02:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fpth210-0.10)

### Merging #16656 will **degrade performances by 4.6%**

<sub>Comparing <code>brent/pth210-0.10</code> (e94bf05) with <code>micha/ruff-0.10</code> (464ea4a)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Fpth210-0.10)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.6% |


---

_Comment by @github-actions[bot] on 2025-03-12 02:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-03-12 07:33_

---

_Merged by @ntBre on 2025-03-12 17:07_

---

_Closed by @ntBre on 2025-03-12 17:07_

---

_Branch deleted on 2025-03-12 17:07_

---
