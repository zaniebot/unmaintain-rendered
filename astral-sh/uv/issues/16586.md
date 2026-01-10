```yaml
number: 16586
title: "`uv cache clean` fails on Windows when sdist contains special filenames"
type: issue
state: open
author: sunfkny
labels:
  - bug
  - help wanted
  - windows
  - great writeup
assignees: []
created_at: 2025-11-04T07:45:54Z
updated_at: 2025-12-18T16:19:21Z
url: https://github.com/astral-sh/uv/issues/16586
synced_at: 2026-01-10T03:11:35Z
```

# `uv cache clean` fails on Windows when sdist contains special filenames

---

_Issue opened by @sunfkny on 2025-11-04 07:45_

### Summary

Moved from https://github.com/astral-sh/uv/issues/15569#issuecomment-3477947525

## Description

On Windows, when running `uv add uwsgi`, the build process fails (which is expected as uwsgi cannot compile on Windows). However, when subsequently running uv cache clean, the command fails because the cached sdist contains a file with a Windows-incompatible name (ending with a period).

## Steps to Reproduce

On Windows, create a new project and add `uwsgi`.

```shell
uv init
uv add uwsgi
```

The build fails as expected since `uwsgi` is not supported on Windows.

Then run:

```shell
uv cache clean
```

## Expected Behavior

`uv cache clean` should remove cached source distributions (sdists).

## Actual Behavior

```text
Clearing cache at: AppData\Local\uv\cache
error: Failed to clear cache at: AppData\Local\uv\cache
  Caused by: failed to remove file `C:\Users\root\AppData\Local\uv\cache\sdists-v9\index\3ba65c4f41aac3a1\uwsgi\2.0.31\l1w3t3Nrz8OnzxF6ri3eg\src\core\logging.`: The system cannot find the file specified. (os error 2)
```

## Suggested Fix

Use logic similar to Python’s [isreserved](https://github.com/python/cpython/blob/947bb4642c612b66de7cae815aaf9c570a93882a/Lib/ntpath.py#L311-L316) to detect and handle reserved or invalid Windows filenames before deletion.

When removing such files, use the extended `\\?\` path prefix to bypass Win32 path normalization, which allows operations on paths exceeding `MAX_PATH` or containing characters normally restricted by the Win32 API (such as trailing dots or spaces).


### Platform

Windows 11 24H2

### Version

uv 0.9.7 (0adb44480 2025-10-30)

### Python version

Python 3.13.5

---

_Label `bug` added by @sunfkny on 2025-11-04 07:45_

---

_Label `windows` added by @konstin on 2025-11-04 16:03_

---

_Label `great writeup` added by @konstin on 2025-11-04 16:08_

---

_Label `help wanted` added by @konstin on 2025-12-18 16:19_

---
