```yaml
number: 8665
title: "Fix ordering for `force-sort-within-sections`"
type: pull_request
state: merged
author: bluthej
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: fix-force-sort-within-sections
created_at: 2023-11-13T22:16:47Z
updated_at: 2023-11-14T06:46:29Z
url: https://github.com/astral-sh/ruff/pull/8665
synced_at: 2026-01-10T23:40:55Z
```

# Fix ordering for `force-sort-within-sections`

---

_Pull request opened by @bluthej on 2023-11-13 22:16_

Fixes #8661 

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Imports like `from x import y` don't have an "asname" for the module, so they were placed before imports like `import x as w` since `None` < `Some(s)` for any string s.
The fix is to first sort by `first_alias`, since it's `None` for `import x as w`, and then by `asname`.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I included the example from the issue to avoid future regressions.


---

_Comment by @github-actions[bot] on 2023-11-13 22:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-11-13 23:27_

Thanks!

---

_Merged by @charliermarsh on 2023-11-13 23:27_

---

_Closed by @charliermarsh on 2023-11-13 23:27_

---

_Label `bug` added by @charliermarsh on 2023-11-13 23:28_

---

_Label `isort` added by @charliermarsh on 2023-11-13 23:28_

---

_Comment by @charliermarsh on 2023-11-13 23:28_

I really appreciate the quick investigation and turnaround here @bluthej.

---

_Comment by @bluthej on 2023-11-14 06:46_

Well the regression was due to my refactor so that was the least I could do 😅
Plus it was an easy fix ^^

---
