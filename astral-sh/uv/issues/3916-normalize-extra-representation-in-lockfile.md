---
number: 3916
title: Normalize extra representation in lockfile
type: issue
state: closed
author: charliermarsh
labels:
  - preview
assignees: []
created_at: 2024-05-29T19:41:35Z
updated_at: 2024-06-03T19:00:36Z
url: https://github.com/astral-sh/uv/issues/3916
synced_at: 2026-01-10T01:23:32Z
---

# Normalize extra representation in lockfile

---

_Issue opened by @charliermarsh on 2024-05-29 19:41_

We should include a single `Distribution` with an `optional-dependencies` entry for each extra, rather than representing each extra as its own distribution and dependency.

---

_Label `preview` added by @charliermarsh on 2024-05-29 19:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-01 17:39_

---

_Referenced in [astral-sh/uv#3958](../../astral-sh/uv/pulls/3958.md) on 2024-06-01 18:13_

---

_Closed by @charliermarsh on 2024-06-03 19:00_

---
