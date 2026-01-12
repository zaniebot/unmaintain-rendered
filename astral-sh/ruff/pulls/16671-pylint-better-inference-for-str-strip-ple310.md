```yaml
number: 16671
title: "[`pylint`] Better inference for `str.strip` (`PLE310`)"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: micha/ruff-0.10
head: micha/pylint-str-strip-non-literals
created_at: 2025-03-12T12:15:53Z
updated_at: 2025-03-13T08:37:05Z
url: https://github.com/astral-sh/ruff/pull/16671
synced_at: 2026-01-12T15:55:55Z
```

# [`pylint`] Better inference for `str.strip` (`PLE310`)

---

_@MichaReiser_

## Summary
This PR stabilizes the behavior introduced in https://github.com/astral-sh/ruff/pull/15985

The new behavior improves the inference of `str.strip` calls:

* before: The rule only considered calls on string or byte literals (`"abcd".strip`)
* now: The rule also catches calls to `strip` on object where the type is known to be a `str` or `bytes` (e.g. `a = "abc"; a.strip("//")`)


The new behavior shipped as part of Ruff 0.9.6 on the 10th of Feb which is a little more than a month ago. 
There have been now new issues or PRs related to the new behavior.



---

_Label `rule` added by @MichaReiser on 2025-03-12 12:16_

---

_Review requested from @ntBre by @MichaReiser on 2025-03-12 12:17_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 12:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Fpylint-str-strip-non-literals)

### Merging #16671 will **degrade performances by 4.61%**

<sub>Comparing <code>micha/pylint-str-strip-non-literals</code> (0561c94) with <code>micha/ruff-0.10</code> (6fc3434)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/micha%2Fpylint-str-strip-non-literals)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.61% |


---

_Comment by @github-actions[bot] on 2025-03-12 12:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-03-12 12:27_

Closing and reopening to trigger another ecosystem run

---

_Closed by @MichaReiser on 2025-03-12 12:27_

---

_Reopened by @MichaReiser on 2025-03-12 12:27_

---

_Added to milestone `v0.10` by @MichaReiser on 2025-03-12 12:48_

---

_@ntBre approved on 2025-03-12 16:38_

---

_Merged by @MichaReiser on 2025-03-13 08:35_

---

_Closed by @MichaReiser on 2025-03-13 08:35_

---

_Branch deleted on 2025-03-13 08:35_

---
