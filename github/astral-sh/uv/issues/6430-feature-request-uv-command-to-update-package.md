---
number: 6430
title: "[Feature request] `uv` command to update package version in `pyproject.toml`"
type: issue
state: closed
author: albertferras-vrf
labels:
  - duplicate
assignees: []
created_at: 2024-08-22T13:18:10Z
updated_at: 2024-08-22T13:33:24Z
url: https://github.com/astral-sh/uv/issues/6430
synced_at: 2026-01-07T13:12:17-06:00
---

# [Feature request] `uv` command to update package version in `pyproject.toml`

---

_Issue opened by @albertferras-vrf on 2024-08-22 13:18_

Currently, we have a command to add dependencies to `pyproject.toml` like this: `uv add package==1.2.3`
It would be nice if we could also bump our package version (project.version) through a command, too. 

I have two different use cases which would get simpler to do:
- Our CI builds and release 'dev version' packages on every commit
- We have >5 packages in some of our projects, which must all be bumped together when any of them changes (they all depend on some common lib, kind of similar to `botocore` stuff), so we have custom script to update all `version` from all packages.


---

_Comment by @zanieb on 2024-08-22 13:33_

Duplicate of https://github.com/astral-sh/uv/issues/6298

---

_Closed by @zanieb on 2024-08-22 13:33_

---

_Label `duplicate` added by @zanieb on 2024-08-22 13:33_

---

_Comment by @zanieb on 2024-08-22 13:33_

Totally agree though :)

---
