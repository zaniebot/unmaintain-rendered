```yaml
number: 7474
title: "Respect `pyproject.toml` credentials from user-provided requirements"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cred
created_at: 2024-09-17T17:29:59Z
updated_at: 2024-09-17T19:09:43Z
url: https://github.com/astral-sh/uv/pull/7474
synced_at: 2026-01-12T16:07:50Z
```

# Respect `pyproject.toml` credentials from user-provided requirements

---

_@charliermarsh_

## Summary

When syncing a lockfile, we need to respect credentials defined in the `pyproject.toml`, even if they won't be used for resolution. Unfortunately, this includes credentials in `tool.uv.sources`, `tool.uv.dev-dependencies`, `project.dependencies`, and `project.optional-dependencies`.

Closes https://github.com/astral-sh/uv/issues/7453.


---

_Review requested from @zanieb by @charliermarsh on 2024-09-17 17:30_

---

_Label `bug` added by @charliermarsh on 2024-09-17 17:30_

---

_Renamed from "Respect `pyproject.toml` credentials provided via requirements" to "Respect `pyproject.toml` credentials from user-provided requirements" by @charliermarsh on 2024-09-17 17:31_

---

_@zanieb reviewed on 2024-09-17 18:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:409 on 2024-09-17 18:40_

Might be nice to have a doc here

---

_@zanieb approved on 2024-09-17 18:40_

---

_Comment by @charliermarsh on 2024-09-17 18:43_

Depressed that exactly one Windows test is overflowing the stack.

---

_Merged by @charliermarsh on 2024-09-17 19:09_

---

_Closed by @charliermarsh on 2024-09-17 19:09_

---

_Branch deleted on 2024-09-17 19:09_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:409 on 2024-09-17 19:09_

Oh shoot forgot. Will PR separately.

---

_@charliermarsh reviewed on 2024-09-17 19:09_

---

_@charliermarsh reviewed on 2024-09-17 19:09_

---

_Review comment by @charliermarsh on `crates/uv/tests/common/mod.rs`:536 on 2024-09-17 19:09_

\cc @konstin -- I had to bump this because one test was failing...

---
