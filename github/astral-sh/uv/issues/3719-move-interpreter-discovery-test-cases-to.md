---
number: 3719
title: Move interpreter discovery test cases to subprocesses
type: issue
state: open
author: zanieb
labels:
  - internal
  - testing
assignees: []
created_at: 2024-05-21T19:46:16Z
updated_at: 2024-05-21T19:46:27Z
url: https://github.com/astral-sh/uv/issues/3719
synced_at: 2026-01-07T13:12:17-06:00
---

# Move interpreter discovery test cases to subprocesses

---

_Issue opened by @zanieb on 2024-05-21 19:46_

Instead of testing interpreter discovery with mocked environment variables, we should consider exposing a CLI command e.g. `uv python find [...]` so we can test with full control of the environment and remove ugly workarounds to patch e.g. the working directory.

---

_Label `internal` added by @zanieb on 2024-05-21 19:46_

---

_Label `testing` added by @zanieb on 2024-05-21 19:46_

---
