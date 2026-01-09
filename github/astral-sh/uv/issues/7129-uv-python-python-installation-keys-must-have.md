---
number: 7129
title: "`uv python`: Python installation keys must have valid Python versions"
type: issue
state: closed
author: pythonweb2
labels:
  - bug
assignees: []
created_at: 2024-09-06T16:58:35Z
updated_at: 2024-09-06T23:23:17Z
url: https://github.com/astral-sh/uv/issues/7129
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv python`: Python installation keys must have valid Python versions

---

_Issue opened by @pythonweb2 on 2024-09-06 16:58_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
In my environment, some old code still uses python 3.6, so I have it installed. When I run `uv python list`, this happens:

```
>>> uv python list
thread 'main' panicked at crates\uv-python\src\installation.rs:247:14:
Python installation keys must have valid Python versions: "Python version `3.6.8` must be >= 3.7"
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

This feels like a bug, since I would expect it to just show me the supported versions, instead of the whole command being broken if an old python version is installed.

---

_Comment by @zanieb on 2024-09-06 17:04_

Eek! Sorry about that.

---

_Label `bug` added by @zanieb on 2024-09-06 17:04_

---

_Referenced in [astral-sh/uv#7131](../../astral-sh/uv/pulls/7131.md) on 2024-09-06 17:19_

---

_Assigned to @zanieb by @charliermarsh on 2024-09-06 23:17_

---

_Closed by @charliermarsh on 2024-09-06 23:23_

---
