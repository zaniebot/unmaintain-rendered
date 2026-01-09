---
number: 16134
title: "Shouldn't `uv sync --no-editable` build & install the editable packages?"
type: issue
state: closed
author: tamir-bahar
labels:
  - question
assignees: []
created_at: 2025-10-06T08:51:21Z
updated_at: 2025-10-12T07:03:32Z
url: https://github.com/astral-sh/uv/issues/16134
synced_at: 2026-01-07T13:12:19-06:00
---

# Shouldn't `uv sync --no-editable` build & install the editable packages?

---

_Issue opened by @tamir-bahar on 2025-10-06 08:51_

### Question

I noticed that using `uv sync --no-editable` in a workspace is different from building all the packages, then installing them via their wheels.
Specifically - Cython generated `.so` files and other build artifacts are not present when using `--no-editable`.

Is there a different flag I'm supposed to be using for this to work?

The [docs](https://docs.astral.sh/uv/concepts/projects/workspaces/#when-not-to-use-workspaces) seem to indicate that using Workspaces for Cython & similar native code packages is recommended and supported, but I can't get it to work.

Thanks!

### Platform

Linux 5.14.0-284.30.1.el9_2.x86_64 x86_64 GNU/Linux

### Version

uv 0.8.23

---

_Label `question` added by @tamir-bahar on 2025-10-06 08:51_

---

_Closed by @tamir-bahar on 2025-10-12 07:03_

---

_Comment by @tamir-bahar on 2025-10-12 07:03_

It works, I made an error on my end. So closed this.

---
