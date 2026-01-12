```yaml
number: 7024
title: Improve compatibility between multi-statement PYI rules
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/consistent
created_at: 2023-08-31T15:14:45Z
updated_at: 2023-08-31T20:45:27Z
url: https://github.com/astral-sh/ruff/pull/7024
synced_at: 2026-01-12T02:45:38Z
```

# Improve compatibility between multi-statement PYI rules

---

_Pull request opened by @charliermarsh on 2023-08-31 15:14_

## Summary

This PR modifies a few of our rules related to which statements (and how many) are allowed in function bodies within `.pyi` files, to improve compatibility with flake8-pyi and improve the interplay dynamics between them. Each change fixes a deviation from flake8-pyi:

- We now always trigger the multi-statement rule (PYI048) regardless of whether one of the statements is a docstring.
- We no longer trigger the `...` rule (PYI010) if the single statement is a docstring or a `pass` (since those are covered by other rules).
- We no longer trigger the `...` rule (PYI010) if the function body contains multiple statements (since that's covered by PYI048).

Closes https://github.com/astral-sh/ruff/issues/7021.

## Test Plan

`cargo test`


---

_Marked ready for review by @charliermarsh on 2023-08-31 15:14_

---

_Label `bug` added by @charliermarsh on 2023-08-31 15:14_

---

_Review requested from @zanieb by @charliermarsh on 2023-08-31 15:14_

---

_Comment by @github-actions[bot] on 2023-08-31 15:29_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb approved on 2023-08-31 17:33_

---

_Merged by @charliermarsh on 2023-08-31 20:45_

---

_Closed by @charliermarsh on 2023-08-31 20:45_

---

_Branch deleted on 2023-08-31 20:45_

---
