---
number: 4217
title: "Use standard filter in `run_with_python_version` test"
type: issue
state: closed
author: zanieb
labels:
  - internal
assignees: []
created_at: 2024-06-10T20:46:33Z
updated_at: 2024-07-02T20:51:53Z
url: https://github.com/astral-sh/uv/issues/4217
synced_at: 2026-01-10T01:23:35Z
---

# Use standard filter in `run_with_python_version` test

---

_Issue opened by @zanieb on 2024-06-10 20:46_

It looks like this test rolls its own patch version filtering but we have filtering mechanisms for this

https://github.com/astral-sh/uv/blob/90947a933df2fe3f42bc9cbbf27dbce3a3b3a708/crates/uv/tests/run.rs#L95-L99

---

_Assigned to @zanieb by @zanieb on 2024-06-10 20:46_

---

_Label `internal` added by @zanieb on 2024-06-10 21:00_

---

_Closed by @zanieb on 2024-07-02 20:51_

---
