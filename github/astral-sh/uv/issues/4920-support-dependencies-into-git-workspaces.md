---
number: 4920
title: Support dependencies into git workspaces
type: issue
state: closed
author: konstin
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-07-09T10:22:33Z
updated_at: 2024-10-30T19:12:25Z
url: https://github.com/astral-sh/uv/issues/4920
synced_at: 2026-01-07T13:12:17-06:00
---

# Support dependencies into git workspaces

---

_Issue opened by @konstin on 2024-07-09 10:22_

Say we have our local workspace with a package `a`, and another workspace in a git repository with `c` and `d`. We have `a -> c`, `c -> d`. Currently, this fails by resolving `c` to the git repository but `d` to pypi.

To reproduce:

```toml
[project]
name = "a"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["c"]

[tool.uv.sources]
c = { git = "https://github.com/konstin/workspace-git-path-dep-test", subdirectory = "packages/c" }
```

https://github.com/konstin/workspace-git-path-dep-test contains the packages `c` and `d`.

---

_Label `bug` added by @konstin on 2024-07-09 10:22_

---

_Label `preview` added by @konstin on 2024-07-09 10:22_

---

_Referenced in [astral-sh/uv#3943](../../astral-sh/uv/issues/3943.md) on 2024-07-09 10:23_

---

_Label `help wanted` added by @konstin on 2024-07-09 12:36_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Referenced in [astral-sh/uv#8665](../../astral-sh/uv/pulls/8665.md) on 2024-10-29 16:07_

---

_Closed by @charliermarsh on 2024-10-30 19:12_

---

_Closed by @charliermarsh on 2024-10-30 19:12_

---
