---
number: 6749
title: "Change project to virtual, `uv sync` still installs it"
type: issue
state: closed
author: j178
labels:
  - bug
  - question
assignees: []
created_at: 2024-08-28T13:57:41Z
updated_at: 2024-08-28T15:11:18Z
url: https://github.com/astral-sh/uv/issues/6749
synced_at: 2026-01-10T01:24:05Z
---

# Change project to virtual, `uv sync` still installs it

---

_Issue opened by @j178 on 2024-08-28 13:57_


## Steps to reproduce

```console
$ uv init project --lib
$ cd project
$ uv sync

$ cat pyproject.toml
[project]
name = "project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

$ cat uv.lock
version = 1
requires-python = ">=3.12"

[[package]]
name = "project"
version = "0.1.0"
source = { editable = "." }
```

After removed the `[build-system]` section from `pyproject.toml`, the project should become virtual, but `uv sync` still build and installs the virtual project:

```console
$ cat pyproject.toml
[project]
name = "project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []

$ uv sync -v
DEBUG uv 0.3.5
DEBUG Found project root: `/private/tmp/project`
DEBUG No workspace root found, using project root
DEBUG The virtual environment's Python version satisfies `Python >=3.12`
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `/Users/Jo/.cache/uv/built-wheels-v3/editable/a0e7529ceb99aa48`
DEBUG Found static `pyproject.toml` for: project @ file:///private/tmp/project
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 1 package in 5ms
DEBUG Using request timeout of 30s
DEBUG Requirement installed, but not fresh: project==0.1.0 (from file:///private/tmp/project)
DEBUG Identified uncached requirement: project @ file:///private/tmp/project
DEBUG Acquired lock for `/Users/Jo/.cache/uv/built-wheels-v3/editable/a0e7529ceb99aa48`
DEBUG Building: project @ file:///private/tmp/project
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.12.5
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==74.0.0 [compatible] (setuptools-74.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/df/b5/168cec9a10bf93b60b8f9af7f4e61d526e31e1aad8b9be0e30837746d700/setuptools-74.0.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG Split specific environment resolution took 0.003s
DEBUG Installing in setuptools==74.0.0 in /Users/Jo/.cache/uv/builds-v0/.tmpJgiFHb
DEBUG Requirement already cached: setuptools==74.0.0
DEBUG Installing build requirement: setuptools==74.0.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_editable()`
DEBUG Calling `setuptools.build_meta:__legacy__.build_editable("/Users/Jo/.cache/uv/built-wheels-v3/editable/a0e7529ceb99aa48/cH78NBLJe7WJ1SJlYzd2P/.tmp6a6fqx", {}, None)`
DEBUG Finished building: project @ file:///private/tmp/project
Prepared 1 package in 579ms
DEBUG Uninstalled project (7 files, 1 directory)
Uninstalled 1 package in 0.87ms
Installed 1 package in 1ms
 ~ project==0.1.0 (from file:///private/tmp/project)
```

## Environment

```
uv 0.3.5 (53ef633c6 2024-08-28) - the master tip
macOS 14.6.1 (23G93)
```

---

_Renamed from "Change project to non-project, `uv sync` still installs it" to "Change project to virtual, `uv sync` still installs it" by @j178 on 2024-08-28 13:58_

---

_Comment by @zanieb on 2024-08-28 13:59_

I don't think our changes have released yet! #6585 is in the next release.

---

_Label `question` added by @zanieb on 2024-08-28 13:59_

---

_Comment by @zanieb on 2024-08-28 13:59_

Oh sorry I see "master tip" now.

---

_Comment by @charliermarsh on 2024-08-28 14:00_

I can take a look at this.

---

_Comment by @zanieb on 2024-08-28 14:01_

Presumably because the lockfile isn't treated as stale? (my naive guess haha)

---

_Comment by @charliermarsh on 2024-08-28 14:03_

Probably yeah...

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-28 14:03_

---

_Label `bug` added by @charliermarsh on 2024-08-28 14:03_

---

_Comment by @charliermarsh on 2024-08-28 14:03_

Thank you for testing on `main`.

---

_Referenced in [astral-sh/uv#6754](../../astral-sh/uv/pulls/6754.md) on 2024-08-28 14:57_

---

_Closed by @charliermarsh on 2024-08-28 15:11_

---

_Closed by @charliermarsh on 2024-08-28 15:11_

---

_Referenced in [astral-sh/uv#12311](../../astral-sh/uv/issues/12311.md) on 2025-03-19 11:57_

---

_Referenced in [astral-sh/uv#1419](../../astral-sh/uv/issues/1419.md) on 2025-05-15 21:45_

---
