---
number: 11800
title: uv 0.6.3 fails to install python on Windows Server 2016
type: issue
state: open
author: StephenBrown2
labels:
  - bug
  - windows
assignees: []
created_at: 2025-02-26T15:20:33Z
updated_at: 2025-04-13T11:16:41Z
url: https://github.com/astral-sh/uv/issues/11800
synced_at: 2026-01-07T13:12:18-06:00
---

# uv 0.6.3 fails to install python on Windows Server 2016

---

_Issue opened by @StephenBrown2 on 2025-02-26 15:20_

### Summary

Recently, I have seen that uv 0.6.3 has failed to install python on Windows Server 2016 (Datacenter Edition), due to an inability to rename the temp file to the venv. This is not a problem with uv 0.6.2 or 0.6.1 on Server 2016, or on Windows Server 2019 or 2022 with uv 0.6.3.

`uv 0.6.3 (x86_64-pc-windows-msvc)` is installed.

`.python-version` file is set to `<3.12`

`pyproject.toml` has the following relevant setting:
```toml
[tool.uv]
python-preference = "only-managed"
```

Before this is run, `uv sync --frozen --verbose` is also run, but provides no output in the logs.

Running `uv venv --verbose` with uv 0.6.3 on Windows Server 2016:
```
    + CategoryInfo          : NotSpecified: (DEBUG uv 0.6.3 (a0b9f22a2 2025-02-24):String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError
DEBUG Found project root: `C:\projects\<project>`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `C:\projects\<project>\.python-version`
DEBUG Using Python request `<3.12` from version file at `.python-version`
DEBUG Searching for Python <3.12 in managed installations
DEBUG Searching for managed installations at `C:\Users\vagrant\AppData\Roaming\uv\python`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `C:\Users\vagrant\AppData\Roaming\uv\python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
Downloading cpython-3.11.11-windows-x86_64-none (24.3MiB)
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250212/cpython-3.11.11%2B20250212-x86_64-pc-windows-msvc-install_only_stripped.tar.gz to temporary location: C:\Users\vagrant\AppData\Roaming\uv\python\.temp\.tmpnp321E
DEBUG Extracting cpython-3.11.11%2B20250212-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
 Downloaded cpython-3.11.11-windows-x86_64-none
DEBUG Moving C:\Users\vagrant\AppData\Roaming\uv\python\.temp\.tmpnp321E\python to C:\Users\vagrant\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none
DEBUG Released lock at `C:\Users\vagrant\AppData\Roaming\uv\python\.lock`
  Ã— Failed to copy to:
  â”‚ C:\Users\vagrant\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none
  â•°â”€â–¶ failed to rename file from
      C:\Users\vagrant\AppData\Roaming\uv\python\.temp\.tmpnp321E\python to
      C:\Users\vagrant\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none:
      Incorrect function. (os error 1)
```

Information about the filesystem:
```
DriveLetter FileSystemLabel FileSystem DriveType HealthStatus OperationalStatus SizeRemaining     Size
----------- --------------- ---------- --------- ------------ ----------------- -------------     ----
C           Windows 2016    NTFS       Fixed     Healthy      OK                     50.54 GB 63.66 GB
```

uv 0.6.2 on Windows 2016:
```
+ uv run -m pytest test/functional $($args) 2>&1
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (DEBUG uv 0.6.2 (6d3614eec 2025-02-19):String) [], RemoteException
    + FullyQualifiedErrorId : NativeCommandError
DEBUG Found project root: `C:\projects\<project>`
DEBUG No workspace root found, using project root
DEBUG Discovered project `<project>` at: C:\projects\<project>
DEBUG Acquired lock for `C:\projects\<project>`
DEBUG Reading Python requests from version file at `C:\projects\<project>\.python-version`
DEBUG Using Python request `<3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The virtual environment's Python version satisfies `<3.12`
```

### Platform

Windows Server 2016

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

Python 3.11 (.python-version == <3.12)

---

_Label `bug` added by @StephenBrown2 on 2025-02-26 15:20_

---

_Comment by @zanieb on 2025-02-26 15:38_

It looks like no download is occurring on 0.6.2? Are you in the same project?

Does `uv python install 3.12 --reinstall` work on 0.6.2? 

---

_Comment by @zanieb on 2025-02-26 15:40_

Unfortunately that Windows error is very opaque and renaming is a very basic operation.

---

_Comment by @StephenBrown2 on 2025-02-26 15:42_

This is grabbed from CI logs on the same project in a matrix, I'll see if I can grab the output from the `uv python install` command, but I added the `python-preference` config to the description, so uv is always downloading a new python.

```yaml
    strategy:
      matrix:
        targets:
          # - macos
          # - linux
          # - windows-server
          - windows-server-2016
          - windows-server-2019
          - windows-server-2022
        uv-version:
          - '0.6.1'
          - '0.6.2'
          - '0.6.3'
```

---

_Comment by @StephenBrown2 on 2025-02-26 15:58_

Also, the fact that no output is given for `uv sync --frozen --verbose` is frustrating, though I didn't enable `$env:RUST_LOG = "uv=debug"` for that command. I'll add that as well and see if it outputs anything for 0.6.2.

---

_Comment by @StephenBrown2 on 2025-02-26 16:27_

With or without `$env:RUST_LOG = "uv=debug"`, the following commands:
```pwsh
$uvOutput = uv sync --frozen --verbose | Out-String
Write-Host "uv sync --frozen --verbose output:"
Write-Host $uvOutput
Write-Host "-------------------------------------"
$uvPython = uv python install 3.12 --reinstall | Out-String
Write-Host "uv python install 3.12 --reinstall output:"
Write-Host $uvPython
Write-Host "-------------------------------------"
```
Output the following:
```txt
       uv sync --frozen --verbose output:
       
       -------------------------------------
       uv python install 3.12 --reinstall output:
       
       -------------------------------------
```
regardless of uv or windows version.

Windows and PowerShell are not my preferred environment, however, so I may be missing something.

---

_Comment by @StephenBrown2 on 2025-03-07 15:51_

Just an update, this is still an issue with uv 0.6.5, but still only on Windows Server 2016:
```
uv : Downloading cpython-3.11.11-windows-x86_64-none (24.3MiB)
       At C:\projects\jumpcloud-agent\bin\TestFunctional.ps1:7 char:3
       +   uv run -m pytest --last-failed test/functional $($args) 2>&1
       +   ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
           + CategoryInfo          : NotSpecified: (Downloading cpy...-none (24.3MiB):String) [], RemoteException
           + FullyQualifiedErrorId : NativeCommandError
        
        
       Downloaded cpython-3.11.11-windows-x86_64-none
       
       
       error
       : Failed to copy to: C:\Users\vagrant\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none
       
         
       Caused by: failed to rename file from C:\Users\vagrant\AppData\Roaming\uv\python\.temp\.tmpXMPWzf\python to C:\Users\vagrant\AppData\Roaming\uv\python\cpython-3.11.11-windows-x86_64-none: Incorrect function. (os error 1)
```

---

_Comment by @ufwtlsb on 2025-03-27 08:34_

i have same problem

---

_Label `windows` added by @zanieb on 2025-04-01 19:16_

---

_Comment by @ufwtlsb on 2025-04-13 11:16_

uv 0.6.14 is ok

---
