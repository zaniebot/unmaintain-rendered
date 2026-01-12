```yaml
number: 16667
title: "[`pep8-naming`]: Ignore methods decorated with `@typing.override` (`invalid-argument-name`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/invalid-argument-names-override
created_at: 2025-03-12T09:48:26Z
updated_at: 2025-03-13T07:43:49Z
url: https://github.com/astral-sh/ruff/pull/16667
synced_at: 2026-01-12T15:55:55Z
```

# [`pep8-naming`]: Ignore methods decorated with `@typing.override` (`invalid-argument-name`)

---

_@MichaReiser_

## Summary

This PR stabilizes the preview behavior for `invalid-argument-name` (`N803`) 
to ignore argument names of functions decorated with `typing.override` because
these methods are *out of the authors* control. 

This behavior was introduced in https://github.com/astral-sh/ruff/pull/15954 
and released as part of Ruff 0.9.5 (6th of February). 

There have been no new issues or PRs since this behavior change (preview) was introduced.



---

_Label `rule` added by @MichaReiser on 2025-03-12 09:48_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 09:55_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Finvalid-argument-names-override)

### Merging #16667 will **degrade performances by 10.81%**

<sub>Comparing <code>micha/invalid-argument-names-override</code> (f31db75) with <code>micha/ruff-0.10</code> (b8f1284)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Finvalid-argument-names-override)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.81% |


---

_Comment by @github-actions[bot] on 2025-03-12 09:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 12:48_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 16:58_

---

_@ntBre approved on 2025-03-12 18:01_

---

_Merged by @MichaReiser on 2025-03-13 07:43_

---

_Closed by @MichaReiser on 2025-03-13 07:43_

---

_Branch deleted on 2025-03-13 07:43_

---
