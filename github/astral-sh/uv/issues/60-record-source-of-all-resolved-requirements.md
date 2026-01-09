---
number: 60
title: Record source of all resolved requirements
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-10-08T16:42:42Z
updated_at: 2023-10-20T05:15:00Z
url: https://github.com/astral-sh/uv/issues/60
synced_at: 2026-01-07T13:12:16-06:00
---

# Record source of all resolved requirements

---

_Issue opened by @charliermarsh on 2023-10-08 16:42_

Like pip-compile, the resultant lockfile should contain comments telling you why each dependency was included.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-10 20:09_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-10-19 02:29_

---

_Comment by @charliermarsh on 2023-10-19 02:30_

Not working on this but we still need to do it. The PubGrub graph doesn't make this super trivial, we have to reconstruct it ourselves after the resolution.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-20 00:57_

---

_Referenced in [astral-sh/uv#149](../../astral-sh/uv/pulls/149.md) on 2023-10-20 04:50_

---

_Closed by @charliermarsh on 2023-10-20 05:15_

---
