```yaml
number: 8101
title: Fix range of unparenthesized tuple subject in match statement
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/match
created_at: 2023-10-21T02:55:11Z
updated_at: 2023-10-22T23:58:34Z
url: https://github.com/astral-sh/ruff/pull/8101
synced_at: 2026-01-12T02:32:41Z
```

# Fix range of unparenthesized tuple subject in match statement

---

_Pull request opened by @charliermarsh on 2023-10-21 02:55_

## Summary

This was just a bug in the parser ranges, probably since it was initially implemented. Given `match n % 3, n % 5: ...`, the "subject" (i.e., the tuple of two binary operators) was using the entire range of the `match` statement.

Closes https://github.com/astral-sh/ruff/issues/8091.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-10-21 02:55_

---

_Review requested from @konstin by @charliermarsh on 2023-10-21 02:55_

---

_Label `bug` added by @charliermarsh on 2023-10-21 02:55_

---

_Label `formatter` added by @charliermarsh on 2023-10-21 02:55_

---

_Comment by @github-actions[bot] on 2023-10-21 03:12_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@MichaReiser approved on 2023-10-22 23:47_

---

_Merged by @charliermarsh on 2023-10-22 23:58_

---

_Closed by @charliermarsh on 2023-10-22 23:58_

---

_Branch deleted on 2023-10-22 23:58_

---
