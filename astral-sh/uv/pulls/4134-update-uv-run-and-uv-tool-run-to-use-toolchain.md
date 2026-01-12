```yaml
number: 4134
title: "Update `uv run` and `uv tool run` to use `Toolchain::find`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/toolchain-iiii
created_at: 2024-06-07T15:48:07Z
updated_at: 2024-06-07T19:29:01Z
url: https://github.com/astral-sh/uv/pull/4134
synced_at: 2026-01-12T16:06:03Z
```

# Update `uv run` and `uv tool run` to use `Toolchain::find`

---

_@zanieb_

Extends https://github.com/astral-sh/uv/pull/4121

---

_Label `internal` added by @zanieb on 2024-06-07 15:48_

---

_@zanieb reviewed on 2024-06-07 15:48_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:120 on 2024-06-07 15:48_

These were reimplementing logic that's inside of `find` anyway.

---

_Marked ready for review by @zanieb on 2024-06-07 18:15_

---

_Merged by @zanieb on 2024-06-07 19:28_

---

_Closed by @zanieb on 2024-06-07 19:29_

---

_Branch deleted on 2024-06-07 19:29_

---
