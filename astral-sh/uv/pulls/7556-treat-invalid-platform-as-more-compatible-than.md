```yaml
number: 7556
title: Treat invalid platform as more compatible than invalid Python
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/prio
created_at: 2024-09-19T17:02:32Z
updated_at: 2024-09-19T17:25:22Z
url: https://github.com/astral-sh/uv/pull/7556
synced_at: 2026-01-12T16:07:53Z
```

# Treat invalid platform as more compatible than invalid Python

---

_@charliermarsh_

## Summary

I think this is just inverted. It means that when we fail in https://github.com/astral-sh/uv/issues/7553, we show a message for "invalid Python implementation" (since there are some wheels that don't match), but we should be showing "invalid platform", matching the order of operations in our compatibility check.

Closes https://github.com/astral-sh/uv/issues/7553.


---

_Review requested from @zanieb by @charliermarsh on 2024-09-19 17:02_

---

_Label `bug` added by @charliermarsh on 2024-09-19 17:02_

---

_Label `error messages` added by @charliermarsh on 2024-09-19 17:02_

---

_Marked ready for review by @charliermarsh on 2024-09-19 17:02_

---

_@zanieb approved on 2024-09-19 17:23_

---

_Merged by @charliermarsh on 2024-09-19 17:25_

---

_Closed by @charliermarsh on 2024-09-19 17:25_

---

_Branch deleted on 2024-09-19 17:25_

---
