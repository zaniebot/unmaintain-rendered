```yaml
number: 20249
title: "[`ruff`] Use helper function for empty f-string detection in `in-empty-collection` (`RUF060`)"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: ruf060-use-helper-function
created_at: 2025-09-04T19:13:22Z
updated_at: 2025-09-04T20:28:55Z
url: https://github.com/astral-sh/ruff/pull/20249
synced_at: 2026-01-10T17:46:21Z
```

# [`ruff`] Use helper function for empty f-string detection in `in-empty-collection` (`RUF060`)

---

_Pull request opened by @TaKO8Ki on 2025-09-04 19:13_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20238

Replace inline f-string emptiness check with `is_empty_f_string` helper function

## Test Plan

<!-- How was it tested? -->

I have added test cases to `crates/ruff_linter/resources/test/fixtures/ruff/RUF060.py`

---

_Comment by @github-actions[bot] on 2025-09-04 19:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 requested changes on 2025-09-04 20:09_

Can you move these new examples to the end of the file and put a link to the relevant GitHub issue in a comment above them?

---

_Label `bug` added by @ntBre on 2025-09-04 20:11_

---

_Renamed from "[`RUF060`] Use helper function for empty f-string detection" to "[`ruff`] Use helper function for empty f-string detection in `in-empty-collection` (`RUF060`)" by @dylwil3 on 2025-09-04 20:13_

---

_Label `rule` added by @dylwil3 on 2025-09-04 20:13_

---

_Label `preview` added by @dylwil3 on 2025-09-04 20:18_

---

_@dylwil3 approved on 2025-09-04 20:18_

Thank you!

---

_Merged by @dylwil3 on 2025-09-04 20:20_

---

_Closed by @dylwil3 on 2025-09-04 20:21_

---

_Branch deleted on 2025-09-04 20:21_

---
