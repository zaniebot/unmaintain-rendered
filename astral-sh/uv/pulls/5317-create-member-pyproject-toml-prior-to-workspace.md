```yaml
number: 5317
title: "Create member `pyproject.toml` prior to workspace discovery"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/add
created_at: 2024-07-22T23:50:03Z
updated_at: 2024-07-23T00:07:40Z
url: https://github.com/astral-sh/uv/pull/5317
synced_at: 2026-01-12T16:06:45Z
```

# Create member `pyproject.toml` prior to workspace discovery

---

_@charliermarsh_

## Summary

Otherwise, if the path is already a member, discovery fails.

Also adds a failing test for "adding members that are already covered by `members`".


---

_Label `bug` added by @charliermarsh on 2024-07-22 23:50_

---

_Renamed from "Avoid creating `init` directory prior to workspace discovery" to "Create member `pyproject.toml` prior to workspace discovery" by @charliermarsh on 2024-07-22 23:54_

---

_@zanieb reviewed on 2024-07-23 00:00_

---

_Review comment by @zanieb on `crates/uv/tests/init.rs`:762 on 2024-07-23 00:00_

To clarify, we want to add a specific entry even though it's already covered?

---

_Merged by @charliermarsh on 2024-07-23 00:03_

---

_Closed by @charliermarsh on 2024-07-23 00:03_

---

_Branch deleted on 2024-07-23 00:03_

---

_@charliermarsh reviewed on 2024-07-23 00:05_

---

_Review comment by @charliermarsh on `crates/uv/tests/init.rs`:762 on 2024-07-23 00:05_

Nope! That's a bug. Already exists prior to this PR.

---

_@charliermarsh reviewed on 2024-07-23 00:07_

---

_Review comment by @charliermarsh on `crates/uv/tests/init.rs`:762 on 2024-07-23 00:07_

(I'm already fixing it separately, it's how I discovered the bug fixed in this PR.)

---

_Label `preview` added by @charliermarsh on 2024-07-23 00:07_

---
