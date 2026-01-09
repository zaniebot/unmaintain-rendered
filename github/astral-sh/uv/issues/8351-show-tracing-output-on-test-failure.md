---
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
synced_at: 2026-01-07T13:12:17-06:00
---

# Show tracing output on test failure

---

_Issue opened by @zanieb on 2024-10-18 23:48_

In the vein of https://github.com/astral-sh/uv/pull/8349, it's nice to have more information from the child process when a test fails. Right now, if you turn tracing on it ends up in the snapshot and breaks the test. We should have tracing on by default, but exclude it from the snapshots and display it on failure. This could be implemented with something like a `UV_LOG_FILE` option we pass to the child process then read from on failure.

---

_Label `help wanted` added by @zanieb on 2024-10-18 23:48_

---

_Label `internal` added by @zanieb on 2024-10-18 23:48_

---

_Label `testing` added by @zanieb on 2024-10-18 23:48_

---

_Referenced in [astral-sh/uv#9850](../../astral-sh/uv/issues/9850.md) on 2024-12-12 20:10_

---

_Referenced in [astral-sh/uv#9873](../../astral-sh/uv/issues/9873.md) on 2024-12-13 18:34_

---

_Referenced in [astral-sh/uv#11774](../../astral-sh/uv/pulls/11774.md) on 2025-02-25 08:51_

---
