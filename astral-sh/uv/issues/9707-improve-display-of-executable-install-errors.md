```yaml
number: 9707
title: "Improve display of executable install errors during `uv python install`"
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - error messages
assignees: []
created_at: 2024-12-07T17:01:26Z
updated_at: 2024-12-09T03:20:44Z
url: https://github.com/astral-sh/uv/issues/9707
synced_at: 2026-01-12T15:59:56Z
```

# Improve display of executable install errors during `uv python install`

---

_@zanieb_

e.g.,

```
error: Failed to install cpython-3.13.0-macos-aarch64-none
  Caused by: Executable already exists at `/Users/zb/.local/bin/python3` but is not managed by uv; use `--force` to replace it
error: Failed to install cpython-3.13.0-macos-aarch64-none
  Caused by: Executable already exists at `/Users/zb/.local/bin/python` but is not managed by uv; use `--force` to replace it
```

we show ~the same message twice. It'd be nice to group these.

---

_Label `help wanted` added by @zanieb on 2024-12-07 17:01_

---

_Label `error messages` added by @zanieb on 2024-12-07 17:01_

---

_Comment by @refeed on 2024-12-09 03:20_

Hi, I'd like to work on this. Is there an easy way to reproduce this?

---
