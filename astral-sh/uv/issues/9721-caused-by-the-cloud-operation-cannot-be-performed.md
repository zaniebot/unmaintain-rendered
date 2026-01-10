---
number: 9721
title: "Caused by: The cloud operation cannot be performed on a file with incompatible hardlinks. (os error 396)"
type: issue
state: closed
author: ananthanarayanan431
labels:
  - question
assignees: []
created_at: 2024-12-08T15:56:41Z
updated_at: 2025-02-20T00:14:05Z
url: https://github.com/astral-sh/uv/issues/9721
synced_at: 2026-01-10T01:24:45Z
---

# Caused by: The cloud operation cannot be performed on a file with incompatible hardlinks. (os error 396)

---

_Issue opened by @ananthanarayanan431 on 2024-12-08 15:56_

C:\Users\anant\OneDrive\Desktop\Agent python\chatbot-agent>uv add ruff
Resolved 38 packages in 349ms
Prepared 1 package in 1.54s
error: Failed to install: httpx-0.27.2-py3-none-any.whl (httpx==0.27.2)
  Caused by: failed to hardlink file from C:\Users\anant\AppData\Local\uv\cache\archive-v0\fG93Erur8OMDKBPFItH9s\httpx\_api.py to C:\Users\anant\OneDrive\Desktop\Agent python\chatbot-agent\.venv\Lib\site-packages\httpx\_api.py
  Caused by: The cloud operation cannot be performed on a file with incompatible hardlinks. (os error 396)

Let me know the solution for this issue?

---

_Comment by @charliermarsh on 2024-12-08 16:11_

Unfortunately no, I don't, it's something to do with your operating system and setup. You can set `--link-mode copy` or `UV_LINK_MODE=copy`.

---

_Label `question` added by @charliermarsh on 2024-12-08 16:11_

---

_Comment by @shapedthought on 2024-12-22 20:18_

Hi there, I was getting the same issue when installing httpx or requests on Windows 11. But now I'm getting this error.

![Image](https://github.com/user-attachments/assets/d7b3b754-588b-4df3-a681-78823017c3a5)

Any ideas?

---

_Closed by @charliermarsh on 2025-02-16 18:59_

---

_Comment by @ananthanarayanan431 on 2025-02-17 15:46_

uv cache clean

This will probably work

---

_Comment by @quantechgithub on 2025-02-20 00:14_

> uv cache clean
> 
> This will probably work

This worked for me

---
