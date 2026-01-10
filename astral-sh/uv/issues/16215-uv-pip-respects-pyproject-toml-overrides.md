---
number: 16215
title: UV pip respects pyproject toml overrides
type: issue
state: closed
author: haarisr
labels:
  - question
assignees: []
created_at: 2025-10-09T19:09:33Z
updated_at: 2025-10-29T02:49:06Z
url: https://github.com/astral-sh/uv/issues/16215
synced_at: 2026-01-10T01:26:04Z
---

# UV pip respects pyproject toml overrides

---

_Issue opened by @haarisr on 2025-10-09 19:09_

### Question

Is uv pip interface supposed to respect the pyproject toml override?

I have a dependency override in my project. I wanted to try a newer version of it, so I did uv pip install dep and it seems to install the override version and not the version that is specified.

```bash
uv init bar
cd bar
echo "[tool.uv]\noverride-dependencies = ['numpy==1.26.4']" >> pyproject.toml
uv sync
uv pip install numpy==2.2.0
```

Output
```bash
Using CPython 3.13.7
Creating virtual environment at: .venv
Resolved 1 package in 3ms
Audited in 0.00ms
Resolved 1 package in 6ms
Installed 1 package in 15ms
 + numpy==1.26.4
```

### Platform

_No response_

### Version

uv 0.8.15

---

_Label `question` added by @haarisr on 2025-10-09 19:09_

---

_Comment by @konstin on 2025-10-14 12:15_

This value is only used when resolving `[project]` dependencies, but `uv pip install numpy==2.2.0` doesn't look at `[project]` and doesn't consider those overrides.

---

_Comment by @haarisr on 2025-10-14 15:39_

Sorry, I am not following. 

 Is the above supposed to return 1.26 or 2.2?

---

_Comment by @charliermarsh on 2025-10-28 23:58_

Sorry, I think Konsti is accidentally saying the opposite: if you run that command in a directory containing a `pyproject.toml` with those overrides, it will indeed respect the overrides.

---

_Closed by @haarisr on 2025-10-29 02:49_

---
