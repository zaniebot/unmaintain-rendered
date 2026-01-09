---
number: 10426
title: "Display path separators consistently in `uv tool list` on Windows"
type: issue
state: closed
author: richieadler
labels:
  - bug
  - compatibility
assignees: []
created_at: 2025-01-09T13:33:24Z
updated_at: 2025-02-20T21:28:39Z
url: https://github.com/astral-sh/uv/issues/10426
synced_at: 2026-01-07T13:12:18-06:00
---

# Display path separators consistently in `uv tool list` on Windows

---

_Issue opened by @richieadler on 2025-01-09 13:33_

In Windows, currently paths are shown with backslashes for the tool directory but with forward slashes for the executable path itself:

```console
> tool list --show-paths
bump-pydantic v0.8.0 (C:\Users\mhuerta\AppData\Roaming\uv\data\tools\bump-pydantic)
- bump-pydantic.exe (C:/Users/mhuerta/.local/bin/bump-pydantic.exe)
cookiecutter v2.6.0 (C:\Users\mhuerta\AppData\Roaming\uv\data\tools\cookiecutter)
- cookiecutter.exe (C:/Users/mhuerta/.local/bin/cookiecutter.exe)
```

Ideally all paths in Windows should appear with backslashes.

---

_Label `bug` added by @charliermarsh on 2025-02-16 18:56_

---

_Label `compatibility` added by @charliermarsh on 2025-02-16 18:56_

---

_Renamed from "[minor improvement] uv tool list: Ensure paths are shown in a consistent way in Windows for --show-paths" to "Display path separators consistently in `uv tool list` on Windows" by @zanieb on 2025-02-16 19:55_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-02-20 13:35_

---

_Referenced in [astral-sh/uv#11667](../../astral-sh/uv/pulls/11667.md) on 2025-02-20 14:42_

---

_Closed by @jtfmumm on 2025-02-20 21:28_

---
