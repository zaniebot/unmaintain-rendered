---
number: 99
title: Investigate the impact on a custom metadata index
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-16T02:23:41Z
updated_at: 2024-04-08T02:18:09Z
url: https://github.com/astral-sh/uv/issues/99
synced_at: 2026-01-07T13:12:16-06:00
---

# Investigate the impact on a custom metadata index

---

_Issue opened by @charliermarsh on 2023-10-16 02:23_

Could we provide our own endpoint that's closer to the Crates sparse index? That is: a single endpoint for each package, which includes all the dependencies for all versions, thereby limiting the number of fetches to the depth of the tree. (Right now, the resolver needs to perform a fetch for every package-version pair.)

---

_Comment by @konstin on 2023-10-16 08:56_

Also an interesting query: Here's 200 packages and the date i last updated them, which of these got updated since? That way we could do `puffin update` in potentially one query.

---

_Label `performance` added by @charliermarsh on 2023-10-24 01:18_

---

_Added to milestone `Future` by @charliermarsh on 2023-10-25 04:32_

---

_Comment by @charliermarsh on 2024-04-08 02:18_

Too high-level to be actionable, will revisit.

---

_Closed by @charliermarsh on 2024-04-08 02:18_

---
