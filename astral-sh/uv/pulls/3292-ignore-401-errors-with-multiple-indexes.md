```yaml
number: 3292
title: Ignore 401 errors with multiple indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/forbidden
created_at: 2024-04-28T12:54:13Z
updated_at: 2024-04-28T14:06:44Z
url: https://github.com/astral-sh/uv/pull/3292
synced_at: 2026-01-10T14:37:54Z
```

# Ignore 401 errors with multiple indexes

---

_Pull request opened by @charliermarsh on 2024-04-28 12:54_

## Summary

It seems like Azure might return a 401 when you request a package that doesn't exist (even with valid credentials)? But I admittedly haven't tested this. (We already skip 403, and this seems similar?)

Closes https://github.com/astral-sh/uv/issues/3291.


---

_Review requested from @zanieb by @charliermarsh on 2024-04-28 12:54_

---

_Marked ready for review by @charliermarsh on 2024-04-28 12:54_

---

_Label `compatibility` added by @charliermarsh on 2024-04-28 12:54_

---

_@zanieb approved on 2024-04-28 13:48_

---

_Merged by @charliermarsh on 2024-04-28 14:06_

---

_Closed by @charliermarsh on 2024-04-28 14:06_

---

_Branch deleted on 2024-04-28 14:06_

---
