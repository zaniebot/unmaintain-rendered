```yaml
number: 8191
title: Do not use free-threaded interpreters without a free-threaded request
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-freethread
created_at: 2024-10-14T22:09:56Z
updated_at: 2024-10-14T22:48:33Z
url: https://github.com/astral-sh/uv/pull/8191
synced_at: 2026-01-10T12:54:04Z
```

# Do not use free-threaded interpreters without a free-threaded request

---

_Pull request opened by @zanieb on 2024-10-14 22:09_

As mentioned in https://github.com/astral-sh/uv/issues/8189

We only checked if an interpreter was free-threaded _when_ free-threaded variants were requested. But we should not use free-threaded interpreters unless explicitly requested.

---

_Label `bug` added by @zanieb on 2024-10-14 22:09_

---

_Review requested from @charliermarsh by @zanieb on 2024-10-14 22:25_

---

_@charliermarsh approved on 2024-10-14 22:26_

---

_Merged by @zanieb on 2024-10-14 22:48_

---

_Closed by @zanieb on 2024-10-14 22:48_

---

_Branch deleted on 2024-10-14 22:48_

---
