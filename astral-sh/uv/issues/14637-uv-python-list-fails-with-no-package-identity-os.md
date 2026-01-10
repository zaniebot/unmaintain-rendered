---
number: 14637
title: "`uv python list` fails with no package identity (os error 15700)"
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
assignees: []
created_at: 2025-07-15T19:49:58Z
updated_at: 2025-07-15T21:47:36Z
url: https://github.com/astral-sh/uv/issues/14637
synced_at: 2026-01-10T01:25:47Z
---

# `uv python list` fails with no package identity (os error 15700)

---

_Issue opened by @MeGaGiGaGon on 2025-07-15 19:49_

### Summary

When running `uv python list`, it fails with the error `The process has no package identity. (os error 15700)`
Initial output:
```
PS ~>uv python list
error: Failed to inspect Python interpreter from search path at `AppData\Local\Microsoft\WindowsApps\python.exe`
  Caused by: Failed to query Python interpreter at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\python.exe`
  Caused by: The process has no package identity. (os error 15700)
```

After disabling all python execution aliases after asking for help in the astral discord, I now get the same error, but with a different path:
```
PS ~>uv python list
error: Failed to inspect Python interpreter from Microsoft Store at `AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe`
  Caused by: Failed to query Python interpreter at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe`
  Caused by: The process has no package identity. (os error 15700)
```

My memories are hazy on how I got into this situation since it was a few years ago, but I think it went something like this;
- I tried to install python via the windows store, the install had some issues.
- I tried uninstalling it/reinstalling it/installing different versions of it several times.
- Eventually I gave up and installed it via the official python installer, which worked fine.
- Throughout the years I installed several different versions of python through the official installers, and also updated some versions, all working fine.
- Years later, some PSU issues happened, which has made the windows store completely broken/un-launchable on my system.
- Now I'm trying `uv python list` and getting these errors.

I also can't reproduce the initial error anymore, re-enabling the `3.9` and `3.11` aliases gives a file access error:
```
error: Failed to inspect Python interpreter from search path at `AppData\Local\Microsoft\WindowsApps\python3.exe`
  Caused by: Failed to query Python interpreter at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\python3.exe`
  Caused by: The file cannot be accessed by the system. (os error 1920)
