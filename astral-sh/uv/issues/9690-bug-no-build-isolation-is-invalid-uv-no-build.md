---
number: 9690
title: "[BUG]`--no-build-isolation` is invalid, UV_NO_BUILD_ISOLATION must be used"
type: issue
state: closed
author: sdbds
labels:
  - needs-mre
assignees: []
created_at: 2024-12-06T16:58:14Z
updated_at: 2024-12-06T19:10:25Z
url: https://github.com/astral-sh/uv/issues/9690
synced_at: 2026-01-10T01:24:44Z
---

# [BUG]`--no-build-isolation` is invalid, UV_NO_BUILD_ISOLATION must be used

---

_Issue opened by @sdbds on 2024-12-06 16:58_

UV version 0.5.6
`--no-build-isolation` is invalid, UV_NO_BUILD_ISOLATION must be used

---

_Comment by @FishAlchemist on 2024-12-06 17:16_

To reproduce the problem, please provide an MRE(minimal reproducible example).

---

_Label `needs-mre` added by @charliermarsh on 2024-12-06 18:24_

---

_Comment by @sdbds on 2024-12-06 19:10_

Sorry, the local reproduction failed, it seems to be okay now. It might be that I used the wrong command.

---

_Closed by @sdbds on 2024-12-06 19:10_

---
