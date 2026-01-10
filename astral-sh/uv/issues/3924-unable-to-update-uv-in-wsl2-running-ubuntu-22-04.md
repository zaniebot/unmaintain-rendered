---
number: 3924
title: Unable to update uv in WSL2 running Ubuntu 22.04
type: issue
state: closed
author: schergr
labels:
  - question
assignees: []
created_at: 2024-05-30T12:54:24Z
updated_at: 2024-10-21T21:29:31Z
url: https://github.com/astral-sh/uv/issues/3924
synced_at: 2026-01-10T01:23:32Z
---

# Unable to update uv in WSL2 running Ubuntu 22.04

---

_Issue opened by @schergr on 2024-05-30 12:54_

When I run the install of uv inside of a WSL2 Ubuntu instance following the Linux install procedure as follows. 

```
schergr@212SAWKS2:~$ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.2.5 x86_64-unknown-linux-gnu
installing to /home/schergr/.cargo/bin
  uv
everything's installed!
schergr@212SAWKS2:~$
```
I an expecting to get the v0.2.5 version of uv. However, when I run uv -V I get the following
```

schergr@212SAWKS2:~$ uv -V
uv 0.1.39
```
When I run install via pip as follows....```

schergr@212SAWKS2:~$ pip install uv
Defaulting to user installation because normal site-packages is not writeable
Requirement already satisfied: uv in ./.local/lib/python3.10/site-packages (0.1.39)
```
I get the following, and no update is available.

I may be missing something, but the documentation does not cover this scenario as far as I can tell.

---

_Comment by @charliermarsh on 2024-05-30 13:36_

What does “which uv” show? You likely have uv installed from some other source (like pip) and it’s taking precedence on PATH. 

---

_Label `question` added by @charliermarsh on 2024-05-30 17:44_

---

_Closed by @zanieb on 2024-10-21 21:29_

---
