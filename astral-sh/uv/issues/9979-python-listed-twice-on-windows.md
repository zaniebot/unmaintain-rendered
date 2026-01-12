```yaml
number: 9979
title: Python listed twice on Windows
type: issue
state: closed
author: eykamp
labels:
  - bug
  - question
assignees: []
created_at: 2024-12-17T18:49:41Z
updated_at: 2025-04-02T19:25:00Z
url: https://github.com/astral-sh/uv/issues/9979
synced_at: 2026-01-12T16:00:03Z
```

# Python listed twice on Windows

---

_@eykamp_

In a Windows 10 CMD shell, I do this:

`uv python list`

and I get a list of Pythons available to me.  Some are listed twice, due to differences in capitalization:

````
cpython-3.10.15-windows-x86_64-none      <download available>
cpython-3.10.7-windows-x86_64-none       c:\users\chris\AppData\local\Programs\python\python310\python.exe
cpython-3.10.7-windows-x86_64-none       C:\Users\Chris\AppData\Local\Programs\Python\Python310\python.exe
cpython-3.9.20-windows-x86_64-none       <download available>
````

On Windows, I think these duplicates should be filtered out, as Windows is not case-sensitive.

---

_Label `bug` added by @zanieb on 2024-12-17 18:52_

---

_Label `good first issue` added by @zanieb on 2024-12-17 18:52_

---

_Comment by @charliermarsh on 2024-12-17 20:25_

