---
number: 15955
title: "`uv python upgrade` doesn't replace binary in bin home for release candidates"
type: issue
state: closed
author: zsol
labels:
  - bug
assignees: []
created_at: 2025-09-19T20:51:24Z
updated_at: 2025-09-22T16:59:49Z
url: https://github.com/astral-sh/uv/issues/15955
synced_at: 2026-01-10T01:26:01Z
---

# `uv python upgrade` doesn't replace binary in bin home for release candidates

---

_Issue opened by @zsol on 2025-09-19 20:51_

### Summary



```
❯ uv -vvv python upgrade 3.14
DEBUG uv 0.8.19 (fc7c2f8b5 2025-09-19)
warning: `uv python upgrade` is experimental and may change without warning. Pass `--preview-features python-upgrade` to disable this warning
TRACE Checking lock for `/Users/zsol/.local/share/uv/python` at `/Users/zsol/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/Users/zsol/.local/share/uv/python`
TRACE Found existing installation cpython-3.14.0rc3-macos-aarch64-none
TRACE Found existing installation cpython-3.14.0rc2-macos-aarch64-none
TRACE Found existing installation cpython-3.13.7-macos-aarch64-none
TRACE Found existing installation cpython-3.12.11-macos-aarch64-none
TRACE Found existing installation cpython-3.11.13-macos-aarch64-none
TRACE Found existing installation cpython-3.10.18-macos-aarch64-none
TRACE Found existing installation cpython-3.8.20-macos-aarch64-none
DEBUG Found `cpython-3.14.0rc3-macos-aarch64-none` for request `3.14 (cpython-3.14-macos-aarch64-none)`
DEBUG Using request timeout of 30s
TRACE Discovered `sysconfig` data at: /Users/zsol/.local/share/uv/python/cpython-3.14.0rc3-macos-aarch64-none/lib/python3.14/_sysconfigdata__darwin_darwin.py
TRACE No updates required
TRACE Discovered `pkgconfig` data at: /Users/zsol/.local/share/uv/python/cpython-3.14.0rc3-macos-aarch64-none/lib/pkgconfig/python-3.14-embed.pc
TRACE Discovered `pkgconfig` data at: /Users/zsol/.local/share/uv/python/cpython-3.14.0rc3-macos-aarch64-none/lib/pkgconfig/python-3.14.pc
DEBUG Inspecting existing executable at `/Users/zsol/.local/bin/python3.14`
DEBUG Executable already exists for `cpython-3.14.0rc2-macos-aarch64-none` at `/Users/zsol/.local/bin/python3.14`. Use `--force` to replace it
DEBUG Released lock at `/Users/zsol/.local/share/uv/python/.lock`
```

I expected a message like
```
DEBUG Replacing existing executable for `cpython-3.14.0rc2-macos-aarch64-none` at `/Users/zsol/.local/bin/python3.14` with executable for `cpython-3.14.0rc3-macos-aarch64-none` since it is an upgrade
```

### Platform

macos 15.6.1 arm64

### Version

0.8.19

### Python version

_No response_

---

_Label `bug` added by @zsol on 2025-09-19 20:51_

---

_Comment by @zanieb on 2025-09-19 23:04_

Just confirming I can reproduce this
```
❯ uv self update 0.8.18
info: Checking for updates...
success: Downgraded uv from v0.8.19 to v0.8.18! https://github.com/astral-sh/uv/releases/tag/0.8.18
❯ uv python uninstall 3.14
Searching for Python versions matching: Python 3.14
Uninstalled Python 3.14.0rc3 in 92ms
 - cpython-3.14.0rc3-macos-aarch64-none (python3.14)
❯ uv python install 3.14
Installed Python 3.14.0rc2 in 1.51s
 + cpython-3.14.0rc2-macos-aarch64-none (python3.14)
❯ python3.14 --version
Python 3.14.0rc2
❯ uv self update
info: Checking for updates...
success: Upgraded uv from v0.8.18 to v0.8.19! https://github.com/astral-sh/uv/releases/tag/0.8.19
❯ uv python upgrade
warning: `uv python upgrade` is experimental and may change without warning. Pass `--preview-features python-upgrade` to disable this warning
Installed Python3.14.0rc3 in 2.58s
 + cpython-3.14.0rc3-macos-aarch64-none
❯ python3.14 --version
Python 3.14.0rc2
```

---

_Referenced in [astral-sh/uv#15959](../../astral-sh/uv/pulls/15959.md) on 2025-09-20 11:39_

---

_Closed by @zanieb on 2025-09-22 16:59_

---
