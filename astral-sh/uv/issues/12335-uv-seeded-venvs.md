---
number: 12335
title: uv-seeded venvs
type: issue
state: closed
author: orent
labels:
  - enhancement
assignees: []
created_at: 2025-03-20T10:14:24Z
updated_at: 2025-03-23T11:22:09Z
url: https://github.com/astral-sh/uv/issues/12335
synced_at: 2026-01-10T01:25:18Z
---

# uv-seeded venvs

---

_Issue opened by @orent on 2025-03-20 10:14_

### Summary

The --seed argument installs pip into the venv. Why pip and not the superior uv pip implementation?

Suggestion:

The --seed=uv argument will seed the venv with uv and also add an alias named "pip" for compatbility that runs "uv pip". Could be a script or a symlink ( + uv executable detecting if argv0 is "pip" to run the pip subcommand)

Particularly useful for the un-activated environment workflow

Consider actually making this the default for --seed rather than requiring --seed=uv


### Example

_No response_

---

_Label `enhancement` added by @orent on 2025-03-20 10:14_

---

_Comment by @FishAlchemist on 2025-03-20 10:41_

Similar or related to the following:
* https://github.com/astral-sh/uv/issues/1657
* https://github.com/astral-sh/uv/issues/12199

---

_Comment by @zanieb on 2025-03-20 13:15_

And 

- #10949
- #1331

---

_Closed by @zanieb on 2025-03-20 13:15_

---

_Comment by @orent on 2025-03-23 11:22_

Whether or not “uv pip” will be available as “pip” is secondary. The point is having a single command to install an environment pre seeded with uv.

---
