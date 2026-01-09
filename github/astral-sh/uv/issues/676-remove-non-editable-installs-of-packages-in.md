---
number: 676
title: Remove non-editable installs of packages in install plan
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-17T19:51:50Z
updated_at: 2023-12-18T09:28:16Z
url: https://github.com/astral-sh/uv/issues/676
synced_at: 2026-01-07T13:12:16-06:00
---

# Remove non-editable installs of packages in install plan

---

_Issue opened by @charliermarsh on 2023-12-17 19:51_

Right now, if you install `black` from a registry, then request an editable install of `black`, we do the editable install without uninstalling the existing version...

---

_Label `bug` added by @charliermarsh on 2023-12-17 19:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-17 19:51_

---

_Comment by @charliermarsh on 2023-12-17 19:54_

I know how to fix this, though for `pip-sync` we'll need to build the editables before we make the install plan.

---

_Comment by @charliermarsh on 2023-12-17 20:55_

I think we can come up with an abstraction that will let us build only the editables we _need_ prior to creating the install plan.

---

_Referenced in [astral-sh/uv#677](../../astral-sh/uv/pulls/677.md) on 2023-12-18 02:14_

---

_Referenced in [astral-sh/uv#682](../../astral-sh/uv/pulls/682.md) on 2023-12-18 09:23_

---

_Closed by @konstin on 2023-12-18 09:28_

---
