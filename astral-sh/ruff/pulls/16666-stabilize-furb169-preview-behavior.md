```yaml
number: 16666
title: Stabilize FURB169 preview behavior
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/type-non-comparison-other-expressions
created_at: 2025-03-12T09:34:53Z
updated_at: 2025-03-13T07:43:32Z
url: https://github.com/astral-sh/ruff/pull/16666
synced_at: 2026-01-10T19:49:02Z
```

# Stabilize FURB169 preview behavior

---

_Pull request opened by @MichaReiser on 2025-03-12 09:34_

## Summary

This PR stabilizes the preview behavior introduced in https://github.com/astral-sh/ruff/pull/15905

The behavior change is that the rule now also recognizes `type(expr) is type(None)` comparisons where `expr` isn't a name expression. 
For example, the rule now detects `type(a.b) is type(None)` and suggests rewriting the comparison to `a.b is None`.

The new behavior was introduced with Ruff 0.9.5 (6th of February), about a month ago. There are no open issues or PRs related to this rule (or behavior change).




---

_Label `rule` added by @MichaReiser on 2025-03-12 09:35_

---

_@MichaReiser reviewed on 2025-03-12 09:35_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/mod.rs`:65 on 2025-03-12 09:35_

The same file is used for the non-preview tests, which is why it's safe to remove it from here.

---

_Comment by @codspeed-hq[bot] on 2025-03-12 09:39_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftype-non-comparison-other-expressions)

### Merging #16666 will **degrade performances by 10.81%**

<sub>Comparing <code>micha/type-non-comparison-other-expressions</code> (40dd5ee) with <code>micha/ruff-0.10</code> (b8f1284)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Ftype-non-comparison-other-expressions)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.81% |


---

_Comment by @github-actions[bot] on 2025-03-12 09:41_

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

_@ntBre approved on 2025-03-12 18:04_

---

_Merged by @MichaReiser on 2025-03-13 07:43_

---

_Closed by @MichaReiser on 2025-03-13 07:43_

---

_Branch deleted on 2025-03-13 07:43_

---
