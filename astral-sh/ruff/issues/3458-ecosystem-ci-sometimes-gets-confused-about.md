---
number: 3458
title: Ecosystem CI sometimes gets confused about upstream
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-03-12T05:23:31Z
updated_at: 2023-03-14T02:37:13Z
url: https://github.com/astral-sh/ruff/issues/3458
synced_at: 2026-01-10T01:22:42Z
---

# Ecosystem CI sometimes gets confused about upstream

---

_Issue opened by @charliermarsh on 2023-03-12 05:23_

E.g., in https://github.com/charliermarsh/ruff/pull/3456 -- that PR didn't affect any _actual_ change, but the ecosystem check triggered on it, and the output seemed to compare against another open PR (https://github.com/charliermarsh/ruff/pull/3455).

![Screen Shot 2023-03-12 at 12 22 32 AM](https://user-images.githubusercontent.com/1309177/224526035-4ffa6805-5543-49af-9978-c7da61c55b6b.png)


---

_Label `bug` added by @charliermarsh on 2023-03-12 05:23_

---

_Referenced in [astral-sh/ruff#3467](../../astral-sh/ruff/pulls/3467.md) on 2023-03-12 19:17_

---

_Referenced in [astral-sh/ruff#3499](../../astral-sh/ruff/pulls/3499.md) on 2023-03-14 01:28_

---

_Closed by @charliermarsh on 2023-03-14 02:37_

---
