---
number: 11470
title: "`choco install uv` fails for non-licensed users"
type: issue
state: closed
author: jakewilliami
labels:
  - question
  - releases
assignees: []
created_at: 2025-02-13T03:47:45Z
updated_at: 2025-02-13T04:48:39Z
url: https://github.com/astral-sh/uv/issues/11470
synced_at: 2026-01-10T01:25:06Z
---

# `choco install uv` fails for non-licensed users

---

_Issue opened by @jakewilliami on 2025-02-13 03:47_

### Summary

Title: `choco install uv` fails for non-licensed users:
```
PS C:\Windows\System32> choco install uv --version 0.5.30
Chocolatey v2.4.2
Installing the following packages:
uv
By installing, you accept licenses for the packages.
Downloading package from source 'https://community.chocolatey.org/api/v2/'
Progress: Downloading uv 0.5.30... 100%

uv v0.5.30 [Approved]
uv package files install completed. Performing other installation steps.
The package uv wants to run 'chocolateyInstall.ps1'.
Note: If you don't run this script, the installation will fail.
Note: To confirm automatically next time, use '-y' or consider:
choco feature enable -n allowGlobalConfirmation
Do you want to run the script?([Y]es/[A]ll - yes to all/[N]o/[P]rint): A

Attempt to get headers for https://github.com/astral-sh/uv/releases/download/0.5.30/uv-x86_64-pc-windows-msvc.zip failed.
  The remote file either doesn't exist, is unauthorized, or is forbidden for url 'https://github.com/astral-sh/uv/releases/download/0.5.30/uv-x86_64-pc-windows-msvc.zip'. Exception calling "GetResponse" with "0" argument(s): "The underlying connection was closed: An unexpected error occurred on a send."
Downloading uv 64 bit
  from 'https://github.com/astral-sh/uv/releases/download/0.5.30/uv-x86_64-pc-windows-msvc.zip'
ERROR: The remote file either doesn't exist, is unauthorized, or is forbidden for url 'https://github.com/astral-sh/uv/releases/download/0.5.30/uv-x86_64-pc-windows-msvc.zip'. Exception calling "GetResponse" with "0" argument(s): "The underlying connection was closed: An unexpected error occurred on a send."
This package is likely not broken for licensed users - see https://docs.chocolatey.org/en-us/features/private-cdn.
The install of uv was NOT successful.
Error while running 'C:\ProgramData\chocolatey\lib\uv\tools\chocolateyInstall.ps1'.
See log for details.

Chocolatey installed 0/1 packages. 1 packages failed.
See the log for details (C:\ProgramData\chocolatey\logs\chocolatey.log).

Failures
- uv (exited 404) - Error while running 'C:\ProgramData\chocolatey\lib\uv\tools\chocolateyInstall.ps1'.
```

I don't think it's my network because I've been able to install other packages (e.g., `git`) via Chocolatey.

### Platform

Windows 10

### Version

uv 0.5.30

### Python version

Python 3.13

---

_Label `bug` added by @jakewilliami on 2025-02-13 03:47_

---

_Comment by @zanieb on 2025-02-13 04:39_

My only guess here is that GitHub is rate limiting you? I'm sorry, but we can't do much to debug `choco` errors.

---

_Label `bug` removed by @zanieb on 2025-02-13 04:39_

---

_Label `question` added by @zanieb on 2025-02-13 04:39_

---

_Label `releases` added by @zanieb on 2025-02-13 04:39_

---

_Comment by @jakewilliami on 2025-02-13 04:48_

@zanieb you're absolutely right.  I tried downloading the release manually and it didn't work.  GitHub is completely inaccessible in my network (for some reason).  The `choco` error didn't make that obvious (to me).  Sorry for the false alarm.

---

_Closed by @jakewilliami on 2025-02-13 04:48_

---
