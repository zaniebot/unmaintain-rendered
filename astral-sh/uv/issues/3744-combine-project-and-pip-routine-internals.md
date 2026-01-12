```yaml
number: 3744
title: "Combine `project` and `pip` routine internals"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2024-05-22T16:46:13Z
updated_at: 2024-05-22T20:06:05Z
url: https://github.com/astral-sh/uv/issues/3744
synced_at: 2026-01-12T15:58:45Z
```

# Combine `project` and `pip` routine internals

---

_@charliermarsh_

I think I can merge the utilities in `crates/uv/src/commands/project/mod.rs` with those in `crates/uv/src/commands/pip/operations.rs`, so that we can have shared routines for installing and resolving between the two APIs and across the various commands. Would lead to a lot of deduplication and ideally better abstractions.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-22 16:46_

---

_Label `internal` added by @charliermarsh on 2024-05-22 16:46_

---

_Closed by @charliermarsh on 2024-05-22 20:06_

---
