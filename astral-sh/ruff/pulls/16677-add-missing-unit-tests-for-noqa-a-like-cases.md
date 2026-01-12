```yaml
number: 16677
title: "Add missing unit tests for `# noqa: A`-like cases"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - testing
assignees: []
merged: true
base: micha/ruff-0.10
head: noqa
created_at: 2025-03-12T14:07:52Z
updated_at: 2025-03-12T17:12:46Z
url: https://github.com/astral-sh/ruff/pull/16677
synced_at: 2026-01-12T15:55:55Z
```

# Add missing unit tests for `# noqa: A`-like cases

---

_@InSyncWithFoo_

## Summary

Follow-up to #16659.

This change adds tests for these three cases, which are (also) not covered by existing tests:

* `# noqa: A` (lone incomplete code)
* `# noqa: A123, B` (complete codes, last one incomplete)
* `# noqa: A123B` (squashed codes, last one incomplete)

The third case is mentioned in [the specification](https://github.com/astral-sh/ruff/pull/16483#issue-2892361457), but neither of the other two are:

> If an item does not consist entirely of codes (e.g. `F401abc`), parsing aborts with an error and the user is warned of an `InvalidCodeSuffix` [...]

## Test Plan

Unit tests.


---

_Comment by @codspeed-hq[bot] on 2025-03-12 14:12_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo%3Anoqa)

### Merging #16677 will **degrade performances by 10.49%**

<sub>Comparing <code>InSyncWithFoo:noqa</code> (f644fbb) with <code>micha/ruff-0.10</code> (e9bfdfd)</sub>



### Summary

`❌ 1` regressions  
`✅ 31` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/InSyncWithFoo%3Anoqa)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` red_knot_check_file[incremental] `` | 4.9 ms | 5.5 ms | -10.49% |


---

_Comment by @github-actions[bot] on 2025-03-12 14:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-03-12 14:16_

---

_Label `testing` added by @MichaReiser on 2025-03-12 14:16_

---

_Comment by @dylwil3 on 2025-03-12 14:38_

Thanks!

>The third case is mentioned in https://github.com/astral-sh/ruff/pull/16483#issue-2892361457, but neither of the other two are

These cases are covered by this part of the spec:

>We expect a list of rule codes, separated by commas or whitespace. After splitting first on commas, and then on whitespace, we attempt to parse each item in turn. If parsing a nonempty item returns no rule codes, we stop. If no codes were found at all we warn the user with MissingCodes.

e.g. `A` is not a valid code, so we stop. There were no preceding codes, so we return `MissingCodes`, etc.

---

_@dylwil3 approved on 2025-03-12 14:38_

---

_Merged by @dylwil3 on 2025-03-12 14:40_

---

_Closed by @dylwil3 on 2025-03-12 14:40_

---

_Branch deleted on 2025-03-12 17:12_

---
