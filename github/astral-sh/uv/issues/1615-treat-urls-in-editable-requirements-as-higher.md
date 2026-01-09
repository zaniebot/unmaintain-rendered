---
number: 1615
title: Treat URLs in editable requirements as higher priority
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-02-17T20:24:07Z
updated_at: 2024-02-22T02:28:00Z
url: https://github.com/astral-sh/uv/issues/1615
synced_at: 2026-01-07T13:12:16-06:00
---

# Treat URLs in editable requirements as higher priority

---

_Issue opened by @charliermarsh on 2024-02-17 20:24_

If you add `iniconfig` as a top-level requirement in `compile_editable_url_requirement`, we end up choosing `iniconfig` from the registry rather than the URL dep. The issue is that we need to visit URL deps _before_ their corresponding registry deps. (This is not a common setup, but the current behavior is slightly incorrect.)


---

_Label `bug` added by @charliermarsh on 2024-02-17 20:24_

---

_Referenced in [astral-sh/uv#1796](../../astral-sh/uv/pulls/1796.md) on 2024-02-21 19:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-21 19:45_

---

_Closed by @charliermarsh on 2024-02-22 02:28_

---

_Closed by @charliermarsh on 2024-02-22 02:28_

---
