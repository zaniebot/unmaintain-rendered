---
number: 743
title: Improve hit-rate on parallel pre-fetching
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2024-01-02T16:38:51Z
updated_at: 2024-01-03T10:37:46Z
url: https://github.com/astral-sh/uv/issues/743
synced_at: 2026-01-10T01:23:04Z
---

# Improve hit-rate on parallel pre-fetching

---

_Issue opened by @charliermarsh on 2024-01-02 16:38_

Right now, we try to pre-fetch version metadata for packages, but in practice, it looks like we're missing those opportunities as we don't yet have the _package_-level metadata in the vast majority of cases.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-02 16:38_

---

_Label `performance` added by @charliermarsh on 2024-01-02 16:38_

---

_Referenced in [astral-sh/uv#744](../../astral-sh/uv/pulls/744.md) on 2024-01-02 20:59_

---

_Closed by @konstin on 2024-01-03 10:37_

---
