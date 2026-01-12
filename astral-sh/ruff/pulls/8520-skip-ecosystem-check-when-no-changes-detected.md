```yaml
number: 8520
title: Skip ecosystem check when no changes detected
type: pull_request
state: merged
author: T-256
labels:
  - internal
assignees: []
merged: true
base: main
head: patch-4
created_at: 2023-11-06T17:32:26Z
updated_at: 2023-11-06T19:21:58Z
url: https://github.com/astral-sh/ruff/pull/8520
synced_at: 2026-01-10T23:40:55Z
```

# Skip ecosystem check when no changes detected

---

_Pull request opened by @T-256 on 2023-11-06 17:32_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

For example, https://github.com/astral-sh/ruff/pull/8512 doesn't need ecosystem check
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_@zanieb approved on 2023-11-06 17:51_

Hm okay, I'm not sure saving the 50s is critical but why not :) thanks!

---

_Comment by @zanieb on 2023-11-06 18:00_

@T-256 it doesn't show up until after its `needs` jobs complete.

All of these checks are expected to run when the workflow itself is modified.

---

_Comment by @github-actions[bot] on 2023-11-06 18:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @zanieb on 2023-11-06 18:18_

---

_Closed by @zanieb on 2023-11-06 18:18_

---

_Label `internal` added by @zanieb on 2023-11-06 18:18_

---

_Branch deleted on 2023-11-06 19:21_

---
