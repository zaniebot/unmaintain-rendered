---
number: 13582
title: "`uv python install --reinstall` does not include pre-release versions"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-05-21T20:02:58Z
updated_at: 2025-05-27T14:35:50Z
url: https://github.com/astral-sh/uv/issues/13582
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv python install --reinstall` does not include pre-release versions

---

_Issue opened by @zanieb on 2025-05-21 20:02_

### Summary

e.g.,

```
❯ uv python install 3.14 3.13
Installed 2 versions in 23.89s
 + cpython-3.13.3-macos-aarch64-none
 + cpython-3.14.0a7-macos-aarch64-none
❯ uv python install --no-config --reinstall
Installed Python 3.13.3 in 11.45s
 ~ cpython-3.13.3-macos-aarch64-none
```

Presumably we're using a `Default` selector where we should be using `Any`?

### Platform

n/a

### Version

0.7.6

### Python version

n/a

---

_Label `bug` added by @zanieb on 2025-05-21 20:02_

---

_Comment by @zanieb on 2025-05-21 20:04_

Hm we do have logic for this...

https://github.com/astral-sh/uv/blob/b7167dc4d829c88baf8dc599f7f8cfad98a7df1c/crates/uv/src/commands/python/install.rs#L166-L171

Probably a deeper problem?

---

_Comment by @Michaelgathara on 2025-05-25 23:54_

@zanieb I believe I have fixed this up within #13645
Open to thoughts

---

_Referenced in [astral-sh/uv#13645](../../astral-sh/uv/pulls/13645.md) on 2025-05-27 14:35_

---

_Closed by @zanieb on 2025-05-27 14:35_

---
