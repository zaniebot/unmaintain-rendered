```yaml
number: 2461
title: Avoid panicking on cannot-be-a-base URLs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/colon
created_at: 2024-03-14T17:37:24Z
updated_at: 2024-03-14T17:47:17Z
url: https://github.com/astral-sh/uv/pull/2461
synced_at: 2026-01-10T14:49:08Z
```

# Avoid panicking on cannot-be-a-base URLs

---

_Pull request opened by @charliermarsh on 2024-03-14 17:37_

`path_segments_mut` returns an `Err` for cannot-be-a-base URLs. These won't be valid when we try to fetch them anyway, but we need to avoid a panic.

Closes https://github.com/astral-sh/uv/issues/2460.

---

_Label `bug` added by @charliermarsh on 2024-03-14 17:37_

---

_Merged by @charliermarsh on 2024-03-14 17:47_

---

_Closed by @charliermarsh on 2024-03-14 17:47_

---

_Branch deleted on 2024-03-14 17:47_

---
