```yaml
number: 9021
title: Enforce valid format options in spec tests
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: enforce-valid-format-options
created_at: 2023-12-06T07:03:56Z
updated_at: 2023-12-06T07:20:39Z
url: https://github.com/astral-sh/ruff/pull/9021
synced_at: 2026-01-12T15:55:27Z
```

# Enforce valid format options in spec tests

---

_@MichaReiser_

## Summary

Makes `serde` fail when there's a unknown `PyFormatOptions` in a spec `.options.json` file. 

This revealed a few tests that used incorred settings. 

I re-ran the import script, which is why we now get a few updated tests.

## Test Plan

Running the tests revealed some tests with invalid options, proofing the point that it now works as expected.


---

_Comment by @MichaReiser on 2023-12-06 07:04_

Current dependencies on/for this PR:
* main
  * **PR #9021** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9021?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
    * **PR #9020** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9020?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/9021?utm_source=stack-comment).

---

_@MichaReiser reviewed on 2023-12-06 07:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/import_black_tests.py`:52 on 2023-12-06 07:04_

0 isn't a valid option, so the closest we can do is to use 1.

---

_@MichaReiser reviewed on 2023-12-06 07:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__power_op_newline.py.snap`:21 on 2023-12-06 07:04_

This is fixed in preview style.

---

_Label `internal` added by @MichaReiser on 2023-12-06 07:05_

---

_Review requested from @konstin by @MichaReiser on 2023-12-06 07:05_

---

_Review request for @konstin removed by @MichaReiser on 2023-12-06 07:12_

---

_Merged by @MichaReiser on 2023-12-06 07:15_

---

_Closed by @MichaReiser on 2023-12-06 07:15_

---

_Branch deleted on 2023-12-06 07:15_

---

_Comment by @github-actions[bot] on 2023-12-06 07:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
