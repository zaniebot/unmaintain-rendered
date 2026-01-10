---
number: 13837
title: "error while loading shared libraries: $ORIGIN/../lib/libpython3.12.so.1.0: DST not allowed in SUID/SGID programs"
type: issue
state: closed
author: eg-b
labels:
  - question
assignees: []
created_at: 2025-06-04T14:19:03Z
updated_at: 2025-06-04T15:26:03Z
url: https://github.com/astral-sh/uv/issues/13837
synced_at: 2026-01-10T01:25:39Z
---

# error while loading shared libraries: $ORIGIN/../lib/libpython3.12.so.1.0: DST not allowed in SUID/SGID programs

---

_Issue opened by @eg-b on 2025-06-04 14:19_

### Question

Steps to reproduce:
1. create venv using `uv venv --python=3.12`
2. activate venv
3. execute in terminal: `sudo setcap cap_net_raw+ep /home/user/.local/share/uv/python/cpython-3.12.10-linux-x86_64-gnu/bin/python3.12`
4. Then try run `python`

Getting: 

> /home/baranov_eg@avp.ru/work/siem-test/.venv/bin/python: error while loading shared libraries: $ORIGIN/../lib/libpython3.12.so.1.0: DST not allowed in SUID/SGID programs

Is there any option to avoid this? I need to be able to set this cap in order to use `scapy` lib

### Platform

Ubuntu 22.04

### Version

0.7.8

---

_Label `question` added by @eg-b on 2025-06-04 14:19_

---

_Assigned to @geofft by @konstin on 2025-06-04 14:22_

---

_Comment by @zanieb on 2025-06-04 14:26_

Is this a duplicate of https://github.com/astral-sh/python-build-standalone/issues/576 ?

---

_Comment by @zanieb on 2025-06-04 14:26_

Can you upgrade to the latest version of uv and reinstall your Python versions? cc @geofft 

---

_Comment by @geofft on 2025-06-04 15:06_

Yes - this is the same as astral-sh/python-build-standalone#576 (which I forgot to re-close). I expect this to work with our latest Python builds. Please try
```
uv self update
uv python install --reinstall
```
and see if the problem persists.

---

_Comment by @eg-b on 2025-06-04 15:26_

works fine, thanks

---

_Closed by @eg-b on 2025-06-04 15:26_

---
