```yaml
number: 14284
title: "Reduce test worker count from 20 -> 12 on Windows"
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/win-wor
created_at: 2025-06-26T16:39:27Z
updated_at: 2025-07-02T17:59:01Z
url: https://github.com/astral-sh/uv/pull/14284
synced_at: 2026-01-10T06:53:01Z
```

# Reduce test worker count from 20 -> 12 on Windows

---

_Pull request opened by @zanieb on 2025-06-26 16:39_

Investigating https://github.com/astral-sh/uv/issues/14158

---

_Label `internal` added by @zanieb on 2025-06-26 16:39_

---

_Comment by @zanieb on 2025-06-26 16:55_

The test step goes from 5m 23s - 33s -> 5m 43s - 53s (taking two random samples)


---

_Closed by @zanieb on 2025-07-02 17:59_

---
