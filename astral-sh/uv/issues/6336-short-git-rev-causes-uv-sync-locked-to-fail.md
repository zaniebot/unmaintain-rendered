---
number: 6336
title: "Short Git rev causes `uv sync --locked` to fail"
type: issue
state: closed
author: hackedd
labels:
  - bug
assignees: []
created_at: 2024-08-21T15:02:50Z
updated_at: 2024-08-21T18:49:57Z
url: https://github.com/astral-sh/uv/issues/6336
synced_at: 2026-01-10T01:23:59Z
---

# Short Git rev causes `uv sync --locked` to fail

---

_Issue opened by @hackedd on 2024-08-21 15:02_

When I specify a Git dependency with a short `rev`, running `uv sync --locked` fails (telling me the lockfile needs to be updated), although `uv lock` makes no changes.

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "httpx",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv.sources]
httpx = { git = "https://github.com/encode/httpx", rev = "7c0cda" }
```

```console
$ uv sync
Resolved 8 packages in 0.78ms
Audited 8 packages in 0.02ms
$ uv sync --locked
 Updated https://github.com/encode/httpx (7c0cda1)
Resolved 8 packages in 9ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

uv 0.3.0 running on Linux.

Using the complete commit hash resolves the problem.

---

_Comment by @charliermarsh on 2024-08-21 15:03_

Thanks that looks like a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-21 15:03_

---

_Label `bug` added by @charliermarsh on 2024-08-21 15:03_

---

_Referenced in [astral-sh/uv#6341](../../astral-sh/uv/pulls/6341.md) on 2024-08-21 15:23_

---

_Closed by @charliermarsh on 2024-08-21 15:34_

---

_Closed by @charliermarsh on 2024-08-21 15:34_

---

_Comment by @hackedd on 2024-08-21 18:49_

Wow, thanks for the quick fix :)

---
