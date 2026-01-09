---
number: 5952
title: "feature request: support `uv pip freeze --path <path>`"
type: issue
state: closed
author: charles-cooper
labels:
  - compatibility
  - cli
assignees: []
created_at: 2024-08-09T08:15:03Z
updated_at: 2025-01-13T22:50:06Z
url: https://github.com/astral-sh/uv/issues/5952
synced_at: 2026-01-07T13:12:17-06:00
---

# feature request: support `uv pip freeze --path <path>`

---

_Issue opened by @charles-cooper on 2024-08-09 08:15_

uv 0.2.34

add a `--path` option to `uv pip freeze` similar to `pip freeze`:
```
  --path <path>               Restrict to the specified installation path for listing packages (can be used multiple
                              times).
```
in conjunction with https://github.com/astral-sh/uv/issues/1517, after installing packages to a custom location, you may want to freeze the packages installed at that location as well.

---

_Label `compatibility` added by @zanieb on 2024-08-09 13:29_

---

_Label `cli` added by @zanieb on 2024-08-09 13:29_

---

_Comment by @ericmarkmartin on 2025-01-11 00:31_

@zanieb I can take this if it's good w/ you

---

_Referenced in [astral-sh/uv#10488](../../astral-sh/uv/pulls/10488.md) on 2025-01-11 01:45_

---

_Assigned to @ericmarkmartin by @charliermarsh on 2025-01-11 13:55_

---

_Closed by @charliermarsh on 2025-01-13 22:50_

---

_Closed by @charliermarsh on 2025-01-13 22:50_

---
