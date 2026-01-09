---
number: 16400
title: Update all venvs (python minor version)
type: issue
state: closed
author: s-banach
labels:
  - enhancement
assignees: []
created_at: 2025-10-21T21:34:39Z
updated_at: 2025-11-08T23:09:23Z
url: https://github.com/astral-sh/uv/issues/16400
synced_at: 2026-01-07T13:12:19-06:00
---

# Update all venvs (python minor version)

---

_Issue opened by @s-banach on 2025-10-21 21:34_

### Summary

I have maybe 20 venvs for different projects on my laptop, all using python 3.12.
There are two different "batch update" operations I would like to perform on all at once:
- Update the minor version of python to the latest 3.12.x
- Update a package in all of them, e.g. ruff

It seems like the first operation could be be useful to more people besides me.
I can see how the second operation would be less useful to people working more carefully with lockfiles and CI processes to periodically update the lockfiles, etc.



---

_Label `enhancement` added by @s-banach on 2025-10-21 21:34_

---

_Comment by @zanieb on 2025-10-22 00:12_

We do the former if you install Python with the preview flag enabled, e.g., `uv python install --preview 3.12 && uv venv && uv python upgrade`. It's just not a stabilized behavior yet.

The latter doesn't really match our virtual environment per project workflows, I'm not sure we'll add first-class support for that. 

---

_Closed by @s-banach on 2025-10-23 12:34_

---
