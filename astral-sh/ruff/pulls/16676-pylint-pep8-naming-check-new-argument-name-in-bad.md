```yaml
number: 16676
title: "[`pylint`/`pep8-naming`] Check `__new__` argument name in `bad-staticmethod-argument` and not `invalid-first-argument-name-for-class-method` (`PLW0211`/`N804`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/invalid-argument-new-method
created_at: 2025-03-12T13:45:33Z
updated_at: 2025-03-13T07:59:51Z
url: https://github.com/astral-sh/ruff/pull/16676
synced_at: 2026-01-10T19:49:02Z
```

# [`pylint`/`pep8-naming`] Check `__new__` argument name in `bad-staticmethod-argument` and not `invalid-first-argument-name-for-class-method` (`PLW0211`/`N804`)

---

_Pull request opened by @MichaReiser on 2025-03-12 13:45_

## Summary

This PR stabilizes the behavior changes introduced by https://github.com/astral-sh/ruff/pull/13305 that were gated behind preview. 
The change is that `__new__` methods are now no longer flagged by `invalid-first-argument-name-for-class-method` (`N804`) but instead by
`bad-staticmethod-argument` (`PLW0211`)

> __new__ methods are technically static methods, with cls as their first argument. However, Ruff currently classifies them as classmethod, which causes two issues:

## Test Plan

There have been no new issues or PRs related to `N804` or `PLW0211` since the behavior change was released in Ruff 0.9.7 (about 3 weeks ago). 
This is a somewhat recent change but I don't think it's necessary to leave this in preview for another 2 months. The main reason why it was in preview
is that it is breaking, not because it is a risky change.


---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 13:46_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 13:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Finvalid-argument-new-method)

### Merging #16676 will **degrade performances by 4.61%**

<sub>Comparing <code>micha/invalid-argument-new-method</code> (597cf2c) with <code>micha/ruff-0.10</code> (b93f829)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Finvalid-argument-new-method)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.61% |


---

_Comment by @github-actions[bot] on 2025-03-12 13:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 13:59_

---

_@ntBre approved on 2025-03-12 16:22_

---

_Label `rule` added by @MichaReiser on 2025-03-13 07:51_

---

_Merged by @MichaReiser on 2025-03-13 07:59_

---

_Closed by @MichaReiser on 2025-03-13 07:59_

---

_Branch deleted on 2025-03-13 07:59_

---
