---
number: 9072
title: uv init needlessly tries to discover a venv
type: issue
state: closed
author: rdaysky
labels:
  - bug
assignees: []
created_at: 2024-11-12T23:27:57Z
updated_at: 2024-11-13T03:53:58Z
url: https://github.com/astral-sh/uv/issues/9072
synced_at: 2026-01-07T13:12:18-06:00
---

# uv init needlessly tries to discover a venv

---

_Issue opened by @rdaysky on 2024-11-12 23:27_

The existence of a directory named `.venv` in an ancestor of the current directory seems to influence the behavior of `uv init`. In particular, it will look for the version of Python installed in it and put it into requires-python. This is problematic because:

1. By definition, uv init is going to create a new project which will define, in its own pyproject.toml, what it wants from the environment it’s going to run in. That some other thing that resides in a parent directory happened to configure its own venv in a particular way is irrelevant.
2. The directory named `.venv` isn’t necessarily a fully-functional virtual environment. If it was created within a Docker container and became visible to the host because of a volume mount, the symlinks inside will likely appear to be broken and `uv init` will fail entirely. Or maybe the user has a ~/.venv directory that’s just a container for various venvs and is not a venv itself.
3. There doesn’t seem to be a switch to disable this discovery.
4. It’s undocumented.

Given that uv can install Python like any other package, is there any reason not to follow the same process as with any other package and put the latest version into requires-python, ignoring all .venv directories, at least for `--app`? (`--lib`s in general require permissive versioning so there should be some heuristic about what to put into requires-python, but that’s a different question.)

---

_Comment by @bluss on 2024-11-12 23:46_

This is maybe similar to issue #8092

---

_Comment by @zanieb on 2024-11-13 01:08_

I think this sounds like a bug, as-in, we should not read from virtual environments when finding an interpreter for `uv init`. 

---

_Label `bug` added by @zanieb on 2024-11-13 01:08_

---

_Referenced in [astral-sh/uv#9075](../../astral-sh/uv/pulls/9075.md) on 2024-11-13 01:23_

---

_Closed by @zanieb on 2024-11-13 03:53_

---
