```yaml
number: 8351
title: Show tracing output on test failure
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - internal
  - testing
assignees: []
created_at: 2024-10-18T23:48:52Z
updated_at: 2024-10-18T23:48:58Z
url: https://github.com/astral-sh/uv/issues/8351
synced_at: 2026-01-12T15:59:24Z
```

# Show tracing output on test failure

---

_@zanieb_

In the vein of https://github.com/astral-sh/uv/pull/8349, it's nice to have more information from the child process when a test fails. Right now, if you turn tracing on it ends up in the snapshot and breaks the test. We should have tracing on by default, but exclude it from the snapshots and display it on failure. This could be implemented with something like a `UV_LOG_FILE` option we pass to the child process then read from on failure.

---

_Label `help wanted` added by @zanieb on 2024-10-18 23:48_

---

_Label `internal` added by @zanieb on 2024-10-18 23:48_

---

_Label `testing` added by @zanieb on 2024-10-18 23:48_

---
