---
number: 13914
title: "`uv build` in a workspace: output directory is weird/inconsistent"
type: issue
state: closed
author: dimaqq
labels:
  - bug
assignees: []
created_at: 2025-06-09T07:22:13Z
updated_at: 2025-10-19T12:21:17Z
url: https://github.com/astral-sh/uv/issues/13914
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv build` in a workspace: output directory is weird/inconsistent

---

_Issue opened by @dimaqq on 2025-06-09 07:22_

### Summary

I have a workspace with:
- a package at top level
- a sub-package under `./testing`

When building is run at top level:
- `uv build` builds the top-level package, output in `./dist`, aka `repo/dist`
- `uv build testing` builds the sub-package, output in `./dist`, aka `repo/dist`
- `uv build testing/` builds the sub-package, output in `./testing/dist`, aka `repo/testing/dist`

The difference between the last two is kinda weird.

Likewise, when building in the subdirectory:
- `uv build` builds the sub-package, output in `../dist`, aka `repo/dist`
- `uv build .` builds the sub-package, output in `../dist`, aka `repo/dist`
- `uv build ./` builds the sub-package, output in `./dist`, aka `repo/testing/dist`

Could it be that `uv` treats an `arg` as a name and `arg/` as a location?
(note that in my case, the subdirectory name doesn't match the package name, so that's probably not the case).

For the subdirectory case, I'm not even sure what the correct behaviour should be.
`../dist` suggests that uv groks that this is a workspace... or maybe it's a bug?
`./dist` is backwards-compatible... though one may argue that it's against workspace ethos?

Finally:
- `uv build --all` at top-level builds all packages with output in `./dist` aka `repo/dist`

### Platform

macos

### Version

uv 0.7.12 (Homebrew 2025-06-06)

### Python version

any

---

_Label `bug` added by @dimaqq on 2025-06-09 07:22_

---

_Assigned to @konstin by @zanieb on 2025-06-09 14:19_

---

_Referenced in [astral-sh/uv#15133](../../astral-sh/uv/pulls/15133.md) on 2025-08-07 12:24_

---

_Comment by @konstin on 2025-08-07 12:30_

We're fixing this in #15133

---

_Comment by @konstin on 2025-10-19 12:21_

This is fixed in 0.9.

---

_Closed by @konstin on 2025-10-19 12:21_

---
