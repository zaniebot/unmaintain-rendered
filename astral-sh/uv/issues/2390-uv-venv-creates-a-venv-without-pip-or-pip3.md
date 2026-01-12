```yaml
number: 2390
title: "`uv venv` creates a venv without pip or pip3"
type: issue
state: closed
author: lukecivantos
labels:
  - question
assignees: []
created_at: 2024-03-12T19:38:29Z
updated_at: 2024-03-12T20:03:55Z
url: https://github.com/astral-sh/uv/issues/2390
synced_at: 2026-01-12T15:58:37Z
```

# `uv venv` creates a venv without pip or pip3

---

_@lukecivantos_

Hey all – our team is running into issues with running `uv venv` locally where it's created without pip/pip3. 

Is this by design as pip should only be called with `uv`? 

When working locally we use `uv venv --python=$(which python3.11)` which creates the `.venv` folder, but in `bin` there's no pip or pip3. 

If I run `python3.11 -m venv .venv` this doesn't happen. We're on version `0.1.17` of uv. 


Here's a screenshot of first the `bin` folder when running uv venv and then after the folder when running the original call.  

<img width="779" alt="image" src="https://github.com/astral-sh/uv/assets/16458635/c2ca7c02-2570-4f89-afbc-a0d2b67ee5cf">

<img width="777" alt="image" src="https://github.com/astral-sh/uv/assets/16458635/359cafa6-7666-458d-ba04-db9c5593cd50">

Thanks!



---

_Comment by @charliermarsh on 2024-03-12 19:43_

You can run `uv venv --seed` to include `pip`. We don't install `pip` into virtual environments by default, since `uv` on its own is sufficient to install, etc. (It's _supposed_, it's just not the default.)

---

_Label `question` added by @charliermarsh on 2024-03-12 19:46_

---

_Comment by @lukecivantos on 2024-03-12 20:03_

Thanks! If it helps for context – Being able to include mypy just helped us debug some issues around local mypy package issues and the venv looking at the on-device homebrew installs (which we didn't want)  

I'm going to remove `--seed` now, but it was helpful to have as a resource!

---

_Comment by @zanieb on 2024-03-12 20:03_

Thanks!

---

_Closed by @zanieb on 2024-03-12 20:03_

---
