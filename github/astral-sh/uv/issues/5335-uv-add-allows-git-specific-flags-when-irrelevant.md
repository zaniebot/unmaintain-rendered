---
number: 5335
title: "`uv add` allows git-specific flags when irrelevant"
type: issue
state: closed
author: zanieb
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-23T14:52:23Z
updated_at: 2024-07-23T21:48:51Z
url: https://github.com/astral-sh/uv/issues/5335
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv add` allows git-specific flags when irrelevant

---

_Issue opened by @zanieb on 2024-07-23 14:52_

e.g.

```
❯ uv add \
    "https://files.pythonhosted.org/packages/5c/2d/3da5bdf4408b8b2800061c339f240c1802f2e82d55e50bd39c5a881f47f0/httpx-0.27.0.tar.gz" \
    --rev 0.2.0
```

should at least warn and probably error, but does not. The `rev` is not written to the `pyproject.toml`.

---

_Label `bug` added by @zanieb on 2024-07-23 14:52_

---

_Label `preview` added by @zanieb on 2024-07-23 14:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-23 21:04_

---

_Referenced in [astral-sh/uv#5377](../../astral-sh/uv/pulls/5377.md) on 2024-07-23 21:36_

---

_Closed by @charliermarsh on 2024-07-23 21:48_

---
