```yaml
number: 9488
title: Insert backslash when appending to system drive
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/win-sys
created_at: 2024-11-28T00:18:44Z
updated_at: 2024-11-28T00:38:30Z
url: https://github.com/astral-sh/uv/pull/9488
synced_at: 2026-01-10T12:00:00Z
```

# Insert backslash when appending to system drive

---

_Pull request opened by @charliermarsh on 2024-11-28 00:18_

## Summary

When you pass a system drive to `Path::join`, Rust doesn't insert a backslash between the drive and the path itself, so our lookups for system configuration were failing.

Closes https://github.com/astral-sh/uv/issues/9416.


---

_Review requested from @zanieb by @charliermarsh on 2024-11-28 00:18_

---

_Label `bug` added by @charliermarsh on 2024-11-28 00:18_

---

_Label `windows` added by @charliermarsh on 2024-11-28 00:18_

---

_Marked ready for review by @charliermarsh on 2024-11-28 00:18_

---

_@zanieb approved on 2024-11-28 00:32_

---

_Merged by @charliermarsh on 2024-11-28 00:38_

---

_Closed by @charliermarsh on 2024-11-28 00:38_

---

_Branch deleted on 2024-11-28 00:38_

---
