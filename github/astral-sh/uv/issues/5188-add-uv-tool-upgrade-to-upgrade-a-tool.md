---
number: 5188
title: "Add `uv tool upgrade` to upgrade a tool"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - preview
assignees: []
created_at: 2024-07-18T14:24:06Z
updated_at: 2024-08-08T20:48:16Z
url: https://github.com/astral-sh/uv/issues/5188
synced_at: 2026-01-07T13:12:17-06:00
---

# Add `uv tool upgrade` to upgrade a tool

---

_Issue opened by @zanieb on 2024-07-18 14:24_

e.g. given `uv tool upgrade <package name>` we'd upgrade the tool associated with the package to the latest version within the constraints. The user cannot specify new constraints here, they'd need to use `uv tool install` for that.

---

_Label `help wanted` added by @zanieb on 2024-07-18 14:24_

---

_Label `preview` added by @zanieb on 2024-07-18 14:24_

---

_Comment by @zanieb on 2024-07-18 14:25_

I think the main benefit of this design is that we can provide an `upgrade --all` flag to upgrade all tools.

Otherwise I think this is equivalent to `uv tool install <name> --upgrade`?

---

_Comment by @blueraft on 2024-07-18 17:23_

I can do this one

---

_Referenced in [astral-sh/uv#5197](../../astral-sh/uv/pulls/5197.md) on 2024-07-18 18:58_

---

_Closed by @charliermarsh on 2024-08-08 20:48_

---

_Closed by @charliermarsh on 2024-08-08 20:48_

---
