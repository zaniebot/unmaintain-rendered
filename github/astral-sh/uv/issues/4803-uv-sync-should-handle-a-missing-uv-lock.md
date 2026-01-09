---
number: 4803
title: "`uv sync` should handle a missing `uv lock`."
type: issue
state: closed
author: konstin
labels:
  - error messages
  - preview
assignees: []
created_at: 2024-07-04T12:50:17Z
updated_at: 2024-07-09T22:18:32Z
url: https://github.com/astral-sh/uv/issues/4803
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv sync` should handle a missing `uv lock`.

---

_Issue opened by @konstin on 2024-07-04 12:50_

When trying to sync an empty workspace i got:

```
$ uv sync
warning: `uv sync` is experimental and may change without warning.
error: failed to read from file `/home/konsti/projects/workspace-git-path-dep-test/uv.lock`
  Caused by: No such file or directory (os error 2)
```

This error message should hint towards running `uv lock`.

---

_Label `error messages` added by @konstin on 2024-07-04 12:50_

---

_Comment by @zanieb on 2024-07-04 15:28_

Or, as discussed on Discord, we should just create the lock file without a `--locked` option.

---

_Renamed from "`uv sync` should hint on missing `uv lock`." to "`uv sync` should handle a missing `uv lock`." by @zanieb on 2024-07-04 15:28_

---

_Comment by @charliermarsh on 2024-07-04 17:41_

I created a separate issue, since it still makes sense to have a better error here (e.g., if `--locked` is provided): https://github.com/astral-sh/uv/issues/4812

---

_Label `preview` added by @charliermarsh on 2024-07-04 17:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-05 21:43_

---

_Referenced in [astral-sh/uv#4839](../../astral-sh/uv/pulls/4839.md) on 2024-07-05 21:43_

---

_Closed by @charliermarsh on 2024-07-09 22:18_

---

_Closed by @charliermarsh on 2024-07-09 22:18_

---
