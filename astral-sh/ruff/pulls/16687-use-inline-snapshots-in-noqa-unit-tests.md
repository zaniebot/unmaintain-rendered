```yaml
number: 16687
title: "Use inline snapshots in `# noqa` unit tests"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - testing
assignees: []
merged: true
base: micha/ruff-0.10
head: noqa
created_at: 2025-03-12T17:41:23Z
updated_at: 2025-03-12T18:18:50Z
url: https://github.com/astral-sh/ruff/pull/16687
synced_at: 2026-01-12T15:55:56Z
```

# Use inline snapshots in `# noqa` unit tests

---

_@InSyncWithFoo_

## Summary

Follow-up to #16677.

This change converts all unit tests (69 of them) in `noqa.rs` to use inline snapshots instead. It extends the file by more than 1000 lines, but the tests are now much easier to read and reason about.

## Test Plan

`cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-03-12 17:41_

(It was painful switching back and forth to find the corresponding snapshot files.)

---

_Comment by @codspeed-hq[bot] on 2025-03-12 17:46_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo%3Anoqa)

### Merging #16687 will **degrade performances by 4.6%**

<sub>Comparing <code>InSyncWithFoo:noqa</code> (d2c9f74) with <code>micha/ruff-0.10</code> (7bc8bb2)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo%3Anoqa)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 5.2 ms | 5.5 ms | -4.6% |


---

_Comment by @github-actions[bot] on 2025-03-12 17:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-03-12 18:02_

---

_Label `testing` added by @MichaReiser on 2025-03-12 18:02_

---

_@dylwil3 approved on 2025-03-12 18:16_

Love it, thank you!

---

_Merged by @dylwil3 on 2025-03-12 18:16_

---

_Closed by @dylwil3 on 2025-03-12 18:16_

---

_Branch deleted on 2025-03-12 18:18_

---
