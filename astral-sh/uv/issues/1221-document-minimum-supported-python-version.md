---
number: 1221
title: Document minimum supported python version
type: issue
state: closed
author: konstin
labels:
  - documentation
assignees: []
created_at: 2024-02-01T11:18:50Z
updated_at: 2024-02-04T21:56:01Z
url: https://github.com/astral-sh/uv/issues/1221
synced_at: 2026-01-10T01:23:05Z
---

# Document minimum supported python version

---

_Issue opened by @konstin on 2024-02-01 11:18_

References:

Python support timelines (https://devguide.python.org/versions/)

![Python support timelines](https://github.com/astral-sh/puffin/assets/6826232/21ee05b8-1917-4ce3-b218-3a5c04bce698)

Ruff downloads by python minor version (https://pypistats.org/packages/ruff)

![Ruff downloads by python minor version](https://github.com/astral-sh/puffin/assets/6826232/a2c55120-1472-4ccd-b90e-c92bd800ef8b)

I can add the minimum supported python versions for some popular libraries and frameworks if relevant.


---

_Assigned to @charliermarsh by @konstin on 2024-02-01 11:18_

---

_Comment by @charliermarsh on 2024-02-01 14:12_

Any reason that we shouldn't support 3.7? I don't think it costs us anything.

---

_Label `documentation` added by @charliermarsh on 2024-02-01 14:12_

---

_Comment by @konstin on 2024-02-02 12:04_

I don't want to encourage using unsupported versions and we'd have to build 3.7 testing infrastructure (mainly the standalone builds). I'm fine if it works incidentally, but I'd prefer the official python version support being all non-EOL python version at the latest patch version.

---

_Referenced in [astral-sh/uv#1239](../../astral-sh/uv/pulls/1239.md) on 2024-02-02 21:55_

---

_Closed by @charliermarsh on 2024-02-04 21:56_

---

_Referenced in [astral-sh/uv#10454](../../astral-sh/uv/issues/10454.md) on 2025-01-10 02:52_

---
