```yaml
number: 16949
title: "Broken `uv tool list` after updating to 0.9.15"
type: issue
state: open
author: powercoconola
labels:
  - bug
assignees: []
created_at: 2025-12-03T02:28:06Z
updated_at: 2025-12-10T15:23:30Z
url: https://github.com/astral-sh/uv/issues/16949
synced_at: 2026-01-12T16:02:41Z
```

# Broken `uv tool list` after updating to 0.9.15

---

_@powercoconola_

### Summary

When I have 0.9.14 installed, all packages work fine:

```
$ uv tool list
black v25.9.0
- black
- blackd
```

After upgrading to 0.9.15, some are broken:

```
warning: Querying Python at `C:\Users\username\uv\black\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'        
 (run `uv tool install black --reinstall` to reinstall)
```

Doesn't happen with all packages, just some. I tested 0.9.13 and that also works fine.

### Platform

Windows 10

### Version

0.9.15

### Python version

3.12.11

---

_Label `bug` added by @powercoconola on 2025-12-03 02:28_

---

_Comment by @powercoconola on 2025-12-03 02:31_

Tried to get a verbose output:

```
$ uv tool run black -vv
error: Querying Python at `C:\Users\username\uv\black\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'
```

---

_Comment by @charliermarsh on 2025-12-03 02:32_

Thanks for filing, we'll take a look.

---

_Comment by @powercoconola on 2025-12-03 02:34_

Ran on 0.9.15 and 0.9.14, same output:

```
$ uv python list --only-installed
cpython-3.14.0-windows-x86_64-none     C:\Users\username\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none\python.exe
cpython-3.13.7-windows-x86_64-none     C:\Users\username\AppData\Roaming\uv\python\cpython-3.13.7-windows-x86_64-none\python.exe
cpython-3.12.12-windows-x86_64-none    C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.12-windows-x86_64-none\python.exe
cpython-3.11.13-windows-x86_64-none    C:\Users\username\AppData\Roaming\uv\python\cpython-3.11.13-windows-x86_64-none\python.exe
cpython-3.10.18-windows-x86_64-none    C:\Users\username\AppData\Roaming\uv\python\cpython-3.10.18-windows-x86_64-none\python.exe
```

Not sure why it is looking for 3.12.11 when I have 3.12.12 installed it looks like, interesting.

---

_Comment by @powercoconola on 2025-12-08 21:07_

Still broken after updating to 0.9.16

```
> powershell -NoProfile -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/0.9.16/install.ps1 | iex"
Downloading uv 0.9.16 (x86_64-pc-windows-msvc)
Installing to C:\Users\username\.local\bin
  uv.exe
  uvx.exe
  uvw.exe
everything's installed!

> uv tool list
warning: Querying Python at `C:\Users\username\uv\black\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'
 (run `uv tool install black --reinstall` to reinstall)

> powershell -NoProfile -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/0.9.14/install.ps1 | iex"
Downloading uv 0.9.14 (x86_64-pc-windows-msvc)
Installing to C:\Users\username\.local\bin
  uv.exe
  uvx.exe
  uvw.exe
everything's installed!

> uv tool list
black v25.9.0
- black
- blackd
```

---

_Comment by @zanieb on 2025-12-08 22:45_

@samypr100 I feel like https://github.com/astral-sh/uv/pull/16894 is the only change on Windows that could be relevant?

---

_Comment by @samypr100 on 2025-12-08 22:59_

I'll take a look

---

_Comment by @zanieb on 2025-12-08 23:01_

There's just not much in the diff. It's a pretty weird regression though.

@powercoconola Does running the `black` that you've installed with uv work? 

---

_Comment by @powercoconola on 2025-12-08 23:04_

> There's just not much in the diff. It's a pretty weird regression though.
> 
> [@powercoconola](https://github.com/powercoconola) Does running the `black` that you've installed with uv work?

```
> uv self version
uv 0.9.16 (a63e5b62e 2025-12-06)

> uv tool run black -vvv
error: Querying Python at `C:\Users\username\uv\black\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'

> black
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'
```

