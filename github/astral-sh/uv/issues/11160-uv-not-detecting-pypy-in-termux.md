---
number: 11160
title: uv not detecting pypy in termux
type: issue
state: open
author: Vinfall
labels:
  - bug
assignees: []
created_at: 2025-02-02T08:38:53Z
updated_at: 2025-02-02T08:39:26Z
url: https://github.com/astral-sh/uv/issues/11160
synced_at: 2026-01-07T13:12:18-06:00
---

# uv not detecting pypy in termux

---

_Issue opened by @Vinfall on 2025-02-02 08:38_

### Summary

I am not using the latest version, so I'll wait a few days and see if it's solved in 0.5.26 as I noticed it has python detection bugfix. This is opened just in case it persists.

Quoted my comment from https://github.com/astral-sh/uv/issues/7373#issuecomment-2629265658:

uv on Termux now works quite well except `uv python install` but that's a upstream issue and on Termux we have official PyPy packages and TUR for older Pythons.

However, if I have installed both `python3` and `pypy3`, only python3 is detected (despite pypy being in `$PATH`).

### Steps to reproduce
```sh
# python interpreter and uv
$ pkg install python3 python3-pip pypy3 uv

$ uv python list
cpython-3.12.8-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python3.12
cpython-3.12.8-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python3 -> python3.12
cpython-3.12.8-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python -> python3.12
```

### Expected
uv detects python3 and pypy3.
### Actual behavior
only python3 is detected.

### Verbose log

not quite tbh:
```sh
$ uv --verbose python list
DEBUG uv 0.5.25
DEBUG Searching for any Python interpreter in managed installations or search path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found `cpython-3.12.8-linux-aarch64-none` at `/data/data/com.termux/files/usr/bin/python` (search path)
DEBUG Found `cpython-3.12.8-linux-aarch64-none` at `/data/data/com.termux/files/usr/bin/python3` (search path)
DEBUG Failed to inspect Python interpreter from search path at `/data/data/com.termux/files/usr/bin/pypy3` 
DEBUG Skipping bad interpreter at /data/data/com.termux/files/usr/bin/pypy3 from search path: Could not detect a glibc or a musl libc (while running on Linux)
DEBUG Found `cpython-3.12.8-linux-aarch64-none` at `/data/data/com.termux/files/usr/bin/python3.12` (search path)
cpython-3.12.8-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python3.12
cpython-3.12.8-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python3 -> python3.12
cpython-3.12.8-linux-aarch64-none    /data/data/com.termux/files/usr/bin/python -> python3.12
```

### PATH

```sh
$ echo $PATH | sed 's/:/\n/g'
/data/data/com.termux/files/home/.local/bin
/data/data/com.termux/files/home/.local/bin-sh
/data/data/com.termux/files/home/.local/bin-rust
/data/data/com.termux/files/usr/bin
$ which python3
/data/data/com.termux/files/usr/bin/python3
$ which pypy3
/data/data/com.termux/files/usr/bin/pypy3
```

Like I said in the linked issue, it's exact the same cause and I don't see why I need to write it twice... Actually the original issue has more information than mine.

### Platform

Linux 4.14.186+ aarch64 Android

### Version

uv 0.5.25 (from termux)

### Python version

Python 3.12.8 and undetected pypy3 (Python 3.9.18 (9c4f8ef178b6, Oct 06 2024, 19:31:31))

---

_Label `bug` added by @Vinfall on 2025-02-02 08:38_

---
