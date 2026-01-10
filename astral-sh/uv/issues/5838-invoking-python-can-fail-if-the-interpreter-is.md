```yaml
number: 5838
title: "Invoking `python` can fail if the interpreter is named `python3`"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-06T22:22:26Z
updated_at: 2024-08-07T03:03:23Z
url: https://github.com/astral-sh/uv/issues/5838
synced_at: 2026-01-10T04:53:49Z
```

# Invoking `python` can fail if the interpreter is named `python3`

---

_Issue opened by @zanieb on 2024-08-06 22:22_

e.g.

```
❯ uv run -v python -c "import anyio"
DEBUG uv 0.2.32
warning: `uv run` is experimental and may change without warning
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/Users/zb/Library/Application Support/uv/python`
DEBUG Found managed installation `cpython-3.12.1-macos-aarch64-none`
DEBUG Found `cpython-3.12.1-macos-aarch64-none` at `/Users/zb/Library/Application Support/uv/python/cpython-3.12.1-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Using Python 3.12.1 interpreter at: /Users/zb/Library/Application Support/uv/python/cpython-3.12.1-macos-aarch64-none/bin/python3
DEBUG Running `python -c import anyio`
error: Failed to spawn: `python`
  Caused by: No such file or directory (os error 2)
```

We should detect and special case `python`, I think.

---

_Comment by @charliermarsh on 2024-08-06 22:23_

Huh. I thought there was always a `python` alias in there.

---

_Comment by @zanieb on 2024-08-06 22:41_

Should we add one to our installs?

```
❯ ls '/Users/zb/Library/Application Support/uv/python/cpython-3.12.1-macos-aarch64-none/bin/'
2to3			idle3			pip			pip3.12			pydoc3.12		python3-config		python3.12-config
2to3-3.12		idle3.12		pip3			pydoc3			python3			python3.12
```

---

_Comment by @charliermarsh on 2024-08-06 23:02_

I guess this would succeed if we always ran in a virtual environment...

---

_Comment by @charliermarsh on 2024-08-06 23:22_

We should probably create a symlink there though. It's interesting that we have `pip` and `pip3` but not `python`.

---

_Comment by @charliermarsh on 2024-08-06 23:23_

Doing some research, it looks like this was. added to `python-build-standalone` in https://github.com/indygreg/python-build-standalone/commit/e9427bd059465e6472ea6ac80e6dd849e53f3e64, and [PEP 394](https://peps.python.org/pep-0394/) says it's allowed. So wondering why it didn't appear here. I'll debug.

---

_Comment by @charliermarsh on 2024-08-06 23:33_

I think the symlink is being filtered out when unzipping.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-06 23:33_

---

_Label `bug` added by @charliermarsh on 2024-08-07 00:47_

---

_Comment by @zanieb on 2024-08-07 01:36_

Regardless of its presence in the python-build-standalone installations, should we special case `python` in `uv run`?

---

_Comment by @charliermarsh on 2024-08-07 01:38_

I don’t think so personally. 

---

_Comment by @zanieb on 2024-08-07 01:51_

Alright, we can see if it comes up.

---

_Comment by @charliermarsh on 2024-08-07 01:51_

(Do you mean: if they use `python`, we inject the full path to the interpreter?)

---

_Comment by @charliermarsh on 2024-08-07 02:22_

On my machine at least, these _do_ have a `python`:

- cpython-3.8.18-macos-aarch64-none
- cpython-3.9.18-macos-aarch64-none
- cpython-3.10.13-macos-aarch64-none

While these do not:

- cpython-3.8.12-macos-aarch64-none
- cpython-3.11.7-macos-aarch64-none
- cpython-3.12.1-macos-aarch64-none


---

_Comment by @charliermarsh on 2024-08-07 02:24_

Ah ok, it looks like those distributions actually don't contain a `python`. So it's an inconsistency in `python-build-standalone`.

---

_Comment by @charliermarsh on 2024-08-07 02:26_

Ooooh those versions pre-date the introduction of that feature.

---

_Comment by @charliermarsh on 2024-08-07 02:28_

I'm going to change our installer to add it

---

_Label `preview` added by @charliermarsh on 2024-08-07 02:50_

---

_Closed by @charliermarsh on 2024-08-07 03:03_

---

_Closed by @charliermarsh on 2024-08-07 03:03_

---
