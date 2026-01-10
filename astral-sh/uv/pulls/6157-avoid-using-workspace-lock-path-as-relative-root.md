```yaml
number: 6157
title: "Avoid using workspace `lock_path` as relative root"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
  - lock
assignees: []
merged: true
base: main
head: charlie/lock-path
created_at: 2024-08-16T17:13:29Z
updated_at: 2024-08-16T17:24:28Z
url: https://github.com/astral-sh/uv/pull/6157
synced_at: 2026-01-10T13:09:50Z
```

# Avoid using workspace `lock_path` as relative root

---

_Pull request opened by @charliermarsh on 2024-08-16 17:13_

## Summary

I've also made it such that these won't panic, and we gracefully continue if we fail to validate a lockfile.

Closes https://github.com/astral-sh/uv/issues/6142.


---

_Label `preview` added by @charliermarsh on 2024-08-16 17:13_

---

_Label `lock` added by @charliermarsh on 2024-08-16 17:13_

---

_Merged by @charliermarsh on 2024-08-16 17:24_

---

_Closed by @charliermarsh on 2024-08-16 17:24_

---

_Branch deleted on 2024-08-16 17:24_

---
