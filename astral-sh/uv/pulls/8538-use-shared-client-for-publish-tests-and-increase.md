```yaml
number: 8538
title: Use shared client for publish tests and increase read timeout
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/publish-test
created_at: 2024-10-24T18:26:21Z
updated_at: 2024-10-25T13:36:26Z
url: https://github.com/astral-sh/uv/pull/8538
synced_at: 2026-01-12T16:08:21Z
```

# Use shared client for publish tests and increase read timeout

---

_@zanieb_

Attempts to address the read timeout case in https://github.com/astral-sh/uv/issues/8418

---

_Label `internal` added by @zanieb on 2024-10-24 18:26_

---

_@charliermarsh approved on 2024-10-24 18:47_

---

_Merged by @zanieb on 2024-10-24 19:24_

---

_Closed by @zanieb on 2024-10-24 19:24_

---

_Branch deleted on 2024-10-24 19:24_

---

_Comment by @konstin on 2024-10-25 07:46_

These changes clashed with the retries I added in https://github.com/astral-sh/uv/pull/8531/files#diff-c00e37877a48b475ceb2bcb82aa53b8c2d473af923fd7322bb62ffa9dd335339R124

---

_Comment by @zanieb on 2024-10-25 13:36_

Retries seem great, but why not use a shared client still?

---
