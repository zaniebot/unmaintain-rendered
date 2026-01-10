---
number: 13581
title: "`uv sync --all-extras --all-groups --all-packages` is not installing `git` sources when `workspace` sources are set."
type: issue
state: closed
author: fabioz
labels:
  - question
assignees: []
created_at: 2025-05-21T20:00:30Z
updated_at: 2025-05-21T20:19:34Z
url: https://github.com/astral-sh/uv/issues/13581
synced_at: 2026-01-10T01:25:35Z
---

# `uv sync --all-extras --all-groups --all-packages` is not installing `git` sources when `workspace` sources are set.

---

_Issue opened by @fabioz on 2025-05-21 20:00_

### Summary

I have a structure such that:

`pyproject.toml` in the workspace root has:

```
[tool.uv.sources]
project1 = { workspace = true }
project2 = { workspace = true }
project3 = { workspace = true }
project4 = { git = "https://github.com/repo/name.git", tag = "v0.0.1-alpha" }

[tool.uv.workspace]
members = ["project1/", "project2/", "project3/"]

```

When I run `uv sync --all-extras --all-groups --all-packages`, it's not getting the "project4", only the workspace ones are being installed, I can't figure out why project4 from the git repo isn't being gotten...

### Platform

Windows 11 x64

### Version

uv 0.7.6 (7f3e94a09 2025-05-19

### Python version

3.12.9

---

_Label `bug` added by @fabioz on 2025-05-21 20:00_

---

_Comment by @konstin on 2025-05-21 20:04_

Are you declaring `project4` as a dependency? Only dependencies are being installed, sources are consulted when resolving dependencies, but they do not declare dependencies themselves. 

---

_Label `bug` removed by @konstin on 2025-05-21 20:04_

---

_Label `question` added by @konstin on 2025-05-21 20:04_

---

_Comment by @fabioz on 2025-05-21 20:19_

You're right, argh, sorry for the noise.

---

_Closed by @fabioz on 2025-05-21 20:19_

---
