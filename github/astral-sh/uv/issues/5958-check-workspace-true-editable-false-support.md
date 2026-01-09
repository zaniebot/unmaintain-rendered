---
number: 5958
title: "Check `{ workspace = true, editable = false }` support"
type: issue
state: closed
author: konstin
labels:
  - preview
assignees: []
created_at: 2024-08-09T14:20:39Z
updated_at: 2024-08-10T00:59:24Z
url: https://github.com/astral-sh/uv/issues/5958
synced_at: 2026-01-07T13:12:17-06:00
---

# Check `{ workspace = true, editable = false }` support

---

_Issue opened by @konstin on 2024-08-09 14:20_

We need to check the support of non-editable workspace dependencies in `pyproject.toml`. If it doesn't work remove it from the docs.

See also #5792.

---

_Label `preview` added by @konstin on 2024-08-09 14:20_

---

_Assigned to @charliermarsh by @konstin on 2024-08-09 14:21_

---

_Comment by @charliermarsh on 2024-08-09 22:01_

I've confirmed that this isn't supported. The issue is that we have to lock the entire workspace. So we add all the members (as editable), then lock, then install the appropriate subset.

In order to support this, I think we'd need to make editable part of `dependencies`, rather than its own source... Since we'd need the workspace member to be a `directory` that can either be installed as editable or non-editable.


---

_Comment by @charliermarsh on 2024-08-09 22:01_

Do we want to support or remove? \cc @zanieb 

---

_Comment by @zanieb on 2024-08-09 22:07_

I guess we should remove it for now.

---

_Comment by @charliermarsh on 2024-08-09 22:55_

I think I should make a timeboxed effort to fix it.

---

_Referenced in [astral-sh/uv#5985](../../astral-sh/uv/pulls/5985.md) on 2024-08-09 23:33_

---

_Referenced in [astral-sh/uv#5987](../../astral-sh/uv/pulls/5987.md) on 2024-08-10 00:27_

---

_Closed by @charliermarsh on 2024-08-10 00:59_

---

_Closed by @charliermarsh on 2024-08-10 00:59_

---
