```yaml
number: 6438
title: Support locks path deps with path deps
type: pull_request
state: closed
author: konstin
labels:
  - bug
assignees: []
draft: true
base: main
head: konsti/support-relative-path-hopping
created_at: 2024-08-22T14:08:30Z
updated_at: 2024-08-24T02:19:12Z
url: https://github.com/astral-sh/uv/pull/6438
synced_at: 2026-01-10T13:09:51Z
```

# Support locks path deps with path deps

---

_Pull request opened by @konstin on 2024-08-22 14:08_

Pass the location of `uv.lock`, the base of our relative lockfile paths, around when locking in project mode. This fixes previous incorrect paths that were using the root of the other workspace as reference for the relative path.

Fixes #6371.


---

_Label `bug` added by @konstin on 2024-08-22 14:08_

---

_@charliermarsh reviewed on 2024-08-22 20:49_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/install.rs`:340 on 2024-08-22 20:49_

Should we make this a custom enum so you don't need this comment everywhere? Something self-documenting, like `WorkspaceRoot::None`?

---

_@charliermarsh reviewed on 2024-08-22 20:49_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/compile.rs`:315 on 2024-08-22 20:49_

Should we just use `None` then?

---

_@charliermarsh reviewed on 2024-08-22 20:49_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/install.rs`:340 on 2024-08-22 20:49_

Otherwise I'd probably just remove the comments.

---

_@charliermarsh approved on 2024-08-22 20:49_

---

_Comment by @charliermarsh on 2024-08-22 20:50_

Those Windows failures are real -- we need to consistently use the canonical or non-canonical paths.

---

_Comment by @konstin on 2024-08-23 08:12_

Waiting for #6490 first

---

_Converted to draft by @konstin on 2024-08-23 08:12_

---

_Closed by @charliermarsh on 2024-08-24 02:19_

---

_Closed by @charliermarsh on 2024-08-24 02:19_

---
