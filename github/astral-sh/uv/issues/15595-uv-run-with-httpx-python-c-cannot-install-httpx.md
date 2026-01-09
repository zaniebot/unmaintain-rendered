---
number: 15595
title: "uv run --with httpx python -c \"\" cannot install httpx in isolated env if already installed"
type: issue
state: closed
author: copdips
labels:
  - bug
assignees: []
created_at: 2025-08-30T15:35:35Z
updated_at: 2025-08-30T17:50:13Z
url: https://github.com/astral-sh/uv/issues/15595
synced_at: 2026-01-07T13:12:19-06:00
---

# uv run --with httpx python -c "" cannot install httpx in isolated env if already installed

---

_Issue opened by @copdips on 2025-08-30 15:35_

### Summary

From the [official example](https://docs.astral.sh/uv/concepts/projects/run/#requesting-additional-dependencies), we can see that:

```bash
uv run --with httpx==0.26.0 python -c "import httpx; print(httpx.__version__)"
0.26.0

uv run --with httpx==0.25.0 python -c "import httpx; print(httpx.__version__)"
0.25.0
```

This's true if `httpx` is not installed in the current venv, but if it's already install, the above commands, both return the one in the current venv, not the one in an isolated env.

Repro:

```bash
$ uv pip install httpx
Resolved 7 packages in 5ms
Installed 1 package in 5ms
 + httpx==0.28.1

# I want to use the version 0.26.0, but 0.28.1 is used

$ uv run --with httpx==0.26.0 python -c "import httpx; print(httpx.__version__)"
0.28.1
```



### Platform

ubuntu24.04 in WSL

### Version

0.8.0

### Python version

3.13

---

_Label `bug` added by @copdips on 2025-08-30 15:35_

---

_Renamed from "uv run --with httpx python -c "" cannot install httpx in isolated env if already install" to "uv run --with httpx python -c "" cannot install httpx in isolated env if already installed" by @copdips on 2025-08-30 15:59_

---

_Comment by @zanieb on 2025-08-30 16:29_

Does this reproduce with the latest version?

---

_Comment by @copdips on 2025-08-30 17:50_

You're right, the lastest version is OK, I'll close this issue.

---

_Closed by @copdips on 2025-08-30 17:50_

---
