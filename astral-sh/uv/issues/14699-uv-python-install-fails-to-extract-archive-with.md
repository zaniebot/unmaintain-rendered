---
number: 14699
title: "`uv python install` fails to extract archive with \"cannot decrypt peer's message\""
type: issue
state: closed
author: char3210
labels:
  - bug
  - network
assignees: []
created_at: 2025-07-18T00:50:27Z
updated_at: 2025-09-18T07:10:08Z
url: https://github.com/astral-sh/uv/issues/14699
synced_at: 2026-01-10T01:25:47Z
---

# `uv python install` fails to extract archive with "cannot decrypt peer's message"

---

_Issue opened by @char3210 on 2025-07-18 00:50_

### Summary

Trying to install python on Windows. My unstable Wi-Fi connection may have something to do with it. It downloads for a few seconds, but randomly fails with this error message. I managed to get lucky running it once where it finished downloading without erroring out and it installed fine, but since then I haven't been able to install another version after trying it 10+ times (it always errored out sometime before it finished)

```
 >uv python install 3.10 -vv
DEBUG uv 0.8.0 (0b2357294 2025-07-17)
TRACE Checking lock for `C:\Users\char\AppData\Roaming\uv\python` at `C:\Users\char\AppData\Roaming\uv\python\.lock`
DEBUG Acquired lock for `C:\Users\char\AppData\Roaming\uv\python`
TRACE Found existing installation cpython-3.13.5-windows-x86_64-none
DEBUG No installation found for request `Python 3.10`
DEBUG Found download `cpython-3.10.18-windows-x86_64-none` for request `Python 3.10`
DEBUG Using request timeout of 30s
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.10.18%2B20250712-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
DEBUG Extracting cpython-3.10.18-20250712-x86_64-pc-windows-msvc-install_only_stripped.tar.gz to temporary location: C:\Users\char\AppData\Roaming\uv\python\.temp\.tmps53RWd
TRACE Handling request for https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.10.18%2B20250712-x86_64-pc-windows-msvc-install_only_stripped.tar.gz with authentication policy auto
TRACE Request for https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.10.18%2B20250712-x86_64-pc-windows-msvc-install_only_stripped.tar.gz is unauthenticated, checking cache
TRACE No credentials in cache for URL https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.10.18%2B20250712-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
TRACE Attempting unauthenticated request for https://github.com/astral-sh/python-build-standalone/releases/download/20250712/cpython-3.10.18%2B20250712-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
Downloading cpython-3.10.18-windows-x86_64-none (download) (21.4MiB)
TRACE Considering retry of error: ExtractError("cpython-3.10.18-20250712-x86_64-pc-windows-msvc-install_only_stripped.tar.gz", Io(Custom { kind: Other, error: TarError { desc: "failed to unpack `\\\\?\\C:\\Users\\char\\AppData\\Roaming\\uv\\python\\.temp\\.tmps53RWd\\python\\Lib\\site-packages\\pip\\_internal\\cli\\__pycache__\\autocompletion.cpython-310.pyc`", io: Custom { kind: Other, error: TarError { desc: "failed to unpack `python/Lib/site-packages/pip/_internal/cli/__pycache__/autocompletion.cpython-310.pyc` into `\\\\?\\C:\\Users\\char\\AppData\\Roaming\\uv\\python\\.temp\\.tmps53RWd\\python\\Lib\\site-packages\\pip\\_internal\\cli\\__pycache__\\autocompletion.cpython-310.pyc`", io: Custom { kind: Other, error: reqwest::Error { kind: Decode, source: reqwest::Error { kind: Body, source: hyper::Error(Body, Error { kind: Io(Custom { kind: InvalidData, error: "cannot decrypt peer's message" }) }) } } } } } } }))
TRACE Cannot retry IO error: not one of `ConnectionReset` or `UnexpectedEof`
TRACE Cannot retry IO error: not one of `ConnectionReset` or `UnexpectedEof`
TRACE Cannot retry IO error: not one of `ConnectionReset` or `UnexpectedEof`
TRACE Cannot retry error: not an IO error
error: Failed to install cpython-3.10.18-windows-x86_64-none
  Caused by: Failed to extract archive: cpython-3.10.18-20250712-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
  Caused by: I/O operation failed during extraction
  Caused by: failed to unpack `\\?\C:\Users\char\AppData\Roaming\uv\python\.temp\.tmps53RWd\python\Lib\site-packages\pip\_internal\cli\__pycache__\autocompletion.cpython-310.pyc`
  Caused by: failed to unpack `python/Lib/site-packages/pip/_internal/cli/__pycache__/autocompletion.cpython-310.pyc` into `\\?\C:\Users\char\AppData\Roaming\uv\python\.temp\.tmps53RWd\python\Lib\site-packages\pip\_internal\cli\__pycache__\autocompletion.cpython-310.pyc`
  Caused by: error decoding response body
  Caused by: request or response body error
  Caused by: error reading a body from connection
  Caused by: cannot decrypt peer's message
DEBUG Released lock at `C:\Users\char\AppData\Roaming\uv\python\.lock`
```                                                                                                                                                                                                                                           

### Platform

Windows 10 x86_64

### Version

uv 0.8.0 (0b2357294 2025-07-17)

### Python version

N/A

---

_Label `bug` added by @char3210 on 2025-07-18 00:50_

---

_Comment by @zanieb on 2025-07-18 01:42_

Thanks for the report. Can you fetch that URL directly? e.g., with `curl`?

---

_Renamed from "uv python install fails" to "`uv python install` fails to extract archive with "cannot decrypt peer's message"" by @zanieb on 2025-07-18 01:45_

---

_Comment by @char3210 on 2025-07-18 06:07_

Opening the URL in my browser downloads it fine

---

_Referenced in [astral-sh/uv#14703](../../astral-sh/uv/pulls/14703.md) on 2025-07-18 07:23_

---

_Closed by @konstin on 2025-07-20 22:28_

---

_Comment by @char3210 on 2025-07-25 03:15_

As a side note this actually seems to be an issue with my Wi-Fi adapter driver, I just haven't noticed downloads randomly failing everywhere until recently. 

---

_Label `network` added by @konstin on 2025-09-18 07:10_

---

_Referenced in [seanmonstar/reqwest#2819](../../seanmonstar/reqwest/issues/2819.md) on 2025-09-18 07:21_

---