---

_Comment by @powercoconola on 2025-12-08 23:06_

Also the suggested fix doesn't fix the problem, just noticed...

```
> uv tool install black --reinstall
error: Querying Python at `C:\Users\username\uv\black\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'
```

---

_Comment by @powercoconola on 2025-12-08 23:11_

Ran the suggested command with verbose output

```
> uv tool install black --reinstall -vvv
DEBUG uv 0.9.16 (a63e5b62e 2025-12-06)
TRACE Checking lock for `C:\Users\username\AppData\Local\uv\cache` at `AppData\Local\uv\cache\.lock`
DEBUG Acquired shared lock for `C:\Users\username\AppData\Local\uv\cache`
DEBUG Using request timeout of 30s
DEBUG Searching for Python 3.12 in managed installations
DEBUG Searching for managed installations at `AppData\Roaming\uv\python`
DEBUG Skipping managed installation `cpython-3.14.0-windows-x86_64-none`: does not satisfy `3.12`
DEBUG Skipping managed installation `cpython-3.13.7-windows-x86_64-none`: does not satisfy `3.12`
DEBUG Found managed installation `cpython-3.12.12-windows-x86_64-none`
TRACE Querying interpreter executable at C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.12-windows-x86_64-none\python.exe
DEBUG Found `cpython-3.12.12-windows-x86_64-none` at `C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.12-windows-x86_64-none\python.exe` (managed installations)
TRACE Checking lock for `uv` at `uv\.lock`
DEBUG Acquired exclusive lock for `uv`
DEBUG Checking for Python environment at: `uv\black`
TRACE Querying interpreter executable at C:\Users\username\uv\black\Scripts\python.exe
DEBUG Released lock at `C:\Users\username\uv\.lock`
DEBUG Released lock at `C:\Users\username\AppData\Local\uv\cache\.lock`
TRACE Error trace: Querying Python at `C:\Users\username\uv\black\Scripts\python.exe` failed with exit status exit code: 103

