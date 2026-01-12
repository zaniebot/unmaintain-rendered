```yaml
number: 15967
title: "The command is not in PATH if `--no-cache` is used in `uv tool run`"
type: issue
state: closed
author: monosans
labels:
  - bug
assignees: []
created_at: 2025-09-21T06:43:20Z
updated_at: 2025-09-22T23:48:00Z
url: https://github.com/astral-sh/uv/issues/15967
synced_at: 2026-01-12T16:02:20Z
```

# The command is not in PATH if `--no-cache` is used in `uv tool run`

---

_@monosans_

### Summary

uv 0.8.19 breaks `uv tool run --no-cache`


```bash
> uv tool run --no-cache prek --help

2025-09-21T02:16:02.5785518Z Downloading cpython-3.13.7-linux-x86_64-gnu (download) (32.0MiB)
2025-09-21T02:16:03.3808032Z  Downloading cpython-3.13.7-linux-x86_64-gnu (download)
2025-09-21T02:16:03.5727932Z Downloading prek (4.4MiB)
2025-09-21T02:16:03.6101656Z  Downloading prek
2025-09-21T02:16:03.6125044Z Installed 1 package in 1ms
2025-09-21T02:16:03.7630297Z error: Failed to spawn: `prek`
2025-09-21T02:16:03.7630879Z   Caused by: No such file or directory (os error 2)
2025-09-21T02:16:03.7669612Z ##[error]Process completed with exit code 2.
```

### Platform

Ubuntu 24.04 amd64, Windows 11 amd64

### Version

0.8.19

### Python version

3.13.7

---

_Label `bug` added by @monosans on 2025-09-21 06:43_

---

_Comment by @my1e5 on 2025-09-22 10:27_

Getting the same on macOS
```
$ uvx --with uv==0.8.18 uv tool run --no-cache ty --version
ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

$ uvx --with uv==0.8.19 uv tool run --no-cache ty --version
error: Failed to spawn: `ty`
  Caused by: No such file or directory (os error 2)
```

---

_Closed by @zanieb on 2025-09-22 20:41_

---

_Comment by @zanieb on 2025-09-22 23:48_

This is fixed in 0.8.20

Thanks for the report!

---
