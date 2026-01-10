---
number: 7539
title: Python installation from local folder
type: issue
state: closed
author: andreas-vester
labels:
  - question
assignees: []
created_at: 2024-09-19T09:17:04Z
updated_at: 2024-09-22T23:28:15Z
url: https://github.com/astral-sh/uv/issues/7539
synced_at: 2026-01-10T01:24:16Z
---

# Python installation from local folder

---

_Issue opened by @andreas-vester on 2024-09-19 09:17_

Using ``uv`` behind a corporate firewall isn't always easy. One of the problems I am facing is related to ``uv python install``. I am not allowed to simply connect to github.com to download files using a command line program. What I can do, on the other hand, is to browse to the GitHub release page and manually download the required cpython tar.gz file.

I am wondering if I can point ``uv python install`` to the local folder where I saved the cpython archive file?


---

_Comment by @zanieb on 2024-09-19 11:03_

You can use `UV_PYTHON_INSTALL_MIRRO` to point to a directory that matches the structure of the URL.

See #6950 

---

_Label `question` added by @zanieb on 2024-09-19 11:03_

---

_Closed by @zanieb on 2024-09-22 23:28_

---
