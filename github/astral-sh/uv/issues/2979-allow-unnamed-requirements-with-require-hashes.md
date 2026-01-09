---
number: 2979
title: "Allow unnamed requirements with `--require-hashes`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-04-10T21:24:35Z
updated_at: 2024-04-11T15:26:51Z
url: https://github.com/astral-sh/uv/issues/2979
synced_at: 2026-01-07T13:12:17-06:00
---

# Allow unnamed requirements with `--require-hashes`

---

_Issue opened by @charliermarsh on 2024-04-10 21:24_

This is a (fixable) limitation of the current implementation. I suspect this to be rare, since with `--require-hashes`, you're typically working with a locked `requirements.txt`, which will always have package names. But we should lift it.

---

_Label `enhancement` added by @charliermarsh on 2024-04-10 21:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-11 04:45_

---

_Referenced in [astral-sh/uv#2993](../../astral-sh/uv/pulls/2993.md) on 2024-04-11 14:29_

---

_Closed by @charliermarsh on 2024-04-11 15:26_

---
