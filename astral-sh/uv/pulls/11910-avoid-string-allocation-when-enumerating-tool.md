```yaml
number: 11910
title: Avoid string allocation when enumerating tool names
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/x
created_at: 2025-03-03T02:51:11Z
updated_at: 2025-03-03T15:13:57Z
url: https://github.com/astral-sh/uv/pull/11910
synced_at: 2026-01-10T11:10:39Z
```

# Avoid string allocation when enumerating tool names

---

_Pull request opened by @charliermarsh on 2025-03-03 02:51_

## Summary

If it's not ASCII, then it's not a valid package name anyway.


---

_Label `performance` added by @charliermarsh on 2025-03-03 02:51_

---

_Marked ready for review by @charliermarsh on 2025-03-03 02:51_

---

_@konstin approved on 2025-03-03 11:06_

---

_Merged by @charliermarsh on 2025-03-03 15:13_

---

_Closed by @charliermarsh on 2025-03-03 15:13_

---

_Branch deleted on 2025-03-03 15:13_

---
