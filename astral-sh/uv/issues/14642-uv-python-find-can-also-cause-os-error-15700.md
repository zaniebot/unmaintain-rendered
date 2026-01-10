---
number: 14642
title: "`uv python find` can also cause `os error 15700`"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
assignees: []
created_at: 2025-07-15T23:04:37Z
updated_at: 2025-07-15T23:25:46Z
url: https://github.com/astral-sh/uv/issues/14642
synced_at: 2026-01-10T01:25:47Z
---

# `uv python find` can also cause `os error 15700`

---

_Issue opened by @MeGaGiGaGon on 2025-07-15 23:04_

### Summary

See #14637 for more details/backstory, but I found that even with the updated binary from #14636, `uv python find` still runs into the corrupted interpreters. With #14636 merged, this should probably also be updated for consistency.
```powershell
PS ~\Downloads>.\uv-windows-x86_64-e7c0217db3c05d6b4d60025341605d3a24a068e3\uv.exe python find 3.9.0
error: Failed to inspect Python interpreter from Microsoft Store at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe`
  Caused by: Failed to query Python interpreter at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe`
  Caused by: The process has no package identity. (os error 15700)
```
I also tested all the other `uv` and `uvx` commands I could think of, but they all worked with the new binary (they give the `Skipping bad interpreter` debug message and start downloading a fresh version of `cpython-3.9.0-windows-x86_64-none`) (they don't work with the current release, but #14636 looks to have fixed it)

<details>
<summary>Output with -vv</summary>

```
PS ~\Downloads>.\uv-windows-x86_64-e7c0217db3c05d6b4d60025341605d3a24a068e3\uv.exe python find 3.9.0 -vv
DEBUG uv 0.7.21 (e7c0217db 2025-07-15)
DEBUG Using Python request `3.9.0` from explicit request
DEBUG Searching for Python 3.9.0 in virtual environments, managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\GiGaGon\AppData\Roaming\uv\python`
DEBUG Skipping managed installation `cpython-3.14.0b4-windows-x86_64-none`: does not satisfy `3.9.0`
DEBUG Skipping managed installation `cpython-3.14.0a6-windows-x86_64-none`: does not satisfy `3.9.0`
DEBUG Skipping managed installation `cpython-3.13.2-windows-x86_64-none`: does not satisfy `3.9.0`
TRACE Searching PATH for executables: python3.9.0.exe, python3.9.exe, python3.exe, python.exe
TRACE Checking `PATH` directory for interpreters: C:\Program Files\PowerShell\7
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Python313\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Python313\
TRACE Found possible Python executable: C:\Program Files\Python313\python.exe
TRACE Found cached interpreter info for Python 3.13.5, skipping query of: C:\Program Files\Python313\python.exe
DEBUG Found `cpython-3.13.5-windows-x86_64-none` at `C:\Program Files\Python313\python.exe` (first executable in the search path)
DEBUG Skipping interpreter at `C:\Program Files\Python313\python.exe` from first executable in the search path: does not satisfy request `3.9.0`
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Eclipse Adoptium\jdk-11.0.27.6-hotspot\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Eclipse Adoptium\jdk-17.0.15.6-hotspot\bin
TRACE Checking `PATH` directory for interpreters: C:\Python311\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Python311\
TRACE Found possible Python executable: C:\Python311\python.exe
TRACE Found cached interpreter info for Python 3.11.3, skipping query of: C:\Python311\python.exe
DEBUG Found `cpython-3.11.3-windows-x86_64-none` at `C:\Python311\python.exe` (search path)
DEBUG Skipping interpreter at `C:\Python311\python.exe` from search path: does not satisfy request `3.9.0`
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Python39\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Python39\
TRACE Found possible Python executable: C:\Program Files\Python39\python.exe
TRACE Found cached interpreter info for Python 3.9.9, skipping query of: C:\Program Files\Python39\python.exe
DEBUG Found `cpython-3.9.9-windows-x86_64-none` at `C:\Program Files\Python39\python.exe` (search path)
DEBUG Skipping interpreter at `C:\Program Files\Python39\python.exe` from search path: does not satisfy request `3.9.0`
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
TRACE Found cached interpreter info for Python 3.9.9, skipping query of: C:\Program Files\Python39\python.exe
DEBUG Found `cpython-3.9.9-windows-x86_64-none` at `C:\Program Files\Python39\python.exe` (registry)
DEBUG Skipping interpreter at `C:\Program Files\Python39\python.exe` from registry: does not satisfy request `3.9.0`
TRACE Found cached interpreter info for Python 3.9.7, skipping query of: C:\Users\GiGaGon\anaconda3\python.exe
DEBUG Found `cpython-3.9.7-windows-x86_64-none` at `C:\Users\GiGaGon\anaconda3\python.exe` (registry)
DEBUG Skipping interpreter at `C:\Users\GiGaGon\anaconda3\python.exe` from registry: does not satisfy request `3.9.0`
TRACE Querying interpreter executable at C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe
DEBUG Failed to inspect Python interpreter from Microsoft Store at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe`
DEBUG Skipping bad interpreter at C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe from Microsoft Store: The process has no package identity. (os error 15700)
error: Failed to inspect Python interpreter from Microsoft Store at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe`
  Caused by: Failed to query Python interpreter at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe`
  Caused by: The process has no package identity. (os error 15700)
```

</details>

### Platform

Windows 10 Pro 22H2 x64-based processor

### Version

uv 0.7.21 (e7c0217db 2025-07-15)

### Python version

N/A

---

_Label `bug` added by @MeGaGiGaGon on 2025-07-15 23:04_

---

_Comment by @MeGaGiGaGon on 2025-07-15 23:08_

Looking at the verbose output, it looks like `find` tries to skip the bad interpreter, since the debug message is output, but I guess continues on to try and use it?
```
DEBUG Skipping bad interpreter at C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe from Microsoft Store: The process has no package identity. (os error 15700)
error: Failed to inspect Python interpreter from Microsoft Store at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe`
  Caused by: Failed to query Python interpreter at `C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe`
  Caused by: The process has no package identity. (os error 15700)
```

---

_Comment by @zanieb on 2025-07-15 23:08_

I think this is expected. If that's the only interpreter we can find, we'll still throw that error.

---

_Comment by @zanieb on 2025-07-15 23:09_

per https://github.com/astral-sh/uv/pull/10716

---

_Comment by @MeGaGiGaGon on 2025-07-15 23:12_

Ah that makes sense. I wonder if it would be better to instead give an output like
```
error: No interpreter found for Python 3.9.0 in virtual environments, managed installations, search path, or registry
note: Some interpreters were skipped with non-critical errors, run again with -v for details
```

---

_Comment by @MeGaGiGaGon on 2025-07-15 23:16_

To expand on my rational: Everything else behaves like no interpreters were found, since they download a fresh copy. That's why I opened this issue, since it's confusing for all other signs to point to there being no valid interpreters, but for `uv python find` to instead say there are corrupted valid interpreters.

---

_Comment by @MeGaGiGaGon on 2025-07-15 23:25_

Another idea to prevent race conditions from being an issue (where the log wouldn't show up on re-run), it could do something like
```
error: No interpreter found for Python 3.9.0 in virtual environments, managed installations, search path, or registry
note: Skipped bad interpreter at C:\Users\GiGaGon\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe from Microsoft Store: The process has no package identity. (os error 15700)
```
Where all of the non-critical errors are printed out after as notes.

---
