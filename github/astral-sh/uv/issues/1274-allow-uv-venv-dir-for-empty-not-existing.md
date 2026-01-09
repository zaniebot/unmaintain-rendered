---
number: 1274
title: "Allow `uv venv ${dir}` for empty (not existing) directories"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-02-11T23:02:34Z
updated_at: 2024-02-17T01:39:57Z
url: https://github.com/astral-sh/uv/issues/1274
synced_at: 2026-01-07T13:12:16-06:00
---

# Allow `uv venv ${dir}` for empty (not existing) directories

---

_Issue opened by @charliermarsh on 2024-02-11 23:02_

_No description provided._

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-11 23:02_

---

_Label `bug` added by @charliermarsh on 2024-02-11 23:02_

---

_Renamed from "Allow `puffin venv ${dir}` for empty (not existing) directories" to "Allow `uv venv ${dir}` for empty (not existing) directories" by @zanieb on 2024-02-15 19:50_

---

_Comment by @zanieb on 2024-02-15 19:50_

We delete existing directories right now, right? Should we not?

---

_Referenced in [astral-sh/uv#1472](../../astral-sh/uv/issues/1472.md) on 2024-02-16 14:47_

---

_Comment by @charliermarsh on 2024-02-17 01:39_

Ah this got fixed. The issue was that we were erroring if you tried to create a venv and the directory already existed (but was empty).

(We delete existing directories _if_ they're virtualenvs. We error if the env exists but isn't a virtualenv.)

---

_Closed by @charliermarsh on 2024-02-17 01:39_

---
