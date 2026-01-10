```yaml
number: 11530
title: uv sync should warn (or fail) when lock file is out of sync with pyproject.toml dependency tables
type: issue
state: closed
author: gaborbernat
labels:
  - question
assignees: []
created_at: 2025-02-15T02:35:00Z
updated_at: 2025-02-15T15:40:00Z
url: https://github.com/astral-sh/uv/issues/11530
synced_at: 2026-01-10T03:50:31Z
```

# uv sync should warn (or fail) when lock file is out of sync with pyproject.toml dependency tables

---

_Issue opened by @gaborbernat on 2025-02-15 02:35_

### Summary

If uv detects that the:

`dependencies`, `dependency-groups`, `optional-dependencies` have been or `tool.uv.environments` changed since the lock file has been generated it should warn the user.

I was sitting here for the last 10 minutes wondering I've updated my project dependency, why is uv sync not picking it up 😂 definitely a skill issue though. So just a small quality of life improvement.



### Example

```
uv sync
warning: the lock file is out of date with the pyproject.toml file
```

poetry does error out when a hash of important parts of the pyproject.toml differs from the one saved in the lockfile, for reference.

---

_Label `enhancement` added by @gaborbernat on 2025-02-15 02:35_

---

_Comment by @zanieb on 2025-02-15 04:13_

Huh? We do update the lockfile by default. Do you have `UV_FROZEN` set or something?

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ vi pyproject.toml
❯ uv sync
Using CPython 3.13.0
Creating virtual environment at: .venv
Resolved 4 packages in 722ms
Prepared 3 packages in 328ms
Installed 3 packages in 4ms
 + anyio==4.8.0
 + idna==3.10
 + sniffio==1.3.1
❯ vi pyproject.toml
❯ cat pyproject.toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = ["anyio<4.8.0"]
❯ uv sync --frozen
Audited 3 packages in 0.20ms
❯ uv sync --locked
Resolved 4 packages in 240ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
❯ uv sync
Resolved 4 packages in 1ms
Prepared 1 package in 303ms
Uninstalled 1 package in 4ms
Installed 1 package in 5ms
 - anyio==4.8.0
 + anyio==4.7.0
```

---

_Label `enhancement` removed by @zanieb on 2025-02-15 04:13_

---

_Label `question` added by @zanieb on 2025-02-15 04:13_

---

_Comment by @gaborbernat on 2025-02-15 04:52_

Actually I do pass in the `--frozen` flag 🤔 https://github.com/tox-dev/tox-uv/blob/main/src/tox_uv/_run_lock.py#L81

---

_Comment by @gaborbernat on 2025-02-15 04:54_

So guess the question should I pass in `--locked` instead, or both `--frozen` and `--locked`.

---

_Comment by @gaborbernat on 2025-02-15 05:09_

> So guess the question should I pass in `--locked` instead

Closing with this conclusion. If my conclusion is incorrect please ping me.

---

_Closed by @gaborbernat on 2025-02-15 05:09_

---

_Comment by @zanieb on 2025-02-15 15:39_

Yeah `--frozen` specifically opts-in to ignoring changes to the `pyproject.toml`.

---
