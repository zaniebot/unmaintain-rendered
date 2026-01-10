---
number: 7096
title: Prune unused source distributions from the cache
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - help wanted
  - cache
assignees: []
created_at: 2024-09-05T19:18:51Z
updated_at: 2024-09-06T01:40:52Z
url: https://github.com/astral-sh/uv/issues/7096
synced_at: 2026-01-10T01:24:10Z
---

# Prune unused source distributions from the cache

---

_Issue opened by @charliermarsh on 2024-09-05 19:18_

If you look at the structure of the cache, we end up having multiple revisions here (the two directories in the rightmost pane):

![Screenshot 2024-09-05 at 3 18 10 PM](https://github.com/user-attachments/assets/204636ce-cf70-445a-a9a6-09633be00e45)

But only one of them is actually referenced in `revision.rev`.

In `cache prune`, we should identify unused revisions and remove them.


---

_Label `enhancement` added by @charliermarsh on 2024-09-05 19:18_

---

_Label `help wanted` added by @charliermarsh on 2024-09-05 19:18_

---

_Label `cache` added by @charliermarsh on 2024-09-05 19:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-06 01:26_

---

_Referenced in [astral-sh/uv#7112](../../astral-sh/uv/pulls/7112.md) on 2024-09-06 01:27_

---

_Closed by @charliermarsh on 2024-09-06 01:40_

---

_Closed by @charliermarsh on 2024-09-06 01:40_

---
