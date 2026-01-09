---
number: 17344
title: "The latest version 0.9.22 doesn't work on Windows"
type: issue
state: open
author: jetunc
labels:
  - external
assignees: []
created_at: 2026-01-07T09:19:56Z
updated_at: 2026-01-07T11:25:31Z
url: https://github.com/astral-sh/uv/issues/17344
synced_at: 2026-01-07T13:12:19-06:00
---

# The latest version 0.9.22 doesn't work on Windows

---

_Issue opened by @jetunc on 2026-01-07 09:19_

### Summary

I got this error:

```
info: Checking for updates...
error: The installation failed. Output from the installer:
Downloading uv 0.9.22 (x86_64-pc-windows-msvc)
Installing to C:\Users\<user>\.cargo\bin
  uv.exe

We encountered an error trying to perform the installation; please review the error messages below.

Operation did not complete successfully because the file contains a virus or potentially unwanted software.
```

I also tried to install it on another Windows machine and encountered the same issue, so I had to downgrade to version 0.9.21.

### Platform

Windows 11 64 bit

### Version

uv 0.9.22

### Python version

Python 3.11

---

_Label `bug` added by @jetunc on 2026-01-07 09:19_

---

_Label `bug` removed by @konstin on 2026-01-07 09:20_

---

_Label `external` added by @konstin on 2026-01-07 09:20_

---

_Comment by @konstin on 2026-01-07 09:22_

This is a false positive by the antivirus software. The uv binary is fine, the AV software is detection is wrong in blocking the installation and showing that message.

---

_Comment by @EliteTK on 2026-01-07 11:25_

Seems specifically isolated to uvw.exe and uvx.exe?

* [uv-x86_64-pc-windows-msvc.zip](https://www.virustotal.com/gui/file/93a0a244f26eec208d8ea8077e3de743d9687ad76c190342722f1f57fa629457)
* [uv.exe](https://www.virustotal.com/gui/file/4995122c0fe34e8c5542fb59e9b095f70e4a8d1edf591fa0ac2caadd6843c4bb)
* [uvw.exe](https://www.virustotal.com/gui/file/15d15558a8c4ae547b58be4e1aa4e13391878edc7e502ace1bbd44c3bb26e97a)
* [uvx.exe](https://www.virustotal.com/gui/file/7ce068699d691b0b818a390a4c3e3294c4a0e3526a7487d40e635f5d44573a2a)

Also it looks like uvw.exe and uvx.exe have been getting flagged by a couple of others even earlier.

It might be worth submitting the files for malware analysis via https://www.microsoft.com/en-us/wdsi/filesubmission as windows defender complaining about uv is likely to affect almost all windows users of uv...

---
