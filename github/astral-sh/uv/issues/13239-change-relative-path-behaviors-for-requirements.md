---
number: 13239
title: "Change relative path behaviors for `requirements` files"
type: issue
state: open
author: zanieb
labels:
  - needs-design
  - breaking
assignees: []
created_at: 2025-04-30T17:36:04Z
updated_at: 2025-10-12T21:16:50Z
url: https://github.com/astral-sh/uv/issues/13239
synced_at: 2026-01-07T13:12:18-06:00
---

# Change relative path behaviors for `requirements` files

---

_Issue opened by @zanieb on 2025-04-30 17:36_

One part of this is

- https://github.com/astral-sh/uv/issues/3271

Similarly, relative paths are relative to the working directory, not the file.

An option...

- `../<path>` to be relative to the file
- `${PWD}/<path>` to be relative to the cwd
- `${PROJECT_ROOT}/<path>` to be relative to the nearest directory with a pyproject.toml, or, if not found, the file
- `${WORKSPACE_ROOT}/<path>` to be relative to the uv workspace root, or, if not found, the file

There are a couple problems here:

- We need consensus on what it _should_ be
- We need a way to roll it out
- We need a way to deal with `pip` having different behavior

---

_Label `needs-design` added by @zanieb on 2025-04-30 17:36_

---

_Added to milestone `v0.8.0` by @zanieb on 2025-04-30 17:36_

---

_Label `breaking` added by @zanieb on 2025-04-30 17:36_

---

_Removed from milestone `v0.8.0` by @zanieb on 2025-06-20 15:14_

---

_Added to milestone `v0.9.0` by @zanieb on 2025-06-20 15:14_

---

_Referenced in [astral-sh/uv#3271](../../astral-sh/uv/issues/3271.md) on 2025-07-10 13:39_

---

_Removed from milestone `v0.9.0` by @zanieb on 2025-10-12 21:16_

---

_Added to milestone `v0.10.0` by @zanieb on 2025-10-12 21:16_

---

_Referenced in [astral-sh/uv#16299](../../astral-sh/uv/issues/16299.md) on 2025-10-14 17:31_

---