```
And the `3.12` aliases give the same new `PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0` message.

It looks like I have three of these `PythonSoftwareFoundation` versions:
```
PS ~\AppData\Local\Microsoft\WindowsApps>tree /f
...
├───PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0
│       idle.exe
│       idle3.11.exe
│       idle3.exe
│       pip.exe
│       pip3.11.exe
│       pip3.exe
│       python.exe
│       python3.11.exe
│       python3.exe
│       pythonw.exe
│       pythonw3.11.exe
│       pythonw3.exe
│
├───PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0
│       idle.exe
│       idle3.12.exe
│       idle3.exe
│       pip.exe
│       pip3.12.exe
│       pip3.exe
│       python.exe
│       python3.12.exe
│       python3.exe
│       pythonw.exe
│       pythonw3.12.exe
│       pythonw3.exe
│
├───PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0
│       idle.exe
│       idle3.9.exe
│       idle3.exe
│       pip.exe
│       pip3.9.exe
│       pip3.exe
│       python.exe
│       python3.9.exe
│       python3.exe
│       pythonw.exe
│       pythonw3.9.exe
│       pythonw3.exe
...
```
There used to be some python related files at the top level of `WindowsApps`, but those were removed after disabling the aliases.

Issue at @zanieb 's request

Edit:

<details>
<summary>Output with `-vvv`:</summary>

```
PS ~>uv python list -vvv
DEBUG uv 0.7.21 (77c771c7f 2025-07-14)
DEBUG Searching for any Python interpreter in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\GiGaGon\AppData\Roaming\uv\python`
DEBUG Found managed installation `cpython-3.14.0a6-windows-x86_64-none`
TRACE Found cached interpreter info for Python 3.14.0a6, skipping query of: C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0a6-windows-x86_64-none\python.exe
DEBUG Found `cpython-3.14.0a6-windows-x86_64-none` at `C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.14.0a6-windows-x86_64-none\python.exe` (managed installations)
DEBUG Found managed installation `cpython-3.13.2-windows-x86_64-none`
TRACE Found cached interpreter info for Python 3.13.2, skipping query of: C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.13.2-windows-x86_64-none\python.exe
DEBUG Found `cpython-3.13.2-windows-x86_64-none` at `C:\Users\GiGaGon\AppData\Roaming\uv\python\cpython-3.13.2-windows-x86_64-none\python.exe` (managed installations)
TRACE Searching PATH for executables: python.exe, python3.exe, cpython.exe, cpython3.exe, pypy.exe, pypy3.exe, graalpy.exe, graalpy3.exe
TRACE Checking `PATH` directory for interpreters: C:\Program Files\PowerShell\7
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Python313\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Python313\
TRACE Found possible Python executable: C:\Program Files\Python313\python.exe
TRACE Found cached interpreter info for Python 3.13.5, skipping query of: C:\Program Files\Python313\python.exe
DEBUG Found `cpython-3.13.5-windows-x86_64-none` at `C:\Program Files\Python313\python.exe` (first executable in the search path)
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Eclipse Adoptium\jdk-11.0.27.6-hotspot\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Eclipse Adoptium\jdk-17.0.15.6-hotspot\bin
TRACE Checking `PATH` directory for interpreters: C:\Python311\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Python311\
TRACE Found possible Python executable: C:\Python311\python.exe
TRACE Found cached interpreter info for Python 3.11.3, skipping query of: C:\Python311\python.exe
DEBUG Found `cpython-3.11.3-windows-x86_64-none` at `C:\Python311\python.exe` (search path)
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Python39\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Python39\
TRACE Found possible Python executable: C:\Program Files\Python39\python.exe
TRACE Found cached interpreter info for Python 3.9.9, skipping query of: C:\Program Files\Python39\python.exe
DEBUG Found `cpython-3.9.9-windows-x86_64-none` at `C:\Program Files\Python39\python.exe` (search path)
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS\system32
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS\System32\Wbem
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS\System32\WindowsPowerShell\v1.0\
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS\System32\OpenSSH\
TRACE Checking `PATH` directory for interpreters: C:\Program Files (x86)\Common Files\Intuit\QBPOSSDKRuntime
TRACE Checking `PATH` directory for interpreters: C:\Program Files\poppler-21.11.0\Library\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files\dotnet\
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Microsoft SQL Server\150\Tools\Binn\
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Microsoft SQL Server\Client SDK\ODBC\170\Tools\Binn\
TRACE Checking `PATH` directory for interpreters: C:\ProgramData\chocolatey\bin
TRACE Found possible Python executable: C:\ProgramData\chocolatey\bin\python3.11.exe
TRACE Querying interpreter executable at C:\ProgramData\chocolatey\bin\python3.11.exe
DEBUG Found `cpython-3.11.3-windows-x86_64-none` at `C:\ProgramData\chocolatey\bin\python3.11.exe` (search path)
TRACE Checking `PATH` directory for interpreters: C:\Program Files\nu\bin\
TRACE Checking `PATH` directory for interpreters: C:\Program Files (x86)\Common Files\Lacerte Shared\LacerteSDK\
TRACE Checking `PATH` directory for interpreters: C:\Program Files\CMake\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files\gs\gs10.03.1\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Git\cmd
TRACE Checking `PATH` directory for interpreters: C:\Program Files\nodejs\
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Microsoft VS Code\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files\PowerShell\7\
TRACE Skipping already seen directory: C:\Program Files\PowerShell\7\
TRACE Checking `PATH` directory for interpreters: C:\Users\GiGaGon\.local\bin
TRACE Checking `PATH` directory for interpreters: C:\Users\GiGaGon\.cargo\bin
TRACE Checking `PATH` directory for interpreters: C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps
TRACE Checking `PATH` directory for interpreters: C:\Users\GiGaGon\AppData\Local\Programs\Microsoft VS Code\bin
TRACE Checking `PATH` directory for interpreters: C:\Users\GiGaGon\AppData\Roaming\npm
TRACE Checking `PATH` directory for interpreters: C:\Program Files\ffmpeg-6.1-essentials_build\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Neovim\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files\JetBrains\RustRover 233.14015.153\bin
TRACE Checking `PATH` directory for interpreters: C:\Users\GiGaGon\AppData\Local\Microsoft\WinGet\Links
TRACE Found cached interpreter info for Python 3.13.5, skipping query of: C:\Program Files\Python313\python.exe
DEBUG Found `cpython-3.13.5-windows-x86_64-none` at `C:\Program Files\Python313\python.exe` (registry)
TRACE Found cached interpreter info for Python 3.13.2, skipping query of: C:\Users\GiGaGon\AppData\Local\Programs\Python\Python313\python.exe
DEBUG Found `cpython-3.13.2-windows-x86_64-none` at `C:\Users\GiGaGon\AppData\Local\Programs\Python\Python313\python.exe` (registry)
TRACE Found cached interpreter info for Python 3.12.0, skipping query of: C:\Users\GiGaGon\AppData\Local\Programs\Python\Python312\python.exe
DEBUG Found `cpython-3.12.0-windows-x86_64-none` at `C:\Users\GiGaGon\AppData\Local\Programs\Python\Python312\python.exe` (registry)
TRACE Found cached interpreter info for Python 3.11.3, skipping query of: C:\Python311\python.exe
DEBUG Found `cpython-3.11.3-windows-x86_64-none` at `C:\Python311\python.exe` (registry)
TRACE Found cached interpreter info for Python 3.10.2, skipping query of: C:\Users\GiGaGon\AppData\Local\Programs\Python\Python310\python.exe
DEBUG Found `cpython-3.10.2-windows-x86_64-none` at `C:\Users\GiGaGon\AppData\Local\Programs\Python\Python310\python.exe` (registry)
TRACE Found cached interpreter info for Python 3.9.9, skipping query of: C:\Program Files\Python39\python.exe
DEBUG Found `cpython-3.9.9-windows-x86_64-none` at `C:\Program Files\Python39\python.exe` (registry)
TRACE Found cached interpreter info for Python 3.9.7, skipping query of: C:\Users\GiGaGon\anaconda3\python.exe
DEBUG Found `cpython-3.9.7-windows-x86_64-none` at `C:\Users\GiGaGon\anaconda3\python.exe` (registry)
TRACE Querying interpreter executable at C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\python.exe
DEBUG Found `cpython-3.12.10-windows-x86_64-none` at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\python.exe` (Microsoft Store)
TRACE Querying interpreter executable at C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe
DEBUG Failed to inspect Python interpreter from Microsoft Store at `PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe`
error: Failed to inspect Python interpreter from Microsoft Store at `PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe`
  Caused by: Failed to query Python interpreter at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe`
  Caused by: The process has no package identity. (os error 15700)
```

</details>

### Platform

Windows 10 Pro 22H2 x64-based processor

### Version

uv 0.7.21 (77c771c7f 2025-07-14)

### Python version

N/A

---

_Label `bug` added by @MeGaGiGaGon on 2025-07-15 19:49_

---

_Referenced in [astral-sh/uv#14636](../../astral-sh/uv/pulls/14636.md) on 2025-07-15 19:51_

---

_Comment by @zanieb on 2025-07-15 19:55_

Thank you!

---

_Closed by @zanieb on 2025-07-15 21:47_

---

_Referenced in [astral-sh/uv#14642](../../astral-sh/uv/issues/14642.md) on 2025-07-15 23:04_

---

_Referenced in [astral-sh/uv#15651](../../astral-sh/uv/issues/15651.md) on 2025-09-03 06:04_

---
