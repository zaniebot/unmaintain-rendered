---
number: 11797
title: "Installation of workspace member's dev dependencies depends on whether the root has a [project] section"
type: issue
state: closed
author: thorbenk
labels:
  - question
assignees: []
created_at: 2025-02-26T11:40:55Z
updated_at: 2025-03-14T00:39:09Z
url: https://github.com/astral-sh/uv/issues/11797
synced_at: 2026-01-07T13:12:18-06:00
---

# Installation of workspace member's dev dependencies depends on whether the root has a [project] section

---

_Issue opened by @thorbenk on 2025-02-26 11:40_

### Question

In my minimal "project with workspace member" setup (https://github.com/thorbenk/uv_workspace_dev_issue , just run `./issue.sh` to reproduce),
I have the following:

- a workspace member `mypkg`
  - declares a dev dependency on `loguru`
- a root `pyproject.toml` file  

I want to understand when the development dependency is part of the virtual environment after `uv sync`.
Apparently it differs depending on whether the root `pyproject.toml` declares a project or not.

If I just have

```toml
[tool.uv.workspace]
members = ["packages/mypkg"]
```

the **dev dependency is installed**.

However, if I have

```toml
[project]
name = "wsplay_root"
version = "0.1.0"
requires-python = ">=3.12.6"
dependencies = ["mypkg"]

[tool.uv.workspace]
members = ["packages/mypkg"]

[tool.uv.sources]
mypkg = { workspace = true }
```

then **the dev dependency ist NOT installed**.

Is this a bug? Or expected behavior?

### Platform

_No response_

### Version

uv 0.6.3

---

_Label `question` added by @thorbenk on 2025-02-26 11:40_

---

_Comment by @thorbenk on 2025-02-26 12:01_

Reading more issues, I've found https://github.com/astral-sh/uv/issues/10934#issuecomment-2630376653 running into a similar question.

---

_Comment by @charliermarsh on 2025-02-27 14:05_

Expected because if you omit a `[project]` table, we install all workspace packages by default, and the dev group is enabled by default; whereas in the second example, you're installing `wsplay_root`, which depends on `mypkg`. So dev dependencies would be installed if defined in `wsplay_root`, but not in `mypkg`, which is just treated like any other dependency.

In the first example, think of it as if `--all-packages` were passed to `uv sync`.

As an aside, "projects without a `[project]` table" are considered legacy.

---

_Closed by @charliermarsh on 2025-03-14 00:39_

---
