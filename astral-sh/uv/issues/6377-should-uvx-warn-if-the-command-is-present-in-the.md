---
number: 6377
title: "Should `uvx` warn if the command is present in the project environment?"
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2024-08-21T21:00:35Z
updated_at: 2024-08-21T21:01:08Z
url: https://github.com/astral-sh/uv/issues/6377
synced_at: 2026-01-10T01:23:59Z
---

# Should `uvx` warn if the command is present in the project environment?

---

_Issue opened by @zanieb on 2024-08-21 21:00_

e.g. `uvx pytest` will probably fail. When `pytest` is a dependency of the project in the working directory, should we just use it? Should we warn? etc.

Ref https://github.com/astral-sh/uv/issues/6376#issuecomment-2302988063

---

_Label `error messages` added by @zanieb on 2024-08-21 21:01_

---

_Referenced in [astral-sh/uv#7186](../../astral-sh/uv/issues/7186.md) on 2024-09-08 13:22_

---
