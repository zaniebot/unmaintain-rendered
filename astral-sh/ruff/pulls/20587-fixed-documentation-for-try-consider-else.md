```yaml
number: 20587
title: Fixed documentation for try_consider_else
type: pull_request
state: merged
author: LilMonk
labels:
  - documentation
assignees: []
merged: true
base: main
head: lilmonk/patch-doc-20570
created_at: 2025-09-26T01:41:30Z
updated_at: 2025-09-27T13:55:17Z
url: https://github.com/astral-sh/ruff/pull/20587
synced_at: 2026-01-10T17:40:28Z
```

# Fixed documentation for try_consider_else

---

_Pull request opened by @LilMonk on 2025-09-26 01:41_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR addresses #20570 . In the example, the correct usage had a bug/issue where in the except block after logging exception, None was getting returned, which made the linters flag out the code. So adding an empty raise solves the issue.

## Test Plan

Tested it by building the doc locally.


---

_@ntBre reviewed on 2025-09-26 15:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/tryceratops/rules/try_consider_else.rs`:45 on 2025-09-26 15:42_

I think we should add this to the example before the fix too.

---

_@LilMonk reviewed on 2025-09-27 08:55_

---

_Review comment by @LilMonk on `crates/ruff_linter/src/rules/tryceratops/rules/try_consider_else.rs`:45 on 2025-09-27 08:55_

Done.

---

_Review requested from @ntBre by @MichaReiser on 2025-09-27 09:11_

---

_@ntBre approved on 2025-09-27 13:46_

Thank you!

---

_Label `documentation` added by @ntBre on 2025-09-27 13:46_

---

_Merged by @ntBre on 2025-09-27 13:50_

---

_Closed by @ntBre on 2025-09-27 13:50_

---

_Comment by @github-actions[bot] on 2025-09-27 13:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
