```yaml
number: 4547
title: "`uv run --with=X` shouldn't install `X` if it's already available"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-06-26T13:22:23Z
updated_at: 2024-07-08T01:24:00Z
url: https://github.com/astral-sh/uv/issues/4547
synced_at: 2026-01-12T15:58:50Z
```

# `uv run --with=X` shouldn't install `X` if it's already available

---

_@charliermarsh_

If it's already part of the project (and thus the environment), we shouldn't need to install it. Same goes for any of the dependencies of `X`.

---

_Label `preview` added by @charliermarsh on 2024-06-26 13:22_

---

_Comment by @charliermarsh on 2024-06-26 13:24_

This requires that we have some concept of "packages that are available, but not in our current environment". It causes some problems for the install plan. In some ways it's conceptually similar to respecting multiple `sys.path` entries.


---

_Comment by @charliermarsh on 2024-06-26 13:36_

I can be responsible for this though unclear on its priority.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-26 13:55_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-06-26 17:34_

---

_Label `enhancement` added by @charliermarsh on 2024-07-06 19:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-07 19:38_

---

_Closed by @charliermarsh on 2024-07-08 01:24_

---
