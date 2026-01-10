```yaml
number: 7022
title: "Rollback `pyproject.toml` changes on all errors"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/net
created_at: 2024-09-04T14:27:39Z
updated_at: 2024-09-04T14:42:17Z
url: https://github.com/astral-sh/uv/pull/7022
synced_at: 2026-01-10T12:53:38Z
```

# Rollback `pyproject.toml` changes on all errors

---

_Pull request opened by @charliermarsh on 2024-09-04 14:27_

## Summary

The error handlers now happen one level higher, matching on _any_ `Err` that's returned from the lock-and-sync operations.

Closes https://github.com/astral-sh/uv/issues/7011.


---

_Label `bug` added by @charliermarsh on 2024-09-04 14:27_

---

_@zanieb approved on 2024-09-04 14:37_

---

_Merged by @charliermarsh on 2024-09-04 14:42_

---

_Closed by @charliermarsh on 2024-09-04 14:42_

---

_Branch deleted on 2024-09-04 14:42_

---
