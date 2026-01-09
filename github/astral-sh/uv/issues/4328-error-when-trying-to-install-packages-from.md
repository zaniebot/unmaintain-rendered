---
number: 4328
title: Error when trying to install packages from multiple directories from monorepo
type: issue
state: open
author: fervand1
labels:
  - bug
  - windows
  - cache
assignees: []
created_at: 2024-06-14T14:54:38Z
updated_at: 2024-06-15T20:24:01Z
url: https://github.com/astral-sh/uv/issues/4328
synced_at: 2026-01-07T13:12:17-06:00
---

# Error when trying to install packages from multiple directories from monorepo

---

_Issue opened by @fervand1 on 2024-06-14 14:54_

When trying to install multiple local packages with

`uv pip install package1@D:\package_dir\package1_dir package2@D:\package_dir\package2_dir package3@D:\package_dir\package3_dir ...`

I see the following error. I am trying to install more than 20+ such packages together.

![image](https://github.com/astral-sh/uv/assets/44014460/844bfa09-7163-4495-bc69-6a0e0598b89c)

Also note, when I try to do editable installs they work absolutely fine.


---

_Comment by @fervand1 on 2024-06-15 17:30_

I was able to get it working by setting the environment variable `UV_CONCURRENT_INSTALLS` to 1

---

_Closed by @fervand1 on 2024-06-15 17:30_

---

_Comment by @notatallshaw on 2024-06-15 18:32_

@charliermarsh FYI, I see this issue has been closed with a workaround, but I still think that it happens should be on your radar.

---

_Comment by @zanieb on 2024-06-15 19:44_

We've seen this error on Windows before but I thought we had addressed it

---

_Reopened by @zanieb on 2024-06-15 19:44_

---

_Label `windows` added by @zanieb on 2024-06-15 19:44_

---

_Label `cache` added by @zanieb on 2024-06-15 19:44_

---

_Label `bug` added by @zanieb on 2024-06-15 19:44_

---

_Comment by @fervand1 on 2024-06-15 20:24_

@zanieb I used the latest version of uv for my tests today. So it should still be reproducible. Only way I was able to get it working was with the env variable set. If it can be addressed it will be nice. Thanks

---

_Referenced in [pypa/pip#12816](../../pypa/pip/pulls/12816.md) on 2024-07-01 16:45_

---
