---
number: 5950
title: Activation prompt with invalid path on Windows Git Bash
type: issue
state: closed
author: BenediktMaag
labels:
  - bug
assignees: []
created_at: 2024-08-09T05:57:49Z
updated_at: 2024-08-09T16:28:01Z
url: https://github.com/astral-sh/uv/issues/5950
synced_at: 2026-01-10T01:23:54Z
---

# Activation prompt with invalid path on Windows Git Bash

---

_Issue opened by @BenediktMaag on 2024-08-09 05:57_

Windows 11, uv 0.2.34 (c681c5a33 2024-08-07)

Activating a environment after creating it:

$ uv v
Using Python 3.12.0 interpreter at: C:\Users\MaagB\AppData\Local\Programs\Python\Python312\python.exe
Creating virtualenv at: .venv
Activate with: source .venv\Scripts\activate

$  source .venv\Scripts\activate
bash: .venvScriptsactivate: No such file or directory

Problem:
git bash on windows seems to resolve the escape sequences before making the source call. To properly activate the environment we either need to properly espace the backslash by another backslash:

$ source .venv\\Scripts\\activate
(tmpinstall)

Or use the Unix Path:
$ source .venv/Scripts/activate
(tmpinstall)


---

_Label `bug` added by @zanieb on 2024-08-09 13:31_

---

_Comment by @zanieb on 2024-08-09 13:31_

Thanks for the report!

---

_Comment by @charliermarsh on 2024-08-09 13:32_

Do we need to treat Windows Git Bash as its own shell?

---

_Comment by @zanieb on 2024-08-09 13:34_

I think it should be sufficient to always use the portable path

---

_Referenced in [astral-sh/uv#5956](../../astral-sh/uv/pulls/5956.md) on 2024-08-09 13:35_

---

_Comment by @zanieb on 2024-08-09 13:36_

As in https://github.com/astral-sh/uv/pull/5956

It might make sense to special-case Windows Git Bash still, we see oddities with it.

---

_Closed by @zanieb on 2024-08-09 16:28_

---