@zanieb -- Do we also want to de-duplicate symlinks? (I think we probably don't expect to see symlinks here anyway?)

---

_Comment by @zanieb on 2024-12-17 20:30_

The symlinks are intentionally shown, i.e., if you have `python -> ...` on your `PATH` that's relevant information.

---

_Comment by @charliermarsh on 2024-12-17 20:33_

Ok. I'm not sure how to solve this then. It's not really correct to just do a case-insensitive comparison.

---

_Comment by @zanieb on 2024-12-17 20:37_

We could avoid showing symlinks on Windows specifically?

I guess the question is, how are both of these being found? Can you share output with `RUST_LOG=uv=trace` set?

---

_Label `good first issue` removed by @charliermarsh on 2024-12-22 17:01_

---

_Label `question` added by @charliermarsh on 2024-12-22 17:01_

---

_Comment by @Gankra on 2025-01-15 18:17_

Looks like my system is Ideal for debugging this. I have a heckload of pythons installed on my windows system by ??? being a programmer ??? and I specifically have Python312 and Python310 installed in this way, and yet the bug only happens for the Python312 install and *not* the Python310 one.

Enabling more logging they appear to have the exact same references in the log, and the lowercase python312 *never is mentioned*.  So something is happening outside the view of our logging machinery.

```
TRACE Checking `PATH` directory for interpreters: C:\Python312\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Python312\
TRACE Found possible Python executable: C:\Python312\python.exe
TRACE Cached interpreter info for Python 3.12.1, skipping probing: C:\Python312\python.exe
DEBUG Found `cpython-3.12.1-windows-x86_64-none` at `C:\Python312\python.exe` (search path)
...
TRACE Checking `PATH` directory for interpreters: C:\Python310\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Python310\
TRACE Found possible Python executable: C:\Python310\python.exe
TRACE Cached interpreter info for Python 3.10.7, skipping probing: C:\Python310\python.exe
DEBUG Found `cpython-3.10.7-windows-x86_64-none` at `C:\Python310\python.exe` (search path)
....
TRACE Cached interpreter info for Python 3.12.1, skipping probing: C:\Python312\python.exe
DEBUG Found `cpython-3.12.1-windows-x86_64-none` at `C:\Python312\python.exe` (registry)
....
TRACE Cached interpreter info for Python 3.10.7, skipping probing: C:\Python310\python.exe
DEBUG Found `cpython-3.10.7-windows-x86_64-none` at `C:\Python310\python.exe` (registry)
....
cpython-3.12.1-windows-x86_64-none                 c:\python312\python.exe
cpython-3.12.1-windows-x86_64-none                 C:\Python312\python.exe
....
cpython-3.10.7-windows-x86_64-none                 C:\Python310\python.exe
```

---

_Assigned to @Gankra by @Gankra on 2025-01-15 18:30_

---

_Comment by @Gankra on 2025-01-15 19:54_

I've isolated this down to my *chocolatey* install of python 3.12 which is a shim executable. 

The chocolatey install *was* DEBUG printed but I missed it because when we debug print it we print `C:\ProgramData\chocolatey\bin\python3.12.exe`, and somewhere in the processing of it into an `Interpretter` it becomes `c:\python312\python.exe`.

---

_Comment by @Gankra on 2025-01-15 20:36_

Ok so [the chocolatey shim](https://github.com/chocolatey/shimgen/blob/70ddc44f27a496984c1b333a8534bce07353eef5/shim/ShimProgram.cs#L58) on my machine contains the hardcoded string `c:\python312\python.exe`, which it uses to invoke the binary.

This in turn subtly changes the behaviour of the python interpretter's `sys.path`, which presumably inherits the exact path string the binary was invoked from.

So the good news is we don't need to deal with symlinks/hardlinks at all here, we just need to be case-insensitive when deduplicating things on windows... although I don't know if it's "incorrect" to dedupe the shim install because I see some notes in the code about how shims are *specifically* unsafe to cache because they're, well, shims.

---

_Comment by @karthiknadig on 2025-04-02 01:25_

This is what I see on my machine, the python install from python.org is getting duplicated.
```
(.venv) PS C:\GIT\TPI\2025-03\sync1\myproject> uv python list --only-installed                                
cpython-3.13.2-windows-x86_64-none     C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.13_qbz5n2kfra8p0\python.exe
cpython-3.12.9-windows-x86_64-none     c:\python312\python.exe
cpython-3.12.9-windows-x86_64-none     C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.12_qbz5n2kfra8p0\python.exe
cpython-3.12.9-windows-x86_64-none     C:\Python312\python.exe
cpython-3.11.9-windows-x86_64-none     c:\python311\python.exe
cpython-3.11.9-windows-x86_64-none     C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe
cpython-3.11.9-windows-x86_64-none     C:\Python311\python.exe
cpython-3.10.11-windows-x86_64-none    C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.10_qbz5n2kfra8p0\python.exe
cpython-3.9.13-windows-x86_64-none     C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.9_qbz5n2kfra8p0\python.exe
cpython-3.8.10-windows-x86_64-none     C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.8_qbz5n2kfra8p0\python.exe
cpython-3.7.9-windows-x86_64-none      C:\Program Files\Python37\python.exe
```

Additional note, I ran this from an activated terminal. Should it have found the one on path as well?

---

_Comment by @zanieb on 2025-04-02 01:29_

Which specifically are you referring to as the duplicates there? `c:\python311\python.exe` and `C:\Python311\python.exe`?

---

_Comment by @karthiknadig on 2025-04-02 01:30_

The lowercase vs upper case drive letter variants.

---

_Comment by @zanieb on 2025-04-02 01:35_

Both versions? Can you explain how it was installed? Chocolatey is not used?

---

_Comment by @karthiknadig on 2025-04-02 01:42_

Both were installed with installer from python.org. Only customization is installing to specific location on disk.

---

_Comment by @zanieb on 2025-04-02 01:44_

Can you share verbose logs? e.g. `uv python list 3.11 -vv`?

---

_Comment by @karthiknadig on 2025-04-02 01:47_

```
>uv python list 3.11 -vv
DEBUG uv 0.6.11 (0632e24d1 2025-03-30)
DEBUG Searching for Python 3.11 in managed installations, search path, or registry
DEBUG Searching for managed installations at `AppData\Roaming\uv\data\python`
TRACE Searching PATH for executables: python3.11.exe, python3.exe, python.exe
TRACE Checking `PATH` directory for interpreters: C:\Users\kanadig\AppData\Local\fnm_multishells\7920_1743558271846
TRACE Checking `PATH` directory for interpreters: C:\Program Files (x86)\Common Files\Oracle\Java\java8path
TRACE Checking `PATH` directory for interpreters: C:\Python312\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Python312\
TRACE Found possible Python executable: C:\Python312\python.exe
TRACE Cached interpreter info for Python 3.12.9, skipping probing: C:\Python312\python.exe
DEBUG Found `cpython-3.12.9-windows-x86_64-none` at `C:\Python312\python.exe` (first executable in the search path)
DEBUG Skipping interpreter at `C:\Python312\python.exe` from first executable in the search path: does not satisfy request `3.11`
TRACE Checking `PATH` directory for interpreters: C:\Python311\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Python311\
TRACE Found possible Python executable: C:\Python311\python.exe
TRACE Cached interpreter info for Python 3.11.9, skipping probing: C:\Python311\python.exe
DEBUG Found `cpython-3.11.9-windows-x86_64-none` at `C:\Python311\python.exe` (search path)
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Quarto\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\wbin
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS\system32
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS\System32\Wbem
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS\System32\WindowsPowerShell\v1.0\
TRACE Checking `PATH` directory for interpreters: C:\WINDOWS\System32\OpenSSH\
TRACE Checking `PATH` directory for interpreters: C:\ProgramData\chocolatey\bin
TRACE Found possible Python executable: C:\ProgramData\chocolatey\bin\python3.11.exe
TRACE Querying interpreter executable at C:\ProgramData\chocolatey\bin\python3.11.exe
DEBUG Found `cpython-3.11.9-windows-x86_64-none` at `C:\ProgramData\chocolatey\bin\python3.11.exe` (search path)
TRACE Checking `PATH` directory for interpreters: C:\ProgramData\miniconda3\Scripts
TRACE Checking `PATH` directory for interpreters: C:\Program Files\WinMerge
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Hatch\
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Docker\Docker\resources\bin
TRACE Checking `PATH` directory for interpreters: C:\Program Files\Git\cmd
TRACE Checking `PATH` directory for interpreters: C:\Program Files\dotnet\
TRACE Checking `PATH` directory for interpreters: C:\Users\kanadig\.pixi\bin
TRACE Checking `PATH` directory for interpreters: C:\Users\kanadig\.cargo\bin
TRACE Checking `PATH` directory for interpreters: C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps
TRACE Found possible Python executable: C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\python3.11.exe
TRACE Querying interpreter executable at C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\python3.11.exe
DEBUG Found `cpython-3.11.9-windows-x86_64-none` at `C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\python3.11.exe` (search path)
TRACE Found possible Python executable: C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\python3.exe
TRACE Querying interpreter executable at C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\python3.exe
DEBUG Found `cpython-3.8.10-windows-x86_64-none` at `C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\python3.exe` (search path)
DEBUG Skipping interpreter at `AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.8_qbz5n2kfra8p0\python.exe` from search path: does not satisfy request `3.11`
TRACE Found possible Python executable: C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\python.exe
TRACE Querying interpreter executable at C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\python.exe
DEBUG Found `cpython-3.8.10-windows-x86_64-none` at `C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\python.exe` (search path)
DEBUG Skipping interpreter at `AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.8_qbz5n2kfra8p0\python.exe` from search path: does not satisfy request `3.11`
TRACE Checking `PATH` directory for interpreters: C:\Users\kanadig\AppData\Local\Programs\Microsoft VS Code\bin
TRACE Checking `PATH` directory for interpreters: C:\texlive\2024\bin\windows
TRACE Checking `PATH` directory for interpreters: C:\Users\kanadig\AppData\Local\Microsoft\WinGet\Packages\Schniz.fnm_Microsoft.Winget.Source_8wekyb3d8bbwe
TRACE Checking `PATH` directory for interpreters: C:\Users\kanadig\AppData\Local\Programs\Microsoft VS Code Insiders\bin
TRACE Checking `PATH` directory for interpreters: C:\Users\kanadig\AppData\Local\Programs\nu\bin\
TRACE Cached interpreter info for Python 3.11.9, skipping probing: C:\Python311\python.exe
DEBUG Found `cpython-3.11.9-windows-x86_64-none` at `C:\Python311\python.exe` (registry)
TRACE Querying interpreter executable at C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe
DEBUG Found `cpython-3.11.9-windows-x86_64-none` at `C:\Users\kanadig\AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe` (Microsoft Store)
cpython-3.11.11-windows-x86_64-none    <download available>
cpython-3.11.9-windows-x86_64-none     c:\python311\python.exe
cpython-3.11.9-windows-x86_64-none     AppData\Local\Microsoft\WindowsApps\PythonSoftwareFoundation.Python.3.11_qbz5n2kfra8p0\python.exe
cpython-3.11.9-windows-x86_64-none     C:\Python311\python.exe
```

---

_Comment by @zanieb on 2025-04-02 02:24_

As noted a few comments prior (https://github.com/astral-sh/uv/issues/9979#issuecomment-2593883086), that duplicate looks like it comes from a chocolatey shim which hard-codes a path which affects executable reports 

```
TRACE Found possible Python executable: C:\ProgramData\chocolatey\bin\python3.11.exe
TRACE Querying interpreter executable at C:\ProgramData\chocolatey\bin\python3.11.exe
DEBUG Found `cpython-3.11.9-windows-x86_64-none` at `C:\ProgramData\chocolatey\bin\python3.11.exe` (search path)
```

---

_Closed by @zanieb on 2025-04-02 19:25_

---
