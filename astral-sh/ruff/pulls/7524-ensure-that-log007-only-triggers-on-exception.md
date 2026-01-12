```yaml
number: 7524
title: "Ensure that LOG007 only triggers on `.exception()` calls"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/LOG007
created_at: 2023-09-19T17:19:06Z
updated_at: 2023-09-19T17:36:18Z
url: https://github.com/astral-sh/ruff/pull/7524
synced_at: 2026-01-12T15:55:24Z
```

# Ensure that LOG007 only triggers on `.exception()` calls

---

_@charliermarsh_

## Summary

Previously, this rule triggered for `logging.info("...", exc_info=False)`.



---

_Label `bug` added by @charliermarsh on 2023-09-19 17:19_

---

_Merged by @charliermarsh on 2023-09-19 17:29_

---

_Closed by @charliermarsh on 2023-09-19 17:29_

---

_Branch deleted on 2023-09-19 17:29_

---

_Comment by @github-actions[bot] on 2023-09-19 17:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
