---
number: 17028
title: "File URLs to Windows networks paths wrongly rewritten [not a bug, user error]"
type: issue
state: closed
author: FabianHellerViseca
labels:
  - bug
assignees: []
created_at: 2025-12-08T10:24:36Z
updated_at: 2025-12-08T13:18:31Z
url: https://github.com/astral-sh/uv/issues/17028
synced_at: 2026-01-10T01:26:12Z
---

# File URLs to Windows networks paths wrongly rewritten [not a bug, user error]

---

_Issue opened by @FabianHellerViseca on 2025-12-08 10:24_

### Summary

When I set e.g. `UV_PYTHON_DOWNLOADS_JSON_URL="file:////my_server/share/download-metadata-windows.json"`
uv returns `error: Invalid download URL: file:///my_server/share/download-metadata-windows.json` because it wrongly replaces multiple "/" with a single "/". The same also happens with file URLs inside the UV_PYTHON_DOWNLOADS_JSON_URL file. curl can handle such file URLs starting with "file:////" correctly.

Minimal reproduceable example (here in Git Bash, the network share doesn't need to exist to see the bug):
```bash
UV_PYTHON_DOWNLOADS_JSON_URL="file:////my_server/share/download-metadata-windows.json" uv venv test_py_13 --python=3.13.11 --verbose
DEBUG uv 0.9.16 (a63e5b62e 2025-12-06)
DEBUG Acquired shared lock for `C:\Users\...\AppData\Local\uv\cache`
DEBUG Using Python request `3.13.11` from explicit request
DEBUG Using request timeout of 30s
DEBUG Released lock at `C:\Users\...\AppData\Local\uv\cache\.lock`
error: Invalid download URL: file:///my_server/share/download-metadata-windows.json
```

### Platform

Windows 11 x86_64

### Version

uv 0.9.16 (a63e5b62e 2025-12-06)

### Python version

_No response_

---

_Label `bug` added by @FabianHellerViseca on 2025-12-08 10:24_

---

_Renamed from "File URLs to Windows networks paths wrongly rewritten, (UV_PYTHON_DOWNLOADS_JSON_URL cannot point to unmapped network path)" to "File URLs to Windows networks paths wrongly rewritten [not a bug, user error]" by @FabianHellerViseca on 2025-12-08 13:17_

---

_Comment by @FabianHellerViseca on 2025-12-08 13:18_

I found that the official way to write file URLs to network shares is just to remove the two slashes, so `UV_PYTHON_DOWNLOADS_JSON_URL="file://my_server/share/download-metadata-windows.json"` works.

---

_Closed by @FabianHellerViseca on 2025-12-08 13:18_

---
