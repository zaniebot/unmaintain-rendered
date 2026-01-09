---
number: 5189
title: Add script for integration tests
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - internal
  - testing
assignees: []
created_at: 2024-07-18T14:45:42Z
updated_at: 2024-07-18T14:48:40Z
url: https://github.com/astral-sh/uv/issues/5189
synced_at: 2026-01-07T13:12:17-06:00
---

# Add script for integration tests

---

_Issue opened by @zanieb on 2024-07-18 14:45_

In https://github.com/astral-sh/uv/pull/5141, #5048, and #5047 we have some integration tests that cover the presence of executables in a virtual environment, check that the Python executable works, and that we can install into an environment. We should consolidate these checks into a reusable script at `scripts`. We probably _ought_ to use `bash` and `powershell` since we're testing Python itself, but I could be convinced otherwise.

---

_Label `help wanted` added by @zanieb on 2024-07-18 14:45_

---

_Label `internal` added by @zanieb on 2024-07-18 14:45_

---

_Label `testing` added by @zanieb on 2024-07-18 14:45_

---
