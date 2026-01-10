---
number: 4901
title: Prefetch failures should be non-fatal
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - help wanted
  - resolver
assignees: []
created_at: 2024-07-08T16:26:34Z
updated_at: 2024-11-23T03:40:46Z
url: https://github.com/astral-sh/uv/issues/4901
synced_at: 2026-01-10T01:23:43Z
---

# Prefetch failures should be non-fatal

---

_Issue opened by @charliermarsh on 2024-07-08 16:26_

See: https://github.com/astral-sh/uv/pull/4900#issuecomment-2214420150

---

_Label `enhancement` added by @charliermarsh on 2024-07-08 16:26_

---

_Label `resolver` added by @charliermarsh on 2024-07-08 16:26_

---

_Referenced in [astral-sh/uv#4900](../../astral-sh/uv/pulls/4900.md) on 2024-07-08 16:26_

---

_Label `help wanted` added by @zanieb on 2024-07-08 16:51_

---

_Comment by @zanieb on 2024-07-08 16:52_

If someone is interested in poking at this, I feel like it shouldn't be _too_ wild to store the error and only surface it if we access the prefetched result — I haven't looked into it though.

---

_Comment by @charliermarsh on 2024-07-08 17:04_

Agreed!

---

_Comment by @charliermarsh on 2024-11-23 03:40_

Oh, I fixed these!

---

_Closed by @charliermarsh on 2024-11-23 03:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-23 03:40_

---
