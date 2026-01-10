---
number: 551
title: "Allow yanked dependencies when requested with `==`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-04T21:56:13Z
updated_at: 2023-12-05T08:44:08Z
url: https://github.com/astral-sh/uv/issues/551
synced_at: 2026-01-10T01:23:04Z
---

# Allow yanked dependencies when requested with `==`

---

_Issue opened by @charliermarsh on 2023-12-04 21:56_

Poetry seems to allow these (with a warning), and users depend on it. (If you use a non-`==` specifier, it just fails without any indication that the packages are yanked -- says there are no such matching versions.)


---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-05 00:00_

---

_Label `enhancement` added by @charliermarsh on 2023-12-05 00:00_

---

_Label `enhancement` removed by @charliermarsh on 2023-12-05 00:00_

---

_Label `bug` added by @charliermarsh on 2023-12-05 00:00_

---

_Referenced in [astral-sh/uv#561](../../astral-sh/uv/pulls/561.md) on 2023-12-05 05:13_

---

_Closed by @konstin on 2023-12-05 08:44_

---
