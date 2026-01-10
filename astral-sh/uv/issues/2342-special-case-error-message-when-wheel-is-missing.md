---
number: 2342
title: "Special case error message when `wheel` is missing with no build isolation"
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2024-03-10T16:39:17Z
updated_at: 2024-03-10T17:35:56Z
url: https://github.com/astral-sh/uv/issues/2342
synced_at: 2026-01-10T01:23:16Z
---

# Special case error message when `wheel` is missing with no build isolation

---

_Issue opened by @konstin on 2024-03-10 16:39_

              I have problems with [`xformers` by Meta](https://github.com/facebookresearch/xformers) (see [issue](https://github.com/facebookresearch/xformers/issues/940)). I even tried with latest `uv` _without build isolation_:

```bash
❯ uv pip install setuptools torch numpy
Resolved 11 packages in 2ms
Installed 1 package in 23ms
 + numpy==1.26.4

❯ uv pip install xformers --no-build-isolation
Resolved 11 packages in 2ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: xformers==0.0.24
  Caused by: Failed to build: xformers==0.0.24
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
usage: setup.py [global_opts] cmd1 [cmd1_opts] [cmd2 [cmd2_opts] ...]
   or: setup.py --help [cmd1 cmd2 ...]
   or: setup.py --help-commands
   or: setup.py cmd --help

error: invalid command 'bdist_wheel'
```

_Originally posted by @baggiponte in https://github.com/astral-sh/uv/issues/2252#issuecomment-1985287074_
            

---

_Assigned to @konstin by @konstin on 2024-03-10 16:39_

---

_Closed by @konstin on 2024-03-10 16:41_

---

_Comment by @baggiponte on 2024-03-10 17:35_

Also posting the solution provided by @charliermarsh : you just have to install `wheel`.

(Also: will you eventually re-implement `wheel` so that `uv` can handle these cases?)

---

_Referenced in [astral-sh/uv#4069](../../astral-sh/uv/issues/4069.md) on 2024-06-05 20:22_

---
