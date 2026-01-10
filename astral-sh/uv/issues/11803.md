```yaml
number: 11803
title: "No `uv tool update-shell` suggestion in powershell"
type: issue
state: closed
author: konstin
labels:
  - bug
  - windows
assignees: []
created_at: 2025-02-26T17:05:33Z
updated_at: 2025-03-01T03:32:49Z
url: https://github.com/astral-sh/uv/issues/11803
synced_at: 2026-01-10T03:50:31Z
```

# No `uv tool update-shell` suggestion in powershell

---

_Issue opened by @konstin on 2025-02-26 17:05_

### Summary

I'm using powershell 7.4.6 on Windows 11. When the tool directory is not in PATH, our hint on powershell is missing the `uv tool update-shell` hint:

```
$ uv tool install black
Resolved 7 packages in 10ms
Installed 7 packages in 57ms
 + black==25.1.0
 + click==8.1.8
 + colorama==0.4.6
 + mypy-extensions==1.0.0
 + packaging==24.2
 + pathspec==0.12.1
 + platformdirs==4.3.6
Installed 2 executables: black.exe, blackd.exe
warning: `C:\Users\Konsti\.local\bin` is not on your PATH. To use installed tools, run `$env:PATH = "C:`\Users`\Konsti`\.local`\bin;$env:PATH"`.
```

`uv tool update-shell` is the correct suggestion here and does work, we should show it here.

Additionally, the escape backticks are conflicting with the outer backticks.

### Platform

Windows 11 x86_64

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

n/a

---

_Label `bug` added by @konstin on 2025-02-26 17:05_

---

_Label `windows` added by @konstin on 2025-02-26 17:05_

---

_Comment by @charliermarsh on 2025-02-27 00:07_

Hm... Is the `SHELL` env var set? It's hard to know why this isn't triggering without more logging.

---

_Comment by @konstin on 2025-02-27 08:47_

I think we match this branch because powershell has no config files (but we can still install it, cause on windows, PATH is in the registry, not in the shell config), this just needs some cross shell testing on windows (if you're running bash on windows, do we edit the registry or the bash config?):

https://github.com/astral-sh/uv/blob/4c161d284bac47104db1ccd2e73daeb2e4d0d182/crates/uv/src/commands/tool/common.rs#L304

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-28 02:07_

---

_Closed by @charliermarsh on 2025-03-01 03:32_

---
