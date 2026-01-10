```yaml
number: 16316
title: Fix example for S611
type: pull_request
state: merged
author: aripollak
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-02-22T17:14:22Z
updated_at: 2025-02-22T19:15:30Z
url: https://github.com/astral-sh/ruff/pull/16316
synced_at: 2026-01-10T19:49:01Z
```

# Fix example for S611

---

_Pull request opened by @aripollak on 2025-02-22 17:14_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

* Existing example did not include RawSQL() call like it should
* Also clarify the example a bit to make it clearer that the code is not secure
## Test Plan

N/A, only documentation updated

---

_Comment by @github-actions[bot] on 2025-02-22 17:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-02-22 19:07_

---

_@ntBre approved on 2025-02-22 19:09_

Thanks! The example matches the rule tests now, which is great.

---

_Merged by @ntBre on 2025-02-22 19:15_

---

_Closed by @ntBre on 2025-02-22 19:15_

---
