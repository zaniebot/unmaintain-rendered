```yaml
number: 17500
title: "Ensure `uv.exe` exits when `uvw.exe` or `uvx.exe` is killed"
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: claude/investigate-issue-17492-znH4F
created_at: 2026-01-15T20:54:57Z
updated_at: 2026-01-16T00:10:41Z
url: https://github.com/astral-sh/uv/pull/17500
synced_at: 2026-01-16T01:04:20Z
```

# Ensure `uv.exe` exits when `uvw.exe` or `uvx.exe` is killed

---

_@zanieb_

This matches the approach used by the trampoline in `bounce.rs`.

Closes #17492

---

_Label `bug` added by @zanieb on 2026-01-15 20:55_

---

_Renamed from "Ensure `uv.exe` exits when `uvw.exe` is killed" to "Ensure `uv.exe` exits when `uvw.exe` or `uvx.exe` is killed" by @zanieb on 2026-01-15 23:52_

---

_Comment by @zanieb on 2026-01-15 23:57_

Terrifying code. Will need to review this closely.

---
