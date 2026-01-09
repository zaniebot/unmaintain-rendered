---
number: 15763
title: Why does uv always automatically switch to Python 3.11?
type: issue
state: open
author: ConanZH429
labels:
  - question
assignees: []
created_at: 2025-09-10T03:34:11Z
updated_at: 2025-10-30T01:21:25Z
url: https://github.com/astral-sh/uv/issues/15763
synced_at: 2026-01-07T13:12:19-06:00
---

# Why does uv always automatically switch to Python 3.11?

---

_Issue opened by @ConanZH429 on 2025-09-10 03:34_

### Question

I have executed the following commands:
uv python install 3.12
uv init --python 3.12
uv venv --python 3.12
uv python pin 3.12
..venv\Scripts\activate

Then I ran:
uv add numpy

After that, uv automatically started installing Python 3.11, and subsequently displayed the following prompt:
Using CPython 3.11.13
error: The requested interpreter resolved to Python 3.11.13, which is incompatible with the project's Python requirement: >=3.12 (from project.requires-python)

This is quite strange. Why does it always automatically switch to Python 3.11?

### Platform

Windows11

### Version

uv 0.8.16 (2de677b0d 2025-09-09)

---

_Label `question` added by @ConanZH429 on 2025-09-10 03:34_

---

_Comment by @konstin on 2025-09-10 08:07_

I can't reproduce this, can you share verbose logs (`uv add numpy -v`)?

---

_Comment by @nbaju1 on 2025-09-10 09:43_

I can reproduce this if there is a `.python-version` file with `3.11` already present in the same folder when running those commands.

Same if the env var `UV_PYTHON` is set to `3.11`.

---

_Comment by @Yuli-yx on 2025-09-12 02:23_

> I can't reproduce this, can you share verbose logs (`uv add numpy -v`)?

Hi, I have the issue. This is my logs, it really confuses me since I set all python version to be 3.12, and uv always switches to python 3.11.

<img width="1103" height="442" alt="Image" src="https://github.com/user-attachments/assets/e56827fc-9e0d-46bc-bc72-19ebbb8e0c7f" />

---

_Comment by @nbaju1 on 2025-09-12 06:09_

> > I can't reproduce this, can you share verbose logs (`uv add numpy -v`)?
> 
> Hi, I have the issue. This is my logs, it really confuses me since I set all python version to be 3.12, and uv always switches to python 3.11.
> 

Can you confirm that you don't have the `.python-version` file set or the `UV_PYTHON` env var set by running `Get-Content .python-version` and `Get-ChildItem Env:UV_PYTHON` (assuming you're running in powershell) before running the uv commands in the same directory?


---

_Comment by @zanieb on 2025-09-12 11:48_

"from explicit request" indicates that you've set `UV_PYTHON=3.11`

---

_Comment by @Yuli-yx on 2025-09-12 14:14_

> > > I can't reproduce this, can you share verbose logs (`uv add numpy -v`)?
> > 
> > 
> > Hi, I have the issue. This is my logs, it really confuses me since I set all python version to be 3.12, and uv always switches to python 3.11.
> 
> Can you confirm that you don't have the `.python-version` file set or the `UV_PYTHON` env var set by running `Get-Content .python-version` and `Get-ChildItem Env:UV_PYTHON` (assuming you're running in powershell) before running the uv commands in the same directory?

Thank you for reply. 
I've explicitly set .python-verison to be 3.12. For `UV_PYTHON`, I will try to run your command as soon as I get back to work. I already checked the user env variable and system env variable in windows setting and there was a UV_PYTHON= 3.11 in user env variable but the problem still exists after i manually deleted it or changed it to 3.12.

---

_Comment by @ConanZH429 on 2025-10-30 01:21_

In addition to these steps @Yuli-yx , I also completely uninstalled UV and reinstalled it twice before resolving the issue.

---
