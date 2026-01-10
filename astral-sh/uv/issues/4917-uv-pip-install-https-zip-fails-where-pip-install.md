---
number: 4917
title: "`uv pip install https://...zip` fails where `pip install` works"
type: issue
state: closed
author: bersbersbers
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-07-09T07:17:30Z
updated_at: 2024-07-09T17:30:44Z
url: https://github.com/astral-sh/uv/issues/4917
synced_at: 2026-01-10T01:23:43Z
---

# `uv pip install https://...zip` fails where `pip install` works

---

_Issue opened by @bersbersbers on 2024-07-09 07:17_

```
$ uv pip install https://github.com/python/mypy/archive/refs/heads/release-1.11.zip
error: Failed to download and build: `release @ https://github.com/python/mypy/archive/refs/heads/release-1.11.zip`
  Caused by: Package metadata name `mypy` does not match given name `release`

$ pip install https://github.com/python/mypy/archive/refs/heads/release-1.11.zip
Collecting https://github.com/python/mypy/archive/refs/heads/release-1.11.zip
...
Successfully installed mypy-1.11.0+dev
```

uv 0.2.23 (4bc36c0cb 2024-07-08) on Python 3.12.4 on Windows 11 23H2

---

_Label `compatibility` added by @konstin on 2024-07-09 09:06_

---

_Label `bug` added by @charliermarsh on 2024-07-09 16:23_

---

_Comment by @charliermarsh on 2024-07-09 16:23_

Thanks, that's a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-09 16:28_

---

_Referenced in [astral-sh/uv#4928](../../astral-sh/uv/pulls/4928.md) on 2024-07-09 17:22_

---

_Closed by @charliermarsh on 2024-07-09 17:30_

---
