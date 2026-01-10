```yaml
number: 2322
title: "Move `Error` methods off of `ErrorKind`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/kind
created_at: 2024-03-10T02:36:42Z
updated_at: 2024-03-10T02:42:24Z
url: https://github.com/astral-sh/uv/pull/2322
synced_at: 2026-01-10T14:54:43Z
```

# Move `Error` methods off of `ErrorKind`

---

_Pull request opened by @charliermarsh on 2024-03-10 02:36_

## Summary

Using `ErrorKind` is leaking an abstraction, since this only exists (IIUC) to box the variant.

---

_Label `internal` added by @charliermarsh on 2024-03-10 02:36_

---

_Merged by @charliermarsh on 2024-03-10 02:42_

---

_Closed by @charliermarsh on 2024-03-10 02:42_

---

_Branch deleted on 2024-03-10 02:42_

---
