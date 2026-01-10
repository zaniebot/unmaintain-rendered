---
number: 553
title: Distribution database deletes local wheels
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-04T22:09:46Z
updated_at: 2023-12-05T17:50:09Z
url: https://github.com/astral-sh/uv/issues/553
synced_at: 2026-01-10T01:23:04Z
---

# Distribution database deletes local wheels

---

_Issue opened by @charliermarsh on 2023-12-04 22:09_

It looks like if you have a local path dependency, then when installing, we unzip it in-place and delete the local wheel. Definitely a high-priority thing to fix! We can either (1) unzip it into the cache, or (2) copy it into the cache earlier in the process.


---

_Label `bug` added by @charliermarsh on 2023-12-04 22:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-05 02:03_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-12-05 02:04_

---

_Comment by @charliermarsh on 2023-12-05 02:07_

I think we're also writing the built wheels to the wrong place, since the `path` on the `LocalWheel::Disk` uses the path of the wheel itself... Rather than the path within the cache.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-05 03:38_

---

_Referenced in [astral-sh/uv#560](../../astral-sh/uv/pulls/560.md) on 2023-12-05 03:43_

---

_Closed by @charliermarsh on 2023-12-05 17:50_

---
