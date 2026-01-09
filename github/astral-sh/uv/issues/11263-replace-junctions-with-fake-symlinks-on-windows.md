---
number: 11263
title: "Replace junctions with \"fake\" symlinks on Windows"
type: issue
state: closed
author: charliermarsh
labels:
  - windows
assignees: []
created_at: 2025-02-05T21:42:44Z
updated_at: 2025-02-13T22:17:51Z
url: https://github.com/astral-sh/uv/issues/11263
synced_at: 2026-01-07T13:12:18-06:00
---

# Replace junctions with "fake" symlinks on Windows

---

_Issue opened by @charliermarsh on 2025-02-05 21:42_

Junctions are causing some weird issues. We also can't replace them atomically. I think instead we should just write a file that contains the target path. It's a little silly, but it's the same thing, simpler, universally supported (of course), and we can replace them atomically.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-05 21:42_

---

_Label `windows` added by @charliermarsh on 2025-02-05 21:42_

---

_Referenced in [DataDog/datadog-agent-buildimages#751](../../DataDog/datadog-agent-buildimages/pulls/751.md) on 2025-02-05 23:25_

---

_Referenced in [astral-sh/uv#11269](../../astral-sh/uv/pulls/11269.md) on 2025-02-06 01:03_

---

_Closed by @zanieb on 2025-02-13 22:17_

---
