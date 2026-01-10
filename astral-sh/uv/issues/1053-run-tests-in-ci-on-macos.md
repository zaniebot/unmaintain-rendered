---
number: 1053
title: Run tests in CI on macOS
type: issue
state: closed
author: zanieb
labels:
  - internal
  - testing
assignees: []
created_at: 2024-01-22T23:02:09Z
updated_at: 2024-01-31T15:27:05Z
url: https://github.com/astral-sh/uv/issues/1053
synced_at: 2026-01-10T01:23:05Z
---

# Run tests in CI on macOS

---

_Issue opened by @zanieb on 2024-01-22 23:02_

Currently we only run CI tests on Linux.

We could include tests for macOS, but it's expensive. Regardless, the project fails to compile https://github.com/astral-sh/puffin/actions/runs/7618370740/job/20749516326

---

_Label `internal` added by @zanieb on 2024-01-22 23:02_

---

_Label `testing` added by @zanieb on 2024-01-22 23:02_

---

_Comment by @charliermarsh on 2024-01-22 23:18_

Note: Fails to compile with all features (which we don’t use). The project does compile on macOS of course.

---

_Comment by @zanieb on 2024-01-23 04:58_

👍 yeah I wasn't sure if it was a feature thing or something to do with the GitHub Actions runners.

---

_Referenced in [astral-sh/uv#1193](../../astral-sh/uv/pulls/1193.md) on 2024-01-31 01:32_

---

_Closed by @charliermarsh on 2024-01-31 15:27_

---