\x1b[31m[stderr]\x1b[39m
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'

error: Querying Python at `C:\Users\username\uv\black\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'
```

---

_Comment by @zanieb on 2025-12-08 23:31_

Does running `black` with 0.9.14 work?

---

_Comment by @powercoconola on 2025-12-08 23:34_

> Does running `black` with 0.9.14 work?

Yes. 

```
> uv self version
uv 0.9.16 (a63e5b62e 2025-12-06)

> uv tool run black -vvv
error: Querying Python at `C:\Users\username\uv\black\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'

> powershell -NoProfile -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/0.9.14/install.ps1 | iex"
Downloading uv 0.9.14 (x86_64-pc-windows-msvc)
Installing to C:\Users\username\.local\bin
  uv.exe
  uvx.exe
  uvw.exe
everything's installed!

> uv tool run black -vvv
Downloading black (1.4MiB)
 Downloaded black
Installed 8 packages in 945ms
Usage: black [OPTIONS] SRC ...

One of 'SRC' or 'code' is required.
```

---

_Comment by @powercoconola on 2025-12-08 23:39_

It looks like to reproduce:

Install uv 0.9.14

```
UV_INDEX=<private_index>
UV_NO_PROGRESS=True
UV_PROJECT_ENVIRONMENT=.venv_username
UV_PYTHON=3.12
UV_PYTHON_PREFERENCE=only-managed
UV_TOOL_BIN_DIR=C:\Users\username\.local\bin
UV_TOOL_DIR=C:\Users\username\uv\
```

`uv python install 3.12.11`
`uv tool install black`
`uv python install 3.12.12`
`uv python uninstall 3.12.11`
Install uv 0.9.15
`uv tool run black` Should fail.

Does that example work on your machines?

---

_Comment by @powercoconola on 2025-12-08 23:51_

> It looks like to reproduce:
> 
> Install uv 0.9.14
> 
> ```
> UV_INDEX=<private_index>
> UV_NO_PROGRESS=True
> UV_PROJECT_ENVIRONMENT=.venv_username
> UV_PYTHON=3.12
> UV_PYTHON_PREFERENCE=only-managed
> UV_TOOL_BIN_DIR=C:\Users\username\.local\bin
> UV_TOOL_DIR=C:\Users\username\uv\
> ```
> 
> `uv python install 3.12.11` `uv tool install black` `uv python install 3.12.12` `uv python uninstall 3.12.11` Install uv 0.9.15 `uv tool run black` Should fail.
> 
> Does that example work on your machines?

I tried this again but this makes both 0.9.14 and 0.9.15 fail... So I wasn't able to reproduce the exact situation where < 0.9.15 is successful. Not exactly sure how to reproduce this then

---

_Comment by @samypr100 on 2025-12-09 03:01_

@powercoconola few questions if you have some time 🙏 

What's the contents of your `<tools_dir>\black\pyvenv.cfg`? It might be possible to repro the 103 error by having `home` point to a broken or non-existent python install.

Does it fail if you explicitly set `UV_LINK_MODE=copy` as an environment variable when removing/reinstalling the tool? I can potentially see a possible scenario where hardlink or symlink never worked before and it always was falling back to `copy` (with maybe a failure along the way) but then it started "working" again leading to such scenarios. Which could maybe explain your last comment about failing in < 0.9.15 as well.

In powershell, what's the output of `Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name LongPathsEnabled`?






---

_Comment by @zanieb on 2025-12-09 14:00_

```
> uv tool run black -vvv
Downloading black (1.4MiB)
 Downloaded black
Installed 8 packages in 945ms
Usage: black [OPTIONS] SRC ...

One of 'SRC' or 'code' is required.
```

In this example, we're using an ephemeral environment for black. I wonder if `C:\Users\username\uv\black\Scripts\black.exe` runs?

---

_Comment by @powercoconola on 2025-12-09 15:48_

Upon messing with `black` so much, it seems I "fixed" it and can't reproduce the exact error. However, I still have the error on other packages, so I'll use `ruff` as my example now. I'll try not to "fix" this one. I have numerous other PCs to test this on so I'm not exactly worried I'll run out of examples.

Confirming initial symptoms:

```
> uv self version
uv 0.9.14 (05814f9cd 2025-12-01)

> uv tool list
ruff v0.8.0
- ruff.exe

> uv tool run ruff --version
Downloading ruff (13.9MiB)
 Downloaded ruff
Installed 1 package in 66ms
ruff 0.14.8

> powershell -NoProfile -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/0.9.16/install.ps1 | iex"
Downloading uv 0.9.16 (x86_64-pc-windows-msvc)
Installing to C:\Users\username\.local\bin
  uv.exe
  uvx.exe
  uvw.exe
everything's installed!

> uv tool run ruff --version
error: Querying Python at `C:\Users\username\uv\ruff\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.7-windows-x86_64-none\python.exe'

> uv python list --only-installed
cpython-3.13.6-windows-x86_64-none     AppData\Roaming\uv\python\cpython-3.13.6-windows-x86_64-none\python.exe
cpython-3.12.12-windows-x86_64-none    AppData\Roaming\uv\python\cpython-3.12.12-windows-x86_64-none\python.exe
```

> What's the contents of your <tools_dir>\black\pyvenv.cfg?

```
> cat $env:UV_TOOL_DIR/ruff/pyvenv.cfg
home = C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.7-windows-x86_64-none
implementation = CPython
uv = 0.5.4
version_info = 3.12.7
include-system-site-packages = false
```

> In powershell, what's the output of Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name LongPathsEnabled?

```
> Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name LongPathsEnabled

LongPathsEnabled : 0
PSPath           : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem
PSParentPath     : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control
PSChildName      : FileSystem
PSDrive          : HKLM
PSProvider       : Microsoft.PowerShell.Core\Registry
```

> In this example, we're using an ephemeral environment for black. I wonder if C:\Users\username\uv\black\Scripts\black.exe runs?

This was an odd outcome...

```
> uv self version
uv 0.9.16 (a63e5b62e 2025-12-06)

> C:\Users\username\uv\ruff\Scripts\ruff.exe --version
ruff 0.8.0

> uv tool run ruff --version
error: Querying Python at `C:\Users\username\uv\ruff\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.7-windows-x86_64-none\python.exe'

> powershell -NoProfile -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/0.9.14/install.ps1 | iex"
Downloading uv 0.9.14 (x86_64-pc-windows-msvc)
Installing to C:\Users\username\.local\bin
  uv.exe
  uvx.exe
  uvw.exe
everything's installed!

> C:\Users\username\uv\ruff\Scripts\ruff.exe --version
ruff 0.8.0

> uv tool run ruff --version
ruff 0.14.8
```


Still working to answer your last question

---

_Comment by @samypr100 on 2025-12-09 16:02_

@zanieb Given `LongPathsEnabled` is false, I don't see how the manifest change can have any impact here 😅

---

_Comment by @samypr100 on 2025-12-09 16:03_

At a glance, the pyenv.cfg does seems to be pointing at a python that does not exist.

---

_Comment by @powercoconola on 2025-12-09 16:04_

> Does it fail if you explicitly set UV_LINK_MODE=copy as an environment variable when removing/reinstalling the tool?


```
> uv self version
uv 0.9.16 (a63e5b62e 2025-12-06)

> uv tool install <private-package> --reinstall
error: Querying Python at `C:\Users\username\uv\<private-package>\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'

> $env:UV_LINK_MODE = "copy"

> uv tool install <private-package> --reinstall
error: Querying Python at `C:\Users\username\uv\<private-package>\Scripts\python.exe` failed with exit status exit code: 103

[stderr]
No Python at '"C:\Users\username\AppData\Roaming\uv\python\cpython-3.12.11-windows-x86_64-none\python.exe'

> uv tool uninstall <private-package>
Uninstalled 3 executables: <private-executable>
```

After that, if I run the installation commands that seems to fix the package.

@samypr100 Does this answer your question? Not sure if I captured your intent here

---

_Comment by @samypr100 on 2025-12-10 01:27_

> [@samypr100](https://github.com/samypr100) Does this answer your question? Not sure if I captured your intent here

Yes, thanks! Doesn't seem to be a copy vs some other mode issue at a glance.

Did you run `uv python upgrade` at some point after upgrading to 0.9.15?

Whats the output of `Get-ChildItem $env:APPDATA\uv\data\python` in powershell?

---

_Comment by @powercoconola on 2025-12-10 15:22_

> Did you run `uv python upgrade` at some point after upgrading to 0.9.15?

I did not. 

> Whats the output of `Get-ChildItem $env:APPDATA\uv\data\python` in powershell?

```
> Get-ChildItem $env:APPDATA\uv\data\python
Get-ChildItem: Cannot find path 'C:\Users\username\AppData\Roaming\uv\data\python' because it does not exist.
```



---

_Comment by @powercoconola on 2025-12-10 15:23_

> Whats the output of `Get-ChildItem $env:APPDATA\uv\data\python` in powershell?

```
> Get-ChildItem $env:APPDATA\uv\data\python
Get-ChildItem: Cannot find path 'C:\Users\username\AppData\Roaming\uv\data\python' because it does not exist.

> Get-ChildItem $env:APPDATA\uv\data
Get-ChildItem: Cannot find path 'C:\Users\username\AppData\Roaming\uv\data' because it does not exist.

> Get-ChildItem $env:APPDATA\uv

    Directory: C:\Users\username\AppData\Roaming\uv

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----          11/25/2025  1:41 PM                python


> Get-ChildItem $env:APPDATA\uv\python

    Directory: C:\Users\username\AppData\Roaming\uv\python

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----          11/25/2024  1:19 PM                .cache
d----          11/25/2025  1:41 PM                .temp
d----          11/25/2025  1:41 PM                cpython-3.12.12-windows-x86_64-none
d----           8/14/2025  4:59 PM                cpython-3.13.6-windows-x86_64-none
-a---          11/25/2024  1:19 PM              1 .gitignore
-a---           2/10/2025  8:37 AM              0 .lock
```

---
