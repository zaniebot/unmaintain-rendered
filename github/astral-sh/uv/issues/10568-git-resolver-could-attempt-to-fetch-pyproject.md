---
number: 10568
title: "Git resolver could attempt to fetch `pyproject.toml` to resolve package metadata"
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2025-01-13T15:50:38Z
updated_at: 2025-01-20T17:50:41Z
url: https://github.com/astral-sh/uv/issues/10568
synced_at: 2026-01-07T13:12:18-06:00
---

# Git resolver could attempt to fetch `pyproject.toml` to resolve package metadata

---

_Issue opened by @charliermarsh on 2025-01-13 15:50_

If a dependency is from GitHub (probably GitLab too), we could add a fast-path during resolution to fetch _just_ the `pyproject.toml`, see if it's static, and read the metadata directly if so.


---

_Label `performance` added by @charliermarsh on 2025-01-13 15:50_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-18 03:30_

---

_Referenced in [astral-sh/uv#10765](../../astral-sh/uv/pulls/10765.md) on 2025-01-20 01:26_

---

_Closed by @charliermarsh on 2025-01-20 17:50_

---

_Closed by @charliermarsh on 2025-01-20 17:50_

---
