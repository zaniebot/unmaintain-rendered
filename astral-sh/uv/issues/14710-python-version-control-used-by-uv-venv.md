---
number: 14710
title: Python version control used by uv venv
type: issue
state: closed
author: cwiede
labels:
  - question
assignees: []
created_at: 2025-07-18T10:00:19Z
updated_at: 2025-07-18T10:57:37Z
url: https://github.com/astral-sh/uv/issues/14710
synced_at: 2026-01-10T01:25:47Z
---

# Python version control used by uv venv

---

_Issue opened by @cwiede on 2025-07-18 10:00_

### Question

When I use `uv venv -p 3.12 --managed-python venvdir` the first time on a new machine, then it automatically downloads the latest patch version of python 3.12 and creates a new venv on it. When I repeat that command at a later time, it will always use the python version already downloaded, regardless whether a new patch version is available or not. I found that behaviour a little bit surprising.

I'm wondering, what would be the correct way of making sure to always use the latest available patch version? Do I manually have to call `uv python install --managed-python 3.12 -r` before the uv venv command?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @cwiede on 2025-07-18 10:00_

---

_Comment by @konstin on 2025-07-18 10:07_

You can use the - currently experimental upgrade command for this:

```
uv python upgrade
```

If you set `UV_PREVIEW=1` when installing Python and creating venvs, `uv python upgrade` will transparently upgrade all venvs created with `-p 3.12` to the latest version. We plan to stabilize this behavior and make it the default in a future version.

---

_Closed by @cwiede on 2025-07-18 10:57_

---
