```yaml
number: 16682
title: "[`flake8-pyi`] Stabilize fix for `unused-private-type-var` (`PYI018`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - fixes
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/promote-fix-pyi018
created_at: 2025-03-12T15:58:01Z
updated_at: 2025-03-13T07:48:02Z
url: https://github.com/astral-sh/ruff/pull/16682
synced_at: 2026-01-12T15:55:55Z
```

# [`flake8-pyi`] Stabilize fix for `unused-private-type-var` (`PYI018`)

---

_@MichaReiser_

## Summary

This PR stabilizes the fix for `PYI018` introduced in https://github.com/astral-sh/ruff/pull/15999/ (first released with Ruff 0.9.5 early February)

There are no known issues with the fix or open PRs.




---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-12 15:58_

---

_Label `fixes` added by @MichaReiser on 2025-03-12 15:58_

---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 15:58_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 15:58_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 16:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fpromote-fix-pyi018)

### Merging #16682 will **degrade performances by 4.61%**

<sub>Comparing <code>micha/promote-fix-pyi018</code> (cdf38ca) with <code>micha/ruff-0.10</code> (464ea4a)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fpromote-fix-pyi018)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.61% |


---

_Comment by @github-actions[bot] on 2025-03-12 16:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-03-12 16:10_

---

_Merged by @MichaReiser on 2025-03-13 07:47_

---

_Closed by @MichaReiser on 2025-03-13 07:47_

---

_Branch deleted on 2025-03-13 07:48_

---
