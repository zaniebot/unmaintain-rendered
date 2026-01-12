```yaml
number: 16659
title: "Add missing unit tests for `# noqa:`-like cases"
type: pull_request
state: merged
author: InSyncWithFoo
labels: []
assignees: []
merged: true
base: micha/ruff-0.10
head: noqa
created_at: 2025-03-12T04:03:47Z
updated_at: 2025-03-12T13:19:14Z
url: https://github.com/astral-sh/ruff/pull/16659
synced_at: 2026-01-12T15:55:55Z
```

# Add missing unit tests for `# noqa:`-like cases

---

_@InSyncWithFoo_

## Summary

Follow-up to #16483.

This case is explicitly described in [the specification](https://github.com/astral-sh/ruff/pull/16483#issue-2892361457), but it is not covered by any tests:

> We expect a list of rule codes, separated by commas or whitespace. After splitting first on commas, and then on whitespace, we attempt to parse each item in turn. If parsing a nonempty item returns no rule codes, we stop. If no codes were found at all we warn the user with `MissingCodes`.

## Test Plan

Unit tests.

---

_Renamed from "[red-knot] mypy_primer: comment on PRs (#16599)" to "Add missing unit tests for `# noqa:`-like cases" by @InSyncWithFoo on 2025-03-12 04:06_

---

_Comment by @codspeed-hq[bot] on 2025-03-12 04:08_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo%3Anoqa)

### Merging #16659 will **degrade performances by 10.81%**

<sub>Comparing <code>InSyncWithFoo:noqa</code> (7e7add7) with <code>micha/ruff-0.10</code> (b8f1284)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo%3Anoqa)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.81% |


---

_Comment by @github-actions[bot] on 2025-03-12 04:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-03-12 07:16_

---

_Assigned to @dylwil3 by @MichaReiser on 2025-03-12 07:16_

---

_@dylwil3 approved on 2025-03-12 10:07_

Thanks! Glad they passed 😄

---

_Merged by @dylwil3 on 2025-03-12 10:08_

---

_Closed by @dylwil3 on 2025-03-12 10:08_

---

_Branch deleted on 2025-03-12 13:19_

---
