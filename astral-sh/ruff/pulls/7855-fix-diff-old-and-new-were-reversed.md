```yaml
number: 7855
title: Fix diff (old and new were reversed)
type: pull_request
state: merged
author: bluthej
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-diff
created_at: 2023-10-09T05:38:46Z
updated_at: 2023-11-26T16:26:04Z
url: https://github.com/astral-sh/ruff/pull/7855
synced_at: 2026-01-12T15:55:25Z
```

# Fix diff (old and new were reversed)

---

_@bluthej_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Fixes #7853.

The old and new source files were reversed in the call to `TextDiff::from_lines`, so the diff output of the CLI was also reversed.

## Test Plan

<!-- How was it tested? -->

Two snapshots were updated in the process, so any reversal should be caught :)

---

_Comment by @github-actions[bot] on 2023-10-09 05:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@dhruvmanila approved on 2023-10-09 07:26_

Thanks!

---

_Label `bug` added by @dhruvmanila on 2023-10-09 07:27_

---

_Merged by @dhruvmanila on 2023-10-09 07:28_

---

_Closed by @dhruvmanila on 2023-10-09 07:28_

---

_Branch deleted on 2023-11-26 16:26_

---
