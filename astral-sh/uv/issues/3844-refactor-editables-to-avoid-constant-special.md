---
number: 3844
title: Refactor editables to avoid constant special-casing
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2024-05-26T23:25:57Z
updated_at: 2024-05-28T15:49:35Z
url: https://github.com/astral-sh/uv/issues/3844
synced_at: 2026-01-10T01:23:31Z
---

# Refactor editables to avoid constant special-casing

---

_Issue opened by @charliermarsh on 2024-05-26 23:25_

Editables are special-cased everywhere. We should try and remove this, and make editables just another distribution that we can resolve and install.

---

_Label `internal` added by @charliermarsh on 2024-05-26 23:26_

---

_Comment by @charliermarsh on 2024-05-26 23:26_

I would love to take a day to do this at some point.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-27 17:10_

---

_Referenced in [astral-sh/uv#3869](../../astral-sh/uv/pulls/3869.md) on 2024-05-27 21:08_

---

_Closed by @charliermarsh on 2024-05-28 15:49_

---
