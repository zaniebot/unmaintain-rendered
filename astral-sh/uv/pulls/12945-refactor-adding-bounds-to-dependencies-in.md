```yaml
number: 12945
title: "Refactor adding bounds to dependencies in `pyproject.toml`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/refactor-adding-bounds
created_at: 2025-04-17T14:12:04Z
updated_at: 2025-04-21T10:25:28Z
url: https://github.com/astral-sh/uv/pull/12945
synced_at: 2026-01-12T16:10:27Z
```

# Refactor adding bounds to dependencies in `pyproject.toml`

---

_@konstin_

Preparation work for making the kind of bound added configurable.

No functional changes, just moving some methods around.

---

_Label `internal` added by @konstin on 2025-04-17 14:12_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:560 on 2025-04-17 17:00_

Not crucial but this might be able to return a refined error like, since it looks like the only errors come from `pyproject_mut`.

---

_@charliermarsh reviewed on 2025-04-17 17:00_

---

_@charliermarsh approved on 2025-04-17 17:00_

---

_Merged by @konstin on 2025-04-21 10:25_

---

_Closed by @konstin on 2025-04-21 10:25_

---

_Branch deleted on 2025-04-21 10:25_

---
