---
number: 7280
title: "`uv python list --all-versions` does not include `3.13.0rc2`"
type: issue
state: closed
author: j178
labels:
  - bug
  - uv python
assignees: []
created_at: 2024-09-11T03:59:35Z
updated_at: 2024-09-11T19:46:17Z
url: https://github.com/astral-sh/uv/issues/7280
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv python list --all-versions` does not include `3.13.0rc2`

---

_Issue opened by @j178 on 2024-09-11 03:59_

version: uv 0.4.9 (77d278f68 2024-09-10)

```console
$ uv python list --all-versions
cpython-3.12.6-macos-aarch64-none     <download available>
cpython-3.12.5-macos-aarch64-none     /Users/Jo/.local/share/uv/python/cpython-3.12.5-macos-aarch64-none/bin/python3 -> python3.12
cpython-3.12.4-macos-aarch64-none     <download available>
cpython-3.12.3-macos-aarch64-none     <download available>
...
```

---

_Comment by @zanieb on 2024-09-11 13:28_

Thanks, this is a side-effect of https://github.com/astral-sh/uv/pull/7278 — I'll fix this.

---

_Label `bug` added by @zanieb on 2024-09-11 13:28_

---

_Label `uv python` added by @zanieb on 2024-09-11 13:28_

---

_Assigned to @zanieb by @zanieb on 2024-09-11 13:28_

---

_Referenced in [astral-sh/uv#7290](../../astral-sh/uv/pulls/7290.md) on 2024-09-11 13:56_

---

_Closed by @zanieb on 2024-09-11 19:46_

---

_Closed by @zanieb on 2024-09-11 19:46_

---
