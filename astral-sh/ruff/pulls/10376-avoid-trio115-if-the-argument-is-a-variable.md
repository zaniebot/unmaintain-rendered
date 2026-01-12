```yaml
number: 10376
title: "Avoid `TRIO115` if the argument is a variable"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
assignees: []
merged: true
base: main
head: trio115-false-positive
created_at: 2024-03-13T04:30:05Z
updated_at: 2024-03-13T13:40:36Z
url: https://github.com/astral-sh/ruff/pull/10376
synced_at: 2026-01-12T15:55:32Z
```

# Avoid `TRIO115` if the argument is a variable

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix "TRIO115 false positive with with sleep(var) where var starts as 0" #9935 based on the discussion in the issue.

## Test Plan

Issue code added to fixture


---

_Comment by @github-actions[bot] on 2024-03-13 04:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-03-13 07:37_

Thanks!

---

_Label `bug` added by @dhruvmanila on 2024-03-13 07:38_

---

_Renamed from "Fix TRIO115 false positive #9935" to "Avoid `TRIO115` if the argument is a variable" by @dhruvmanila on 2024-03-13 07:38_

---

_Merged by @dhruvmanila on 2024-03-13 07:39_

---

_Closed by @dhruvmanila on 2024-03-13 07:39_

---

_Branch deleted on 2024-03-13 13:40_

---
