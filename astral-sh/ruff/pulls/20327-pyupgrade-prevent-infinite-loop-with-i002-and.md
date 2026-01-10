```yaml
number: 20327
title: "[`pyupgrade`] Prevent infinite loop with `I002` and `UP026`"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
assignees: []
merged: true
base: main
head: UP026-Infinite-loop
created_at: 2025-09-10T10:57:09Z
updated_at: 2025-09-12T20:51:13Z
url: https://github.com/astral-sh/ruff/pull/20327
synced_at: 2026-01-10T17:40:28Z
```

# [`pyupgrade`] Prevent infinite loop with `I002` and `UP026`

---

_Pull request opened by @TaKO8Ki on 2025-09-10 10:57_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #19842

Prevent infinite loop with I002 and UP026

- Implement isort-aware handling for UP026 (deprecated mock import):
- Add CLI integration tests in crates/ruff/tests/lint.rs:

## Test Plan

<!-- How was it tested? -->

I have added two integration tests `pyupgrade_up026_respects_isort_required_import_fix` and `pyupgrade_up026_respects_isort_required_import_from_fix` in `crates/ruff/tests/lint.rs`.


---

_Comment by @github-actions[bot] on 2025-09-10 11:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff/tests/lint.rs`:5077 on 2025-09-12 14:37_

Can you recreate the example from the issue here and pass in an empty file (or I guess a file just containing the literal `1`)?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_mock_import.rs`:310 on 2025-09-12 14:41_

Let's combine these `if` statements and have the name checks come first so that the `is_import_required_by_isort` is executed more rarely.

---

_@dylwil3 requested changes on 2025-09-12 14:41_

Looks good, just a few nits - thank you!

---

_Review comment by @TaKO8Ki on `crates/ruff/tests/lint.rs`:5077 on 2025-09-12 20:41_

I've updated it based on the example in the issue.

---

_@TaKO8Ki reviewed on 2025-09-12 20:41_

---

_@TaKO8Ki reviewed on 2025-09-12 20:41_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/pyupgrade/rules/deprecated_mock_import.rs`:310 on 2025-09-12 20:41_

I've combined them into one.

---

_@dylwil3 approved on 2025-09-12 20:44_

Thank you!

---

_Label `bug` added by @dylwil3 on 2025-09-12 20:44_

---

_Renamed from "[`pyupgrade`] Prevent infinite loop with I002 and UP026" to "[`pyupgrade`] Prevent infinite loop with `I002` and `UP026`" by @dylwil3 on 2025-09-12 20:44_

---

_Merged by @dylwil3 on 2025-09-12 20:45_

---

_Closed by @dylwil3 on 2025-09-12 20:45_

---
