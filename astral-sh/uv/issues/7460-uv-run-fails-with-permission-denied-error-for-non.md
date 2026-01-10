---
number: 7460
title: uv run fails with Permission Denied error for non existing command / script in WSL when appendWindowsPath is enabled
type: issue
state: open
author: hille721
labels: []
assignees: []
created_at: 2024-09-17T14:07:42Z
updated_at: 2024-09-17T14:17:50Z
url: https://github.com/astral-sh/uv/issues/7460
synced_at: 2026-01-10T01:24:15Z
---

# uv run fails with Permission Denied error for non existing command / script in WSL when appendWindowsPath is enabled

---

_Issue opened by @hille721 on 2024-09-17 14:07_

When invoking `uv run` on a command or script which does not exist, I'm getting a permission denied error instead of an error message saying that the command / file does not exist: 

```
$ uv run --verbose foobar.py
DEBUG uv 0.4.10
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0rc2-linux-x86_64-gnu`
DEBUG Found `cpython-3.13.0rc2-linux-x86_64-gnu` at `/home/hille/.local/share/uv/python/cpython-3.13.0rc2-linux-x86_64-gnu/bin/python3` (managed installations)
DEBUG Skipping pre-release cpython-3.13.0rc2-linux-x86_64-gnu
DEBUG Found managed installation `cpython-3.12.6-linux-x86_64-gnu`
DEBUG Found `cpython-3.12.6-linux-x86_64-gnu` at `/home/hille/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/bin/python3` (managed installations)
DEBUG Using Python 3.12.6 interpreter at: /home/hille/.local/share/uv/python/cpython-3.12.6-linux-x86_64-gnu/bin/python3
DEBUG Running `foobar.py`
error: Failed to spawn: `foobar.py`
  Caused by: Permission denied (os error 13)
```  

Running on WSL2 Ubuntu 20.04


---

_Comment by @hille721 on 2024-09-17 14:16_

well, I found the issue by myself.

This issue only comes when enabling the appending of Windows paths in the `/etc/wsl.conf`:

```
[interop]
appendWindowsPath = true
```

If I set this flag to `false` I'm getting the expected "No such file or directory" error:

```
uv run foobar.py
error: Failed to spawn: `foobar.py`
  Caused by: No such file or directory (os error 2)
```


  



---

_Renamed from "uv run fails with Permission Denied error for non existing command / script" to "uv run fails with Permission Denied error for non existing command / script in WSL" by @hille721 on 2024-09-17 14:16_

---

_Renamed from "uv run fails with Permission Denied error for non existing command / script in WSL" to "uv run fails with Permission Denied error for non existing command / script in WSL when appendWindowsPath is enabled" by @hille721 on 2024-09-17 14:17_

---
