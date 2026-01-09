---
number: 14866
title: "Add `--no-install-local` to `uv sync`"
type: issue
state: closed
author: danielgafni
labels:
  - enhancement
assignees: []
created_at: 2025-07-24T09:01:39Z
updated_at: 2025-08-22T16:31:53Z
url: https://github.com/astral-sh/uv/issues/14866
synced_at: 2026-01-07T13:12:19-06:00
---

# Add `--no-install-local` to `uv sync`

---

_Issue opened by @danielgafni on 2025-07-24 09:01_

### Summary

This flag would be similar to `--no-install-workspace`, but it would also affect local non-workspace dependencies. 

This is desired in optimized docker builds where we don't want to install frequently changing local packages before installing heavy third party dependencies.  

Currently this workflow is well supported when depending on other packages from the same workspace, but these packages may also come from other workspaces.   

### Example

Current command: 
```shell
uv sync --no-install-workspace \
        --no-install-package anam-face-extraction \
        --no-install-package anam-syncnet \
        --no-install-package anam-data-utils \
        --no-install-package anam-dagster \
        --no-install-package anam-training-data \
        ...
```

Desired command: 
```shell
uv sync --no-install-local
```

---

_Label `enhancement` added by @danielgafni on 2025-07-24 09:01_

---

_Comment by @charliermarsh on 2025-07-24 13:05_

I think this makes sense, IIRC we considered this in-scope at the time but just punted it on it due to lack of demand?

---

_Comment by @zanieb on 2025-07-24 13:09_

Yes we wanted to see if there was a strong justification for it, given that you _can_ just enumerate the packages.

---

_Comment by @danielgafni on 2025-07-24 14:28_

It definitely feels like it should be there. 
I don't want to be maintaining these lists in dozens of Dockerfiles across all the projects I have in different workspaces. 

---

_Referenced in [astral-sh/uv#15328](../../astral-sh/uv/pulls/15328.md) on 2025-08-17 04:45_

---

_Closed by @zanieb on 2025-08-22 16:31_

---
