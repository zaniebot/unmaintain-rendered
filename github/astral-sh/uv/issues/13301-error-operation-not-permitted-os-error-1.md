---
number: 13301
title: "error: Operation not permitted (os error 1)"
type: issue
state: closed
author: Cheiklf
labels:
  - question
assignees: []
created_at: 2025-05-05T13:21:23Z
updated_at: 2025-05-20T13:30:38Z
url: https://github.com/astral-sh/uv/issues/13301
synced_at: 2026-01-07T13:12:18-06:00
---

# error: Operation not permitted (os error 1)

---

_Issue opened by @Cheiklf on 2025-05-05 13:21_

### Question

hello,
I just follow the installation, every thing is ok, version is 7.2.
I make a uv init
then uv add
'''
$ uv add ruff
Using CPython 3.12.10
Creating virtual environment at: .venv
error: Operation not permitted (os error 1) '''

then I have this error : error: Operation not permitted (os error 1)

I'm the only user, and I have all permision

tk for help

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @Cheiklf on 2025-05-05 13:21_

---

_Comment by @konstin on 2025-05-05 13:25_

Hi, can you please fill out the other fields and share the verbose logs? See https://github.com/astral-sh/uv/issues/9452 for more details

---

_Comment by @Cheiklf on 2025-05-05 13:51_

OK sorry, 
I use WSL2 linux Ubuntu
after install, I just make : 
uv init llm2
cd llm2
uv add ruff

![Image](https://github.com/user-attachments/assets/3f9f2880-7ccb-450f-b5d9-79d6833ff7be)

![Image](https://github.com/user-attachments/assets/5499bc7a-294a-410a-97c2-8f9026a52de2)

---

_Comment by @konstin on 2025-05-05 19:14_

Is the directory mounted from a Windows partition? Windows does not support creating symlinks by default, while the linux in WSL requires creating symlinks for a new venv.

We're improving the error message in https://github.com/astral-sh/uv/pull/13303.

---

_Closed by @charliermarsh on 2025-05-20 13:30_

---
