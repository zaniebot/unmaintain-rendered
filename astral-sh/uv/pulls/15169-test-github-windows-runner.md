```yaml
number: 15169
title: Test GitHub Windows runner
type: pull_request
state: closed
author: zanieb
labels:
  - internal
  - testing
assignees: []
draft: true
base: main
head: zb/github-windows
created_at: 2025-08-08T17:37:44Z
updated_at: 2025-08-08T17:56:03Z
url: https://github.com/astral-sh/uv/pull/15169
synced_at: 2026-01-12T16:11:36Z
```

# Test GitHub Windows runner

---

_@zanieb_

We've seen CI times for Windows tests creep up lately — want to rule this (https://github.com/astral-sh/uv/pull/14122) out as a cause.

---

_Label `internal` added by @zanieb on 2025-08-08 17:37_

---

_Label `testing` added by @zanieb on 2025-08-08 17:37_

---

_Comment by @zanieb on 2025-08-08 17:54_

Naw it's slower. Total runtime 11m 45s vs 11m, test runtime like 9m vs 7m.

---

_Closed by @zanieb on 2025-08-08 17:54_

---

_Comment by @zanieb on 2025-08-08 17:56_

Hm that might be with no rust cache... running again.

---
