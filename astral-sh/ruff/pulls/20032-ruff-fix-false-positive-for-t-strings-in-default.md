```yaml
number: 20032
title: "[`ruff`] Fix false positive for t-strings in `default-factory-kwarg` (`RUF026`)"
type: pull_request
state: merged
author: maxmynter
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: ruf026
created_at: 2025-08-21T19:40:12Z
updated_at: 2025-08-22T14:29:43Z
url: https://github.com/astral-sh/ruff/pull/20032
synced_at: 2026-01-10T17:46:21Z
```

# [`ruff`] Fix false positive for t-strings in `default-factory-kwarg` (`RUF026`)

---

_Pull request opened by @maxmynter on 2025-08-21 19:40_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

Closes #19993

## Summary
Recognize t strings as never being callable to avoid false positives on RUF026.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Added two test cases covering t strings. 
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-08-21 19:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-08-22 14:27_

Perfect, thank you!

---

_Renamed from "RUF026: Fix false positive for t strings" to "[`ruff`] Fix false positive for t-strings in `default-factory-kwarg` (`RUF026`)" by @dylwil3 on 2025-08-22 14:29_

---

_Label `bug` added by @dylwil3 on 2025-08-22 14:29_

---

_Label `rule` added by @dylwil3 on 2025-08-22 14:29_

---

_Merged by @dylwil3 on 2025-08-22 14:29_

---

_Closed by @dylwil3 on 2025-08-22 14:29_

---
