---
number: 8551
title: "`uv tree` is always empty for projects with cyclic dependencies"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-10-25T01:38:33Z
updated_at: 2024-10-26T17:30:44Z
url: https://github.com/astral-sh/uv/issues/8551
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv tree` is always empty for projects with cyclic dependencies

---

_Issue opened by @charliermarsh on 2024-10-25 01:38_

E.g., if you run `uv tree` in `flax`, it's always empty. `flax` depends on things that ultimately depend on `flax`.

---

_Label `bug` added by @charliermarsh on 2024-10-25 01:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-25 01:38_

---

_Referenced in [astral-sh/uv#8564](../../astral-sh/uv/pulls/8564.md) on 2024-10-25 13:27_

---

_Closed by @charliermarsh on 2024-10-26 17:30_

---

_Closed by @charliermarsh on 2024-10-26 17:30_

---
