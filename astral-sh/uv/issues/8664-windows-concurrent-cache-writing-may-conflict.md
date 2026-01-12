```yaml
number: 8664
title: "Windows: concurrent cache writing may conflict"
type: issue
state: closed
author: j178
labels:
  - bug
  - windows
  - cache
assignees: []
created_at: 2024-10-29T15:56:35Z
updated_at: 2025-04-08T15:45:24Z
url: https://github.com/astral-sh/uv/issues/8664
synced_at: 2026-01-12T15:59:31Z
```

# Windows: concurrent cache writing may conflict

---

_@j178_

On Windows, when running multiple uv instances in parallel (as we do in our CI, using uv as a test runner), there's a risk of concurrent writes to the cache directory, potentially resulting in "Access denied" errors.

For instance, when executing `uv python list`, uv writes interpreter information to the cache. With multiple uv processes running simultaneously, these write operations may conflict. This issue isn't limited to this specific command; there might be other scenarios where cache writing occurs without file locking.

## Steps to reproduce

```console
$ rm -Recurse -Force C:\Users\nigel\AppData\Local\uv\cache\interpreter-v2\
$ Start-Process 'uv' 'python list' -NoNewWindow; Start-Process 'uv' 'python list' -NoNewWindow; Start-Process 'uv' 'python list' -NoNewWindow; Start-Process 'uv' 'python list' -NoNewWindow;
Failed to inspect Python interpreter from managed installations at `AppData\Roaming\uv\data\python\cpython-3.8.20-windows-x86_64-none\python.exe`
  Caused by: Failed to query Python interpreter
  Caused by: Failed to persist temporary file to AppData\Local\uv\cache\interpreter-v2\ae3c819d96baa6b8.msgpack: Access is denied. (os error 5)
error: Failed to inspect Python interpreter from managed installations at `AppData\Roaming\uv\data\python\cpython-3.8.20-windows-x86_64-none\python.exe`
  Caused by: Failed to query Python interpreter
  Caused by: Failed to persist temporary file to AppData\Local\uv\cache\interpreter-v2\ae3c819d96baa6b8.msgpack: Access is denied. (os error 5)
```

## Environment
```
OS: Windows
uv: 0.4.28
```



---

_Renamed from "Windows: " to "Windows: concurrent writes to cache may conflict" by @j178 on 2024-10-29 15:57_

---

_Renamed from "Windows: concurrent writes to cache may conflict" to "Windows: concurrent cache writing may conflict" by @j178 on 2024-10-29 15:57_

---

_Label `bug` added by @zanieb on 2024-10-29 16:01_

---

_Label `windows` added by @zanieb on 2024-10-29 16:01_

---

_Label `cache` added by @zanieb on 2024-10-29 16:01_

---

_Closed by @j178 on 2025-04-08 15:45_

---
