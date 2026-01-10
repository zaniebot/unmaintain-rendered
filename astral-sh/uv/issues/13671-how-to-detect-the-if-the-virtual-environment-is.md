---
number: 13671
title: How to detect the if the virtual environment is managed by UV ?
type: issue
state: closed
author: Ramesh-kumar-S
labels:
  - question
assignees: []
created_at: 2025-05-27T04:38:11Z
updated_at: 2025-05-27T11:36:03Z
url: https://github.com/astral-sh/uv/issues/13671
synced_at: 2026-01-10T01:25:36Z
---

# How to detect the if the virtual environment is managed by UV ?

---

_Issue opened by @Ramesh-kumar-S on 2025-05-27 04:38_

### Question

Context:
- If the virtual environment is created by pipenv, PIPENV_ACTIVE environment variable is set , so similarly is there any way to detect as if the Venv is managed by UV ?

### Platform

5.14.0-427.40.1.el9_4.x86_64

### Version

uv 0.6.17

---

_Label `question` added by @Ramesh-kumar-S on 2025-05-27 04:38_

---

_Comment by @FishAlchemist on 2025-05-27 07:39_

I briefly looked through the **pipenv** source code. It seems pipenv just uses **virtualenv** to create a virtual environment, and then it puts some additional files into that virtual environment's directory.
Regarding **PIPENV_ACTIVE**, it's actually an environment variable that gets added when you run ``pipenv shell`` to activate the virtual environment. It's not used to determine whether a virtual environment is managed by Pipenv.

However, if you just want to know if a virtual environment was created by uv, you can check the **pyvenv.cfg** file inside the virtual environment's folder. This file contains the version of uv that created the environment, which should tell you whether it was created by uv.
e.g. 
```pyvenv.cfg
home = ...
implementation = CPython
uv = 0.6.6
version_info = 3.12.8
include-system-site-packages = false
seed = true
prompt = ...
```
Oh, and if you're running a program using uv (like with ``uv run``), an environment variable named **UV** will also be present.
https://docs.astral.sh/uv/configuration/environment/#uv

---

_Comment by @konstin on 2025-05-27 08:39_

uv doesn't fundamentally differentiate between venvs created by uv and those created by other tools, it can use venvs by other tools and other tools can use uv venvs.

---

_Comment by @Ramesh-kumar-S on 2025-05-27 09:51_

I see, sounds good, Thanks @FishAlchemist, @konstin. 
Just expecting exactly the same approach what you suggested. I'll hold it with .pyvenv config file to differentiate between actual venv  vs uv venv.

---

_Comment by @Ramesh-kumar-S on 2025-05-27 09:53_

Once the venv has been activated, As with PIPENV_ACTIVE I am able to make out that respective venv has been managed by Pipenv, Yet when It comes to venv module and uv, I couldn't find any difference post activation. Hence the query.

---

_Closed by @charliermarsh on 2025-05-27 11:36_

---
