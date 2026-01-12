```yaml
number: 381
title: "Make resolver's indexes and locks a global package database"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2023-11-09T15:04:19Z
updated_at: 2024-01-16T05:38:44Z
url: https://github.com/astral-sh/uv/issues/381
synced_at: 2026-01-12T15:58:23Z
```

# Make resolver's indexes and locks a global package database

---

_@charliermarsh_

The resolver tracks a lot of state in `Index` (and `Locks`). Ideally, we'd pass this state down to the build context when resolving for _build dependencies_ (and share the same locks). There's probably room to lift this out of the resolver and reframe as a shared package database.

---

_Label `internal` added by @charliermarsh on 2023-11-10 02:11_

---

_Added to milestone `Future` by @charliermarsh on 2023-11-10 02:11_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-15 14:59_

---

_Comment by @charliermarsh on 2024-01-16 05:38_

This is done by https://github.com/astral-sh/puffin/pull/906.

---

_Closed by @charliermarsh on 2024-01-16 05:38_

---
