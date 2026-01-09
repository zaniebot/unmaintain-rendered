---
number: 14997
title: "How to \"uv pip install localfilename.whl\""
type: issue
state: closed
author: Alexisxty
labels:
  - question
assignees: []
created_at: 2025-07-31T15:03:55Z
updated_at: 2025-07-31T15:46:57Z
url: https://github.com/astral-sh/uv/issues/14997
synced_at: 2026-01-07T13:12:19-06:00
---

# How to "uv pip install localfilename.whl"

---

_Issue opened by @Alexisxty on 2025-07-31 15:03_

### Question

When I use "uv pip install localfilename.whl," it directly installs into my conda virtual environment. However, when I run "uv run python test.py," it indicates that the package cannot be found. How can I resolve this issue?


### Platform

centos

### Version

uv 0.7.20

---

_Label `question` added by @Alexisxty on 2025-07-31 15:03_

---

_Comment by @zanieb on 2025-07-31 15:18_

Please share some logs for the `uv run` invocation with the `-v` flag.

---

_Comment by @Alexisxty on 2025-07-31 15:21_

> Please share some logs for the `uv run` invocation with the `-v` flag.

thanks ,I have found a solution by using "uv add filename.whl." Subsequently, I was able to call it by executing "uv run python xx.py." I noticed in a previous issue that the suggested answer was "uv pip ./filename.whl," which is an incorrect response.

---

_Comment by @zanieb on 2025-07-31 15:46_

Great thanks!

---

_Closed by @zanieb on 2025-07-31 15:46_

---
