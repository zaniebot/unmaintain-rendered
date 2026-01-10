```yaml
number: 469
title: "Prefer future stream over `JoinSet` in downloader"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/join-set
created_at: 2023-11-20T13:18:28Z
updated_at: 2023-11-20T13:23:32Z
url: https://github.com/astral-sh/uv/pull/469
synced_at: 2026-01-10T15:50:29Z
```

# Prefer future stream over `JoinSet` in downloader

---

_Pull request opened by @charliermarsh on 2023-11-20 13:18_

This avoids introducing a static lifetime requirement and, in my benchmarks, is even a little faster.

---

_Label `internal` added by @charliermarsh on 2023-11-20 13:18_

---

_Merged by @charliermarsh on 2023-11-20 13:23_

---

_Closed by @charliermarsh on 2023-11-20 13:23_

---

_Branch deleted on 2023-11-20 13:23_

---
