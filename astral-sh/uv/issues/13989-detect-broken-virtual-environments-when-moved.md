---
number: 13989
title: Detect broken virtual environments when moved
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-06-12T13:21:36Z
updated_at: 2025-06-12T13:38:38Z
url: https://github.com/astral-sh/uv/issues/13989
synced_at: 2026-01-10T01:25:41Z
---

# Detect broken virtual environments when moved

---

_Issue opened by @zanieb on 2025-06-12 13:21_

> We can read the beginning of all entrypoints we created (through `entry_points.txt`) and if any of them points to a python interpreter that doesn't exist, we invalidate the environment. There are some details to be figured out around perf and where the scripts come from, e.g. we don't cover non-entrypoint scripts (I don't think we keep a record of `#!python` script we rewrote vs. script where the shebang was user-provided).
> 
> Another approach is recording the venv location (either `pyvenv.cfg` or a different file we create, iirc it's not recorded yet anywhere) and invalidating it if the original location and the current location aren't the same directory (usually, because the original location doesn't exist anymore, but we want to allow symlinks above the `.venv`). 

 _Originally posted by @konstin in [#13196](https://github.com/astral-sh/uv/issues/13196#issuecomment-2849277210)_

In projects, we can invalidate and recreate these environments. In other contexts, we can improve our error messages or warn.

---

_Comment by @zanieb on 2025-06-12 13:30_

There's an existing issue for project environment invalidation

- https://github.com/astral-sh/uv/issues/10895

We should close that out when this is done too.

---

_Referenced in [astral-sh/uv#13992](../../astral-sh/uv/issues/13992.md) on 2025-06-12 13:33_

---

_Referenced in [astral-sh/uv#13994](../../astral-sh/uv/issues/13994.md) on 2025-06-12 13:38_

---

_Label `enhancement` added by @zanieb on 2025-06-12 13:38_

---

_Referenced in [astral-sh/uv#13196](../../astral-sh/uv/issues/13196.md) on 2025-06-12 15:13_

---

_Referenced in [astral-sh/uv#12363](../../astral-sh/uv/issues/12363.md) on 2025-06-26 17:24_

---

_Referenced in [astral-sh/uv#11454](../../astral-sh/uv/issues/11454.md) on 2025-08-19 11:53_

---
