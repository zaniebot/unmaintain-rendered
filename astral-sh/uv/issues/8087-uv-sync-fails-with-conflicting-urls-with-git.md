---
number: 8087
title: "`uv sync` fails with conflicting URLs with Git workspace"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2024-10-10T12:17:50Z
updated_at: 2024-10-30T19:12:24Z
url: https://github.com/astral-sh/uv/issues/8087
synced_at: 2026-01-10T01:24:23Z
---

# `uv sync` fails with conflicting URLs with Git workspace

---

_Issue opened by @charliermarsh on 2024-10-10 12:17_

Here is an example:
```
[project]
name = "example"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "autogen-core==0.4.0dev0",
    "autogen-agentchat==0.4.0dev0",
    "autogen-ext==0.4.0dev0"
]

[tool.uv.sources]
autogen-core = { git = "https://github.com/microsoft/autogen.git", branch = "staging", subdirectory = "python/packages/autogen-core" }
autogen-agentchat = { git = "https://github.com/microsoft/autogen.git", branch = "staging", subdirectory = "python/packages/autogen-agentchat" }
autogen-ext = { git = "https://github.com/microsoft/autogen.git", branch = "staging", subdirectory = "python/packages/autogen-ext" }
```

as I had the same issue while trying to load an un-published package from Git:
```
error: Requirements contain conflicting URLs for package `autogen-core` in split `python_full_version == '3.12.*'`:
```

(note that you'll need to run `GIT_LFS_SKIP_SMUDGE=1 uv sync --verbose` to reproduce, as there's a different and unrelated error about LFS when uv tries to use git to check the repo out, but that's an issue for another ticket)

_Originally posted by @jasongill in https://github.com/astral-sh/uv/issues/6041#issuecomment-2402555793_
            

---

_Comment by @charliermarsh on 2024-10-10 12:19_

This is an interesting issue. The final error is like:

```
error: Requirements contain conflicting URLs for package `autogen-core`:
- file:///Users/crmarsh/.cache/uv/git-v0/checkouts/2b1782d8bf4da9cf/4084a9f3/python/packages/autogen-core
- git+https://github.com/microsoft/autogen.git@staging#subdirectory=python/packages/autogen-core
```

`autogen` actually uses uv workspaces itself, which is the source of the problem.

---

_Label `bug` added by @charliermarsh on 2024-10-10 12:19_

---

_Comment by @jasongill on 2024-10-10 12:52_

Thanks for opening this ticket @charliermarsh . I realized after reporting in the other thread that there was an issue which I couldn't figure out if it was user error (by me), an issue in the autogen repo, or an actual bug so I hesitated to report it further.

But I noticed in the autogen-core pyproject.toml the version is specified as 0.4.0dev0, and when I try to install that package alone it works. However, when I try to install one of its dependencies like autogen-ext or autogen-autochat, the failure appears to be because the uv resolver is trying to find autogen-core 0.4.0.dev0 - note the extra period before dev0.

Again, not sure if this is a uv bug or just I am doing it wrong, but it was odd that the version string was not matching up which appears to be causing this error.

---

_Assigned to @konstin by @charliermarsh on 2024-10-10 13:03_

---

_Comment by @charliermarsh on 2024-10-10 13:03_

Assigning to @konstin.

---

_Referenced in [astral-sh/uv#8665](../../astral-sh/uv/pulls/8665.md) on 2024-10-29 16:07_

---

_Closed by @charliermarsh on 2024-10-30 19:12_

---

_Closed by @charliermarsh on 2024-10-30 19:12_

---
