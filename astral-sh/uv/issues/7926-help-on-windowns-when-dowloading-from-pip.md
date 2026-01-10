---
number: 7926
title: Help on Windowns when dowloading from pip
type: issue
state: closed
author: brunocarlin
labels:
  - question
  - windows
assignees: []
created_at: 2024-10-04T14:22:05Z
updated_at: 2024-10-04T15:28:19Z
url: https://github.com/astral-sh/uv/issues/7926
synced_at: 2026-01-10T01:24:21Z
---

# Help on Windowns when dowloading from pip

---

_Issue opened by @brunocarlin on 2024-10-04 14:22_

Hi, how do I register uv to the command line when doing a pip install of uv, the reason I want that is because uv is not findable by the venv's I am using because it is only installed on my base python

---

_Comment by @zanieb on 2024-10-04 14:27_

Maybe you want `pip install --system uv` or `pipx install uv`? I'm not sure how Windows handles the base Python's bin directory. I'd really recommend just not installing from pip.

---

_Label `question` added by @zanieb on 2024-10-04 14:27_

---

_Label `windows` added by @zanieb on 2024-10-04 14:27_

---

_Comment by @brunocarlin on 2024-10-04 14:32_

I can't not install from pip because my company only allow installations from pip in this case...

---

_Comment by @brunocarlin on 2024-10-04 14:33_

Just to be clear uv does work and is installed on system but I can`t from a venv use python-m uv...

---

_Comment by @zanieb on 2024-10-04 14:35_

Oh why are you using `python-m uv` instead of just `uv`? You could provide the path to your system interpreter... i.e., before activating your environment `which python` then `\path\to\python -m uv`. I imagine you can create an alias for that?

---

_Comment by @brunocarlin on 2024-10-04 14:36_

I trid to add the uv path to system variables, but that didn't work, but honestly I don't know enough windows to create an alias that when I would run uv it would point to that

---

_Comment by @zanieb on 2024-10-04 14:40_

I think you'll need to just look up how to add a path to your system — there's not much we can do for you here. This is why we have a separate installer that handles these things.

---

_Comment by @brunocarlin on 2024-10-04 14:43_

when I add to path which uv file should I use?

---

_Comment by @brunocarlin on 2024-10-04 14:44_

I think I can solve this be using temp names and aliases and doskey, thanks for the ideas!

---

_Closed by @brunocarlin on 2024-10-04 15:28_

---
