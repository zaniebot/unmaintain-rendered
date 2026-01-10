---
number: 15983
title: "Add `--all-variants` option to show debug builds in `uv python list`"
type: issue
state: open
author: albanD
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-09-22T14:29:34Z
updated_at: 2025-09-22T17:25:20Z
url: https://github.com/astral-sh/uv/issues/15983
synced_at: 2026-01-10T01:26:02Z
---

# Add `--all-variants` option to show debug builds in `uv python list`

---

_Issue opened by @albanD on 2025-09-22 14:29_

### Summary

Truncated result is
```bash
$ uv python list --only-downloads
cpython-3.14.0rc3-linux-x86_64-gnu                       <download available>
cpython-3.14.0rc3+freethreaded-linux-x86_64-gnu          <download available>
cpython-3.14.0rc3+freethreaded+debug-linux-x86_64-gnu    <download available>
cpython-3.13.7-linux-x86_64-gnu                          <download available>
cpython-3.13.7+freethreaded-linux-x86_64-gnu             <download available>
cpython-3.13.7+freethreaded+debug-linux-x86_64-gnu       <download available>
... more cpython versions below
```

But these versions can actually be installed
```bash
$ uv python install 3.13+debug
cpython-3.13.7+debug-linux-x86_64-gnu (download) ...
```

### Platform

Linux 6.15.3-100.fc41.x86_64 x86_64 GNU/Linux  (Fedora)

### Version

uv 0.8.19

### Python version

3.12.3

---

_Label `bug` added by @albanD on 2025-09-22 14:29_

---

_Comment by @zanieb on 2025-09-22 14:31_

https://github.com/astral-sh/uv/blob/8f3583a6e63800b770fec3b7be9b754be9d65602/crates/uv/src/commands/python/list.rs#L113-L114

We hide these intentionally. We need an `--all-variants` option or something.

---

_Label `good first issue` added by @zanieb on 2025-09-22 14:31_

---

_Comment by @zanieb on 2025-09-22 14:31_

This needs a little design work, but should be pretty easy.

---

_Comment by @albanD on 2025-09-22 14:59_

That sounds good!
I would also suggest hiding the PythonVariant::FreethreadedDebug when this option is not passed in.

---

_Comment by @zanieb on 2025-09-22 15:41_

Definitely, that was an oversight.

---

_Referenced in [astral-sh/uv#15985](../../astral-sh/uv/pulls/15985.md) on 2025-09-22 15:43_

---

_Renamed from "uv python list is missing cpython (no-freethreaded) debug builds" to "Add `--all-variants` option to show debug builds in `uv python list`" by @zanieb on 2025-09-22 17:24_

---

_Comment by @zanieb on 2025-09-22 17:25_

We should also emit debug builds if it's included in the request, e.g., `uv python list 3.13+debug`

---

_Referenced in [astral-sh/uv#16002](../../astral-sh/uv/pulls/16002.md) on 2025-09-23 14:46_

---
