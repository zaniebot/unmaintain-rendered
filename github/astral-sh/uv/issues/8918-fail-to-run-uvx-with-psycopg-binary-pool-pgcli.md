---
number: 8918
title: "fail to run  `uvx --with=\"psycopg[binary,pool]\" pgcli --help` after upgrade to 0.5.0"
type: issue
state: closed
author: zuisong
labels:
  - bug
assignees: []
created_at: 2024-11-08T05:22:35Z
updated_at: 2024-11-08T20:13:32Z
url: https://github.com/astral-sh/uv/issues/8918
synced_at: 2026-01-07T13:12:18-06:00
---

# fail to run  `uvx --with="psycopg[binary,pool]" pgcli --help` after upgrade to 0.5.0

---

_Issue opened by @zuisong on 2024-11-08 05:22_

```
❯ uvx --with="psycopg[binary,pool]" pgcli --help
error: Failed to parse: `psycopg[binary`
  Caused by: Missing closing bracket (expected ']', found end of dependency specification)
psycopg[binary
❯ uv --version 
uv 0.5.0 (Homebrew 2024-11-07)
```

it works at 0.4.x

---

_Comment by @charliermarsh on 2024-11-08 13:39_

Looks like a bug although I don't think it's new in v0.5. It works for me in v0.4.20, but not in any of the ten releases between then and v0.5.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-08 14:34_

---

_Label `bug` added by @charliermarsh on 2024-11-08 14:34_

---

_Comment by @charliermarsh on 2024-11-08 14:43_

Regressed in #7909 where we started to support `--with "flask, anyio"`.

---

_Comment by @zanieb on 2024-11-08 15:16_

Oh that's a disappointing side-effect.

---

_Referenced in [astral-sh/uv#8946](../../astral-sh/uv/pulls/8946.md) on 2024-11-08 15:23_

---

_Closed by @charliermarsh on 2024-11-08 20:13_

---

_Closed by @charliermarsh on 2024-11-08 20:13_

---
