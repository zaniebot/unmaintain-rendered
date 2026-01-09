---
number: 6853
title: hello.py
type: issue
state: closed
author: eisenhowerj
labels:
  - duplicate
assignees: []
created_at: 2024-08-30T04:23:27Z
updated_at: 2024-08-30T12:47:23Z
url: https://github.com/astral-sh/uv/issues/6853
synced_at: 2026-01-07T13:12:17-06:00
---

# hello.py

---

_Issue opened by @eisenhowerj on 2024-08-30 04:23_

It is a pain (slight, but consistent) to have to remove this file before pushing my "initial" commit. It looks like this has been acknowledged here:

https://github.com/astral-sh/uv/blob/c91c99b35e6b04fb369241487363e7192fe9849b/crates/uv/src/commands/project/init.rs#L365

An alternative would be to add an `--intro` flag that could be changed in the Getting Started docs here: 

https://docs.astral.sh/uv/#project-management

e.g. `uv init example --intro`



---

_Comment by @zanieb on 2024-08-30 12:04_

Hi! This sounds like a duplicate of #6750 

---

_Label `duplicate` added by @zanieb on 2024-08-30 12:04_

---

_Closed by @charliermarsh on 2024-08-30 12:47_

---
