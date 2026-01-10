```yaml
number: 18792
title: "[`flake8-pytest-style`] Mark autofix for `PT001` and `PT023` as unsafe if there's comments in the decorator"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-PT001
created_at: 2025-06-19T13:42:44Z
updated_at: 2025-06-20T13:50:33Z
url: https://github.com/astral-sh/ruff/pull/18792
synced_at: 2026-01-10T18:39:08Z
```

# [`flake8-pytest-style`] Mark autofix for `PT001` and `PT023` as unsafe if there's comments in the decorator

---

_Pull request opened by @LaBatata101 on 2025-06-19 13:42_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
Fixes #18770
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Add regression test
<!-- How was it tested? -->


---

_@MichaReiser reviewed on 2025-06-19 13:56_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:723 on 2025-06-19 13:56_

Could we limit the range to `func.end()..decorator.end()`?

---

_Comment by @github-actions[bot] on 2025-06-19 14:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@LaBatata101 reviewed on 2025-06-19 14:50_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_pytest_style/rules/fixture.rs`:723 on 2025-06-19 14:50_

Yep

---

_Review requested from @MichaReiser by @LaBatata101 on 2025-06-19 14:50_

---

_@MichaReiser approved on 2025-06-20 06:23_

Thank you

---

_Label `bug` added by @MichaReiser on 2025-06-20 06:23_

---

_Label `fixes` added by @MichaReiser on 2025-06-20 06:23_

---

_Merged by @MichaReiser on 2025-06-20 06:23_

---

_Closed by @MichaReiser on 2025-06-20 06:23_

---

_Branch deleted on 2025-06-20 13:50_

---
