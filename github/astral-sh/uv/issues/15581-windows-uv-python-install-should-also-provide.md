---
number: 15581
title: Windows uv python install should also provide python and python3 binaries (symlinks)
type: issue
state: closed
author: Tronic
labels:
  - enhancement
assignees: []
created_at: 2025-08-29T15:28:20Z
updated_at: 2025-08-29T19:47:21Z
url: https://github.com/astral-sh/uv/issues/15581
synced_at: 2026-01-07T13:12:19-06:00
---

# Windows uv python install should also provide python and python3 binaries (symlinks)

---

_Issue opened by @Tronic on 2025-08-29 15:28_

### Summary

After `uv python install` I can start Python with the exact version number e.g. `python3.13` but `python` and `python3` still open Microsoft Store to install Python with the official installer.

Expected outcome: `python` and `python3` in ~/.local/bin/ if none already exist (potentially replacing existing ones when installing a newer version), Note: `~/.local/bin` already is in PATH prior to `WindowsApps`.

System info (bash on Windows):
```bash
$ which python
/c/Users/User/AppData/Local/Microsoft/WindowsApps/python

$ which python3
/c/Users/User/AppData/Local/Microsoft/WindowsApps/python3

$ which python3.13
/c/Users/User/.local/bin/python3.13

$ ls ~/.local/bin/python*
/c/Users/User/.local/bin/python3.13.exe

$ echo $PATH
/mingw64/bin:/usr/bin:/c/Users/User/bin:/c/Windows/system32:/c/Windows:/c/Windows/System32/Wbem:/c/Windows/System32/WindowsPowerShell/v1.0:/c/Windows/System32/OpenSSH:/c/Windows/system32/config/systemprofile/AppData/Local/Microsoft/WindowsApps:/c/windows/system32/HWAudioDriver:/c/Users/User/.local/bin:/c/Users/User/scoop/shims:/c/Users/User/AppData/Local/Microsoft/WindowsApps:/c/Users/User/AppData/Local/Programs/Microsoft VS Code/bin
```

Note: if actually installed via Microsoft Store, `python` and `python3` in `WindowsApps` will actually run it. Potentially we could detect whether those are actual binaries or Store links, and only override when Python is not installed via Store. We can detect that based on where the symlink points to.

Without Microsoft Store install:
```bash
$ ls -la /c/Users/User/AppData/Local/Microsoft/WindowsApps/python3
lrwxrwxrwx 1 User 197121 121 Aug 29 09:19 /c/Users/User/AppData/Local/Microsoft/WindowsApps/python3 -> '/c/Program Files/WindowsApps/Microsoft.DesktopAppInstaller_1.26.430.0_x64__8wekyb3d8bbwe/AppInstallerPythonRedirector.exe'
```

With Microsoft Store install:
```
$ ls -la /c/Users/User/AppData/Local/Microsoft/WindowsApps/python3
lrwxrwxrwx 1 User 197121 111 Aug 29 09:15 /c/Users/User/AppData/Local/Microsoft/WindowsApps/python3 -> '/c/Program Files/WindowsApps/PythonSoftwareFoundation.Python.3.13_3.13.2032.0_x64__qbz5n2kfra8p0/python3.13.exe'
```


### Example

_No response_

---

_Label `enhancement` added by @Tronic on 2025-08-29 15:28_

---

_Comment by @zanieb on 2025-08-29 15:36_

Sounds like you want `uv python install --default`?

---

_Comment by @zanieb on 2025-08-29 15:36_

See https://docs.astral.sh/uv/concepts/python-versions/#installing-python-executables

---

_Comment by @Tronic on 2025-08-29 19:43_

> Sounds like you want `uv python install --default`?

Yes, that installed the required symlinks. On one of my machines (but not the other) PATH was still in incorrect order and needed to be fixed manually, but I suppose this is outside the scope of uv.


---

_Closed by @Tronic on 2025-08-29 19:43_

---

_Comment by @zanieb on 2025-08-29 19:47_

Yeah we can't fix your PATH unfortunately :)

---
