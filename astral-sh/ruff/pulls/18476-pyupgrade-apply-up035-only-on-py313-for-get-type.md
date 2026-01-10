```yaml
number: 18476
title: "[`pyupgrade`] Apply `UP035` only on py313+ for `get_type_hints()`"
type: pull_request
state: merged
author: Viicos
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: up035-get-type-hints
created_at: 2025-06-05T11:06:30Z
updated_at: 2025-06-05T16:16:37Z
url: https://github.com/astral-sh/ruff/pull/18476
synced_at: 2026-01-10T18:45:04Z
```

# [`pyupgrade`] Apply `UP035` only on py313+ for `get_type_hints()`

---

_Pull request opened by @Viicos on 2025-06-05 11:06_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

typing-extensions backports changes on <py3.13.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-05 11:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2025-06-05 11:35_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP035.py.snap`:1197 on 2025-06-05 11:35_

This error message is off: you'd want to import it from typing rather than types on 3.13+ (unlike CapsuleType!)

---

_@Viicos reviewed on 2025-06-05 13:12_

---

_Review comment by @Viicos on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP035.py.snap`:1197 on 2025-06-05 13:12_

Ah yes, fixed

---

_@AlexWaygood approved on 2025-06-05 16:16_

---

_Merged by @AlexWaygood on 2025-06-05 16:16_

---

_Closed by @AlexWaygood on 2025-06-05 16:16_

---

_Label `bug` added by @AlexWaygood on 2025-06-05 16:16_

---

_Label `rule` added by @AlexWaygood on 2025-06-05 16:16_

---
