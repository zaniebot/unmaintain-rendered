```yaml
number: 14870
title: "[`flake8-bugbear`] Skip `B028` if `warnings.warn` is called with `*args` or `**kwargs`"
type: pull_request
state: merged
author: harupy
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-B028-args-kwargs
created_at: 2024-12-09T13:20:51Z
updated_at: 2024-12-09T14:50:58Z
url: https://github.com/astral-sh/ruff/pull/14870
synced_at: 2026-01-12T15:55:49Z
```

# [`flake8-bugbear`] Skip `B028` if `warnings.warn` is called with `*args` or `**kwargs`

---

_@harupy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Applies https://github.com/PyCQA/flake8-bugbear/pull/501 to ruff. `B028` should be skipped if `warnings.warn` is called with `*args` or `**kwargs`.

## Test Plan

<!-- How was it tested? -->

New test cases

---

_Comment by @github-actions[bot] on 2024-12-09 13:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @AlexWaygood on 2024-12-09 14:32_

---

_@AlexWaygood approved on 2024-12-09 14:32_

---

_Merged by @AlexWaygood on 2024-12-09 14:32_

---

_Closed by @AlexWaygood on 2024-12-09 14:32_

---

_Branch deleted on 2024-12-09 14:50_

---
