```yaml
number: 7922
title: fix edge case with PIE804
type: pull_request
state: merged
author: diceroll123
labels: []
assignees: []
merged: true
base: main
head: PIE804-autofix-hotfix
created_at: 2023-10-11T18:51:55Z
updated_at: 2023-10-11T19:09:15Z
url: https://github.com/astral-sh/ruff/pull/7922
synced_at: 2026-01-12T02:32:41Z
```

# fix edge case with PIE804

---

_Pull request opened by @diceroll123 on 2023-10-11 18:51_

## Summary

`foo(**{})` was an overlooked edge case for `PIE804` which introduced a crash within the Fix, introduced in #7884.

I've made it so that `foo(**{})` turns into `foo()` when applied with `--fix`, but is that desired/expected? 🤔 Should we just ignore instead?

## Test Plan

`cargo test`

---

_@charliermarsh approved on 2023-10-11 19:05_

---

_Comment by @charliermarsh on 2023-10-11 19:05_

Thanks!

---

_Merged by @charliermarsh on 2023-10-11 19:05_

---

_Closed by @charliermarsh on 2023-10-11 19:05_

---

_Comment by @github-actions[bot] on 2023-10-11 19:09_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
