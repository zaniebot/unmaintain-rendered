---
number: 6413
title: Allow setting venv location in user level config
type: issue
state: closed
author: symroe
labels:
  - duplicate
  - configuration
assignees: []
created_at: 2024-08-22T07:45:10Z
updated_at: 2024-08-22T20:14:09Z
url: https://github.com/astral-sh/uv/issues/6413
synced_at: 2026-01-07T13:12:17-06:00
---

# Allow setting venv location in user level config

---

_Issue opened by @symroe on 2024-08-22 07:45_

For various reasons, I don't keep my `.venv` directory in the same directory as the project (as seems to be increasingly a convention and is default in `uv` currently).

I'm coming from using `virtualenvwrapper` where I could configure a `WORKON_HOME` as the location for the envs. 

I can replicate this for each venv with something like this:

`uv venv ~/.envs/$(basename "$PWD")`

However I think a limitation of this is that `uv` isn't able to automatically activate the venv when running e.g `uv sync`. 

It would be really nice to have the location (prefix to a directory containing them, defaulting to `.`) of the envs exposed in `~/.config/uv/uv.toml`. This, ideally, would then be respected in other commands like `uv sync`. 



---

_Comment by @symroe on 2024-08-22 07:46_

Related https://github.com/astral-sh/uv/issues/5229

---

_Label `configuration` added by @charliermarsh on 2024-08-22 12:29_

---

_Comment by @zanieb on 2024-08-22 14:16_

I think this is a duplicate of https://github.com/astral-sh/uv/issues/1495 (and #5229)

---

_Label `duplicate` added by @zanieb on 2024-08-22 14:16_

---

_Comment by @symroe on 2024-08-22 20:13_

Yep, sorry I missed that one :+1:

---

_Closed by @symroe on 2024-08-22 20:14_

---
