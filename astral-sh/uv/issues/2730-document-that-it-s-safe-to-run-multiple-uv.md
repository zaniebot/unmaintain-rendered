```yaml
number: 2730
title: "Document that it's safe to run multiple `uv` instances in a single venv"
type: issue
state: closed
author: charlesnicholson
labels:
  - documentation
assignees: []
created_at: 2024-03-30T11:50:19Z
updated_at: 2024-04-04T04:33:26Z
url: https://github.com/astral-sh/uv/issues/2730
synced_at: 2026-01-12T15:58:40Z
```

# Document that it's safe to run multiple `uv` instances in a single venv

---

_@charlesnicholson_

Historically, pip has had a [hard time](https://github.com/pypa/pip/issues/2361) with multiple instances running concurrently. Pip's cache could get corrupted, the venv could get corrupted, etc.

Is it safe to run multiple instances of `uv pip install ...` against the same venv at the same time? I don't see this documented on the markdown landing page. It would be great if `uv` could be run concurrently, even if the strategy is just "lock the entire venv" and have the other `uv` instance(s) block.

---

_Comment by @charliermarsh on 2024-03-30 11:59_

Yeah we designed uv with this in mind. There are two components to it: (1) we do lock the venv when installing, and (2) the cache is designed to be append-only (so installing into separate venvs concurrently should also be fine).

---

_Comment by @charlesnicholson on 2024-03-30 12:25_

That's great to hear! Could I ask that this be part of the documentation? It would help people with pip scars  :)

---

_Renamed from "Is it safe to run multiple `uv` instances concurrently?" to "Document that it's safe to run multiple `uv` instances in a single venv" by @charlesnicholson on 2024-03-30 12:45_

---

_Label `documentation` added by @zanieb on 2024-03-30 14:06_

---

_Comment by @charliermarsh on 2024-03-30 17:18_

Will do!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-30 17:18_

---

_Closed by @charliermarsh on 2024-04-04 04:33_

---
