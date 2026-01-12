```yaml
number: 9059
title: "`uv tree` should support `--show-version-specifiers` like `uv pip tree`"
type: issue
state: open
author: zanieb
labels:
  - wish
  - cli
assignees: []
created_at: 2024-11-12T14:37:35Z
updated_at: 2025-12-19T10:03:19Z
url: https://github.com/astral-sh/uv/issues/9059
synced_at: 2026-01-12T15:59:41Z
```

# `uv tree` should support `--show-version-specifiers` like `uv pip tree`

---

_@zanieb_

I was surprised to find this is not supported.

---

_Label `cli` added by @zanieb on 2024-11-12 14:37_

---

_Comment by @charliermarsh on 2024-11-12 14:40_

It's not possible to support right now. We don't store this information in the lockfile.

---

_Label `wish` added by @charliermarsh on 2024-11-12 17:31_

---

_Comment by @charliermarsh on 2024-11-12 17:32_

It might make sense to start storing this in the lockfile -- it'd be useful for showing the "most recent compatible version" too. But it would also make the locks much larger.

---

_Comment by @zanieb on 2024-11-12 17:52_

Hm. Yeah it seems important to be able to show this for debugging things.

We could at least show them for direct dependencies and read from the `pyproject.toml`

---

_Comment by @charliermarsh on 2024-11-12 17:54_

We could show them for a subset of the tree, yeah.

---

_Comment by @charliermarsh on 2024-11-12 17:54_

I think it will be a non-trivial increase to the lockfile though, maybe like 1.5x the size?

---

_Comment by @zanieb on 2024-11-12 18:16_

How bad would it be to resolve from the lockfile and create an in-memory lock with the additional information for cases like this?

---

_Comment by @charliermarsh on 2024-11-12 19:05_

I think I'd rather just add them to the lock honestly. They're useful for debugging and they'll almost certainly be required by the PEP.

---

_Comment by @slafs on 2025-12-19 10:03_

Hello! Just wanted to ask if this issue is on the radar perhaps?

---
