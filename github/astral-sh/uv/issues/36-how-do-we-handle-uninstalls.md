---
number: 36
title: How do we handle uninstalls?
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-10-07T01:58:27Z
updated_at: 2023-10-09T18:14:34Z
url: https://github.com/astral-sh/uv/issues/36
synced_at: 2026-01-07T13:12:16-06:00
---

# How do we handle uninstalls?

---

_Issue opened by @charliermarsh on 2023-10-07 01:58_

_No description provided._

---

_Comment by @charliermarsh on 2023-10-09 02:19_

Actually looks fairly straightforward. You just iterate over the entries in the `RECORD` (and delete `.pyc` files where applicable): https://peps.python.org/pep-0427/#installing-a-wheel-distribution-1-0-py32-none-any-whl.

---

_Comment by @konstin on 2023-10-09 12:05_

`.pyc` files should also be part of the RECORD

---

_Referenced in [astral-sh/uv#77](../../astral-sh/uv/pulls/77.md) on 2023-10-09 18:06_

---

_Closed by @charliermarsh on 2023-10-09 18:14_

---
