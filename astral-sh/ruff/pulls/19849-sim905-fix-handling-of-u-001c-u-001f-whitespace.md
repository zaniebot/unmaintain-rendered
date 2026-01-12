```yaml
number: 19849
title: "SIM905: Fix handling of U+001C..U+001F whitespace"
type: pull_request
state: merged
author: RazerM
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/split-static-string-py-whitespace
created_at: 2025-08-10T21:05:21Z
updated_at: 2025-08-14T15:34:50Z
url: https://github.com/astral-sh/ruff/pull/19849
synced_at: 2026-01-12T15:56:48Z
```

# SIM905: Fix handling of U+001C..U+001F whitespace

---

_@RazerM_

Fixes #19845

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

The linked issue explains it well, Rust and Python do not agree on what whitespace is.

## Test Plan

Regression test added

---

_Comment by @github-actions[bot] on 2025-08-10 21:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-08-11 03:40_

Thank you, very nice!

---

_Merged by @dylwil3 on 2025-08-11 03:43_

---

_Closed by @dylwil3 on 2025-08-11 03:43_

---

_Branch deleted on 2025-08-11 08:40_

---

_Label `bug` added by @ntBre on 2025-08-14 15:34_

---
