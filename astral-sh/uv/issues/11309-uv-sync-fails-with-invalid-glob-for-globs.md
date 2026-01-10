---
number: 11309
title: "`uv sync ` fails with \"Invalid glob\" for globs starting with `**/` when run in `/`"
type: issue
state: closed
author: danielgafni
labels:
  - error messages
assignees: []
created_at: 2025-02-07T09:39:32Z
updated_at: 2025-02-07T10:24:07Z
url: https://github.com/astral-sh/uv/issues/11309
synced_at: 2026-01-10T01:25:03Z
---

# `uv sync ` fails with "Invalid glob" for globs starting with `**/` when run in `/`

---

_Issue opened by @danielgafni on 2025-02-07 09:39_

### Summary

I have this glob for workspace sources: `**/*-dagger` which works perfectly well locally, but causes `uv` to fail during docker build. 

```
0.743 warning: Invalid glob in `tool.uv.workspace.members`: `**/*-dagger`
0.747 Using CPython 3.11.11 interpreter at: /usr/bin/python3.11
0.747 Creating virtual environment at: /app/venv
0.754 /app/venv/bin/python
0.788 Python 3.11.11
0.791 uv 0.5.27
1.093 error: Invalid glob in `tool.uv.workspace.members`: `/**/*-dagger`
1.093   Caused by: attempting to read `/dev/fd/9` resulted in an error: No such file or directory (os error 2)
```

I believe `uv sync --no-install-workspace` is failing here. 

The failure occurs when 2 conditions are satisfied together:
- the blob starts with `**` (probably `*` will fail too)
- the WORKDIR is just `/` 

### Platform

x86_64 Ubuntu in Docker

### Version

uv 0.5.27

### Python version

Python 3.11.11

---

_Label `bug` added by @danielgafni on 2025-02-07 09:39_

---

_Comment by @konstin on 2025-02-07 10:15_

If you have a glob starting with `**/` and the current directory is `/`, we have to read the entire filesystem, because every could potentially satisfy it. The first part of the error message is misleading, but the second line shows the correct error.

---

_Label `bug` removed by @konstin on 2025-02-07 10:15_

---

_Label `error messages` added by @konstin on 2025-02-07 10:15_

---

_Referenced in [astral-sh/uv#11310](../../astral-sh/uv/pulls/11310.md) on 2025-02-07 10:16_

---

_Renamed from "`uv syunc ` fails with "Invalid glob" for globs starting with `**/` when run in `/`" to "`uv sync ` fails with "Invalid glob" for globs starting with `**/` when run in `/`" by @danielgafni on 2025-02-07 10:23_

---

_Closed by @konstin on 2025-02-07 10:24_

---
