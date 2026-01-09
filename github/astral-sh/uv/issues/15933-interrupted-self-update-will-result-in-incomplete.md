---
number: 15933
title: Interrupted self update will result in incomplete binary file
type: issue
state: open
author: criyle
labels:
  - bug
assignees: []
created_at: 2025-09-18T14:05:37Z
updated_at: 2025-09-18T14:12:27Z
url: https://github.com/astral-sh/uv/issues/15933
synced_at: 2026-01-07T13:12:19-06:00
---

# Interrupted self update will result in incomplete binary file

---

_Issue opened by @criyle on 2025-09-18 14:05_

### Summary

I was doing `uv self update` after I logged to a remote host in vs code, but it was interrupted by vs code because it wants to activate virtual environment for that terminal. Then the uv binary becomes incomplete and running the command will result in `segment fault`. I suspect the update is not atomic operation. 

### Platform

Red Hat Enterprise Linux 9.4 (Plow)

### Version

uv 0.8.16

### Python version

Python 3.12.11

---

_Label `bug` added by @criyle on 2025-09-18 14:05_

---

_Comment by @zanieb on 2025-09-18 14:12_

cc @Gankra 

---
