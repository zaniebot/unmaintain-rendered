---
number: 6599
title: "`--exact` has no effect when used with `--no-build-isolation` in `uv sync`"
type: issue
state: closed
author: kabouzeid
labels:
  - bug
assignees: []
created_at: 2024-08-25T09:51:59Z
updated_at: 2024-08-26T18:05:00Z
url: https://github.com/astral-sh/uv/issues/6599
synced_at: 2026-01-10T01:24:03Z
---

# `--exact` has no effect when used with `--no-build-isolation` in `uv sync`

---

_Issue opened by @kabouzeid on 2024-08-25 09:51_

`uv sync --no-build-isolation` is always inexact. Adding `--exact` has no effect.

Related: https://github.com/astral-sh/uv/issues/6580

```
➜ uv --version
uv 0.3.3 (Homebrew 2024-08-23)

➜ uv pip install ruff
Resolved 1 package in 81ms
Installed 1 package in 2ms
 + ruff==0.6.2

➜ uv sync --no-build-isolation
Resolved 1 package in 1ms
Audited 1 package in 0.07ms

➜ uv sync --no-build-isolation --exact
Resolved 1 package in 0.53ms
Audited 1 package in 0.04ms

➜ uv sync
Resolved 1 package in 0.50ms
Uninstalled 1 package in 0.67ms
 - ruff==0.6.2
```

---

_Referenced in [astral-sh/uv#6580](../../astral-sh/uv/issues/6580.md) on 2024-08-25 09:56_

---

_Comment by @charliermarsh on 2024-08-25 11:35_

Thanks, this seems like a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-25 11:35_

---

_Label `bug` added by @charliermarsh on 2024-08-25 11:37_

---

_Referenced in [astral-sh/uv#6606](../../astral-sh/uv/pulls/6606.md) on 2024-08-25 14:16_

---

_Closed by @charliermarsh on 2024-08-26 18:05_

---

_Closed by @charliermarsh on 2024-08-26 18:05_

---
