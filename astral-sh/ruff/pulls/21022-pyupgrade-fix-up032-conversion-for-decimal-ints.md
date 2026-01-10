```yaml
number: 21022
title: "[`pyupgrade`] Fix `UP032` conversion for decimal ints with underscores"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-up032-decimal-underscores
created_at: 2025-10-21T16:54:01Z
updated_at: 2025-10-22T22:12:16Z
url: https://github.com/astral-sh/ruff/pull/21022
synced_at: 2026-01-10T17:34:34Z
```

# [`pyupgrade`] Fix `UP032` conversion for decimal ints with underscores

---

_Pull request opened by @TaKO8Ki on 2025-10-21 16:54_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #21017

Taught UP032’s parenthesize check to ignore underscores when inspecting decimal integer literals so the converter emits `f"{(1_2).real}"` instead of invalid syntax.

## Test Plan

Added test cases to UP032_2.py.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-10-21 17:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@amyreese approved on 2025-10-22 02:07_

---

_Label `bug` added by @amyreese on 2025-10-22 02:07_

---

_Label `rule` added by @amyreese on 2025-10-22 02:07_

---

_Label `fixes` added by @amyreese on 2025-10-22 02:07_

---

_Label `rule` removed by @amyreese on 2025-10-22 02:07_

---

_Review requested from @ntBre by @amyreese on 2025-10-22 02:07_

---

_@ntBre approved on 2025-10-22 22:11_

Thank you!

---

_Merged by @ntBre on 2025-10-22 22:11_

---

_Closed by @ntBre on 2025-10-22 22:11_

---

_Renamed from "[`ruff`] Fix UP032 conversion for decimal ints with underscores" to "[`pyupgrade`] Fix `UP032` conversion for decimal ints with underscores" by @ntBre on 2025-10-22 22:12_

---
