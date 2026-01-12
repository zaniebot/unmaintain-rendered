```yaml
number: 2336
title: "Remove duplicate `INSTALLER` in `RECORD`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/inst
created_at: 2024-03-10T13:25:23Z
updated_at: 2024-03-10T13:52:07Z
url: https://github.com/astral-sh/uv/pull/2336
synced_at: 2026-01-12T16:04:59Z
```

# Remove duplicate `INSTALLER` in `RECORD`

---

_@charliermarsh_

## Summary

We write this a few lines down with a value passed in by the caller. I suspect I missed that this was already here (with a less accurate value) when adding `INSTALLER`.

---

_Label `bug` added by @charliermarsh on 2024-03-10 13:25_

---

_Comment by @charliermarsh on 2024-03-10 13:25_

This shouldn't really matter in practice but it is wrong.

---

_Merged by @charliermarsh on 2024-03-10 13:52_

---

_Closed by @charliermarsh on 2024-03-10 13:52_

---

_Branch deleted on 2024-03-10 13:52_

---
