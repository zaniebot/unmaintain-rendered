---
number: 7906
title: OS error 396 when using OneDrive
type: issue
state: closed
author: Pdsantos73
labels:
  - question
  - windows
  - cache
assignees: []
created_at: 2024-10-03T19:13:45Z
updated_at: 2025-06-30T16:07:03Z
url: https://github.com/astral-sh/uv/issues/7906
synced_at: 2026-01-07T13:12:17-06:00
---

# OS error 396 when using OneDrive

---

_Issue opened by @Pdsantos73 on 2024-10-03 19:13_

Trying to install fastapi "uv add fastapi[standard]" is showing the following error:

error: Failed to install: certifi-2024.8.30-py3-none-any.whl (certifi==2024.8.30)
Caused by: failed to hardlink file from C:\Users\paulodo\AppData\Local\uv\cache\archive-v0\ez_A-yAgnUYbpxYugeHkX\certifi\py.typed to C:\Users\paulodo\OneDrive - Genesys Telecommunications Laboratories, Inc\DeveloperProject\Python\GenesysClould\middleware_a.venv\Lib\site-packages\certifi\py.typed
Caused by: The cloud operation cannot be performed on a file with incompatible hardlinks. (os error 396)

---

_Comment by @zanieb on 2024-10-03 19:41_

It looks like your cache is on a different file system than your project. Can you set `UV_LINK_MODE=copy`?

---

_Label `question` added by @zanieb on 2024-10-03 19:41_

---

_Label `cache` added by @zanieb on 2024-10-03 19:41_

---

_Comment by @zanieb on 2024-10-03 19:41_

cc @charliermarsh we might want a fallback for this specific error.

---

_Comment by @Pdsantos73 on 2024-10-03 21:02_

Yes,

I included  env variable but de issues persistes
![image](https://github.com/user-attachments/assets/1d0490ff-b339-4d77-9459-42d910b20e9a)

![image](https://github.com/user-attachments/assets/e3bb89fa-a472-4afc-9533-63fcb5f53f7d)



---

_Comment by @Pdsantos73 on 2024-10-03 21:06_

I placed the variable with venv activated.

---

_Comment by @Pdsantos73 on 2024-10-03 22:57_

I recently updated UV to the latest version.
This problem only occurs in some dependencies.

---

_Comment by @Pdsantos73 on 2024-10-03 23:23_

I'm using:
Windows 11
uv 0.4.18 (7b55e9790 2024-10-01)
Python 3.12.6

---

_Comment by @Pdsantos73 on 2024-10-03 23:56_

I created a new project and managed to install it with the **uv add "fastapi[standard]" --link-mode=copy**

It worked!!

Thanks

---

_Referenced in [astral-sh/ruff#13612](../../astral-sh/ruff/issues/13612.md) on 2024-10-04 04:28_

---

_Comment by @charliermarsh on 2024-10-11 09:49_

I sort of doubt we can react to this error specifically, unless it maps to a clear error kind on `io::Error`.

---

_Label `windows` added by @zanieb on 2024-10-21 21:56_

---

_Closed by @zanieb on 2024-10-21 21:56_

---

_Comment by @samozturk on 2024-10-24 09:51_

I had the same problem the solutions above didn't work for me. So I did `uv cache clean`, tried to install again. Solved the issue.

---

_Comment by @jromal on 2024-11-29 16:11_

Unfortunately it happens very often to me. I have to clean the cache too.
It may be that I am working on Windows and many python projects are on a "OneDrive" folder, that is considered sometimes as a local folder but sometimes with a different very long name. OneDrive is a shit, but for space reasons I have to use it in the companies laptop.

Someone else having problems with OneDrive?

---

_Comment by @delreluca on 2024-12-03 11:23_

> Someone else having problems with OneDrive?

Yes, at work I also use OneDrive and I experience this same error (os error 396). `uv cache clean` or moving the cache location fixes the problem.

It doesn’t always happen when I use OneDrive, but I have the suspicion that creating a venv on OneDrive and `uv pip install` into it somehow taints the cache and breaks it, even if you later use a different venv,

---

_Renamed from "Failed to install: certifi-2024.8.30-py3-none-any.whl " to "OS error 396 when using OneDrive" by @zanieb on 2024-12-03 14:29_

---

_Referenced in [cedanl/1cijferho#12](../../cedanl/1cijferho/issues/12.md) on 2025-04-01 07:08_

---

_Comment by @TJMac93 on 2025-06-30 16:07_

@delreluca's latest comment mirrors my experience. I didn't have any issues running `uv add {package}` but after absentmindedly doing `uv pip install {package}` instead, that's when everything stopped working

---
