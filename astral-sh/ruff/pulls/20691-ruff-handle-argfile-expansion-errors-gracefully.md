```yaml
number: 20691
title: "[`ruff`] Handle argfile expansion errors gracefully"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: fix-argfile-expansion-error
created_at: 2025-10-03T12:30:15Z
updated_at: 2025-10-03T13:42:57Z
url: https://github.com/astral-sh/ruff/pull/20691
synced_at: 2026-01-10T17:40:28Z
```

# [`ruff`] Handle argfile expansion errors gracefully

---

_Pull request opened by @TaKO8Ki on 2025-10-03 12:30_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20655

- Guard `argfile::expand_args_from` with contextual error handling so missing @file arguments surface a friendly failure instead of panicking.
- Extract existing stderr reporting into `report_error` for reuse on both CLI parsing failures and runtime errors.

## Test Plan

<!-- How was it tested? -->

Add a regression test to integration_test.rs.


---

_Comment by @github-actions[bot] on 2025-10-03 12:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-10-03 13:17_

---

_Label `cli` added by @ntBre on 2025-10-03 13:17_

---

_Review comment by @ntBre on `crates/ruff/src/main.rs`:44 on 2025-10-03 13:30_

nit: I think we can omit this and call `context` directly on the other error:


```suggestion
```

---

_@ntBre approved on 2025-10-03 13:31_

This looks good to me, thank you!

I just had one tiny nit that I'll try to apply and merge.

---

_Merged by @ntBre on 2025-10-03 13:36_

---

_Closed by @ntBre on 2025-10-03 13:36_

---

_Branch deleted on 2025-10-03 13:42_

---
