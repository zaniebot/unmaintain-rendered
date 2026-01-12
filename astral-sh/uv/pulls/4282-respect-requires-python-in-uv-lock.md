```yaml
number: 4282
title: "Respect `requires-python` in `uv lock`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: zb/lock-requires
created_at: 2024-06-12T17:12:38Z
updated_at: 2024-06-12T18:19:01Z
url: https://github.com/astral-sh/uv/pull/4282
synced_at: 2026-01-12T16:06:07Z
```

# Respect `requires-python` in `uv lock`

---

_@zanieb_

We weren't using the common interface in `uv lock` because it didn't support finding an interpreter without touching the virtual environment. Here I refactor the project interface to support what we need and update `uv lock` to use the shared implementation.

---

_Label `bug` added by @zanieb on 2024-06-12 17:12_

---

_Label `preview` added by @zanieb on 2024-06-12 17:12_

---

_@zanieb reviewed on 2024-06-12 17:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:88 on 2024-06-12 17:15_

We should assume preview is enabled during preview commands for now.

---

_@zanieb reviewed on 2024-06-12 17:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:88 on 2024-06-12 17:15_

(i.e. if I'm using the experimental interface I shouldn't need to opt-in to managed toolchains)

---

_Marked ready for review by @zanieb on 2024-06-12 17:16_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-12 17:17_

---

_@charliermarsh approved on 2024-06-12 17:23_

---

_Merged by @zanieb on 2024-06-12 18:19_

---

_Closed by @zanieb on 2024-06-12 18:19_

---

_Branch deleted on 2024-06-12 18:19_

---
