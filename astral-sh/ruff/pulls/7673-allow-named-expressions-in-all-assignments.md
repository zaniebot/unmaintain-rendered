```yaml
number: 7673
title: "Allow named expressions in `__all__` assignments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/all
created_at: 2023-09-27T04:17:11Z
updated_at: 2023-09-27T04:37:05Z
url: https://github.com/astral-sh/ruff/pull/7673
synced_at: 2026-01-12T15:55:24Z
```

# Allow named expressions in `__all__` assignments

---

_@charliermarsh_

## Summary

This PR adds support for named expressions when analyzing `__all__` assignments, as per https://github.com/astral-sh/ruff/issues/7672. It also loosens the enforcement around assignments like: `__all__ = list(some_other_expression)`. We shouldn't flag these as invalid, even though we can't analyze the members, since we _know_ they evaluate to a `list`.

Closes https://github.com/astral-sh/ruff/issues/7672.

## Test Plan

`cargo test`


---

_Label `bug` added by @charliermarsh on 2023-09-27 04:17_

---

_Review requested from @zanieb by @charliermarsh on 2023-09-27 04:17_

---

_Comment by @github-actions[bot] on 2023-09-27 04:33_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review request for @zanieb removed by @charliermarsh on 2023-09-27 04:36_

---

_Merged by @charliermarsh on 2023-09-27 04:36_

---

_Closed by @charliermarsh on 2023-09-27 04:36_

---

_Branch deleted on 2023-09-27 04:36_

---

_Comment by @charliermarsh on 2023-09-27 04:37_

Actually, small enough that I'm ok merging without review.

---
