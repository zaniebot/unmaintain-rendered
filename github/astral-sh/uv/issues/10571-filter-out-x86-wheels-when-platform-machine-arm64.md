---
number: 10571
title: "Filter out x86 wheels when `platform_machine == 'arm64'` is specified"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2025-01-13T17:07:13Z
updated_at: 2025-01-14T14:39:22Z
url: https://github.com/astral-sh/uv/issues/10571
synced_at: 2026-01-07T13:12:18-06:00
---

# Filter out x86 wheels when `platform_machine == 'arm64'` is specified

---

_Issue opened by @charliermarsh on 2025-01-13 17:07_

We filter by platform, but not architecture. (The title is just an example; the logic should be more general.)

See: https://discord.com/channels/1039017663004942429/1039017663512449056/1328407212825116824


---

_Label `enhancement` added by @charliermarsh on 2025-01-13 17:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-13 17:08_

---

_Comment by @charliermarsh on 2025-01-13 17:08_

I can do this because it will be impacted by my tag refactors.

---

_Closed by @charliermarsh on 2025-01-13 17:08_

---

_Reopened by @charliermarsh on 2025-01-13 17:08_

---

_Referenced in [astral-sh/uv#10584](../../astral-sh/uv/pulls/10584.md) on 2025-01-14 03:27_

---

_Closed by @charliermarsh on 2025-01-14 14:39_

---
