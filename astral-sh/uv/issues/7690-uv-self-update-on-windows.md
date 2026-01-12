```yaml
number: 7690
title: uv self update on Windows
type: issue
state: closed
author: m-aherron
labels:
  - windows
  - releases
assignees: []
created_at: 2024-09-25T18:29:58Z
updated_at: 2024-11-04T19:26:22Z
url: https://github.com/astral-sh/uv/issues/7690
synced_at: 2026-01-12T15:59:15Z
```

# uv self update on Windows

---

_@m-aherron_

Windows 10 - Powershell 7.4.5
Going from uv 0.4.15 to uv 0.4.16 using 
`uv self update `

This upgrade has the following environment variables  defined:
```
UV_INSTALL_DIR            C:\Python\uv
UV_PYTHON_INSTALL_DIR     C:\Python\uv\python
UV_CACHE_DIR              C:\Python\uv\cache
UV_TOOL_DIR               C:\Python\uv\tools
UV_TOOL_BIN_DIR           C:\Python\uv\tools\bin
```
The initial install created C:\Python\uv\bin and put uv and uvx there.

I don't have the details about the exact sequence of events (I may have had  install directory defined with the \bin) 
but the uv code detected a superfluous \bin subdirectory and deleted it.

This upgrade deleted uv.exe and created a C:\Python\uv\bin\bin and C:\Python\uv\bin\tmpname directory.
The new executables were installed in those subdirectories but were not moved to C:\Python\uv\bin.
uv, of course, was subsequently not findable by the OS.

Manually moving the executables to the parent directory and deleting the two created 
directories had everything working correctly with the new versions.



---

_Label `releases` added by @zanieb on 2024-09-25 20:48_

---

_Comment by @zanieb on 2024-09-25 20:48_

Thanks for the report! We'll investigate.

---

_Comment by @zanieb on 2024-09-25 20:48_

Do you think you could provide an exact reproduction? i.e. in a Docker container?

---

_Comment by @m-aherron on 2024-09-25 23:21_

It is Windows so Docker is an issue. 
I'll try to reproduce the issue.
When I execute the 
``` 
uv self update
```
I'll document the before and after of the directories.
What flags, switches, logs would be of help?


---

_Comment by @zanieb on 2024-09-25 23:39_

Oh sorry 😮‍💨 haha

Just to cover all our bases, maybe set `INSTALLER_PRINT_VERBOSE=1 RUST_LOG=trace` and pass the `--verbose` flag? 

---

_Label `windows` added by @zanieb on 2024-09-25 23:39_

---

_Comment by @m-aherron on 2024-09-26 01:43_

This test resulted in successful upgrade of uv and uvx. But it did not clean up properly.  Details in the attached files.
Information on the 4.15 install and all that I could provide about the upgrade to 4.16. 
[uv_self_update_416.txt](https://github.com/user-attachments/files/17140876/uv_self_update_416.txt)
[uv415_install.txt](https://github.com/user-attachments/files/17140877/uv415_install.txt)

My hunch is that once the install creates the temp directory adjacent to the final directory and has the new files there it fails some time while renaming the directories and erasing the old files and the renamed original directory.  Depending on when it bails you may or may not have a working system.

Thanks. 

---

_Comment by @mistydemeo on 2024-09-26 16:50_

The temporary directory adjacent to the install directory is a separate bug that we've just fixed in the development branch of the updater library. (https://github.com/axodotdev/cargo-dist/issues/1374) It'll be fixed in the next release. The temp dir actually contains the previous version rather than the newly-installed one.

I'm looking into the issue with the `bin` directory. I haven't been able to reproduce the original issue yet. When I set the environment variables you mention in the original issue, I get an install in `C:\Python\uv\bin`, and the update goes into the same place. I'll try a few variations to see if I can come up with an installation that reproduces your case exactly.

---

_Comment by @m-aherron on 2024-09-29 00:58_

0.4.17 - Minor annoyance at this point: 
UV_INSTALL_DIR was "C:\Python\uv"
Upgrade worked fine from 4.15 to 4.17 
but it left a copy of uv (version 4.15) in C:\Python\uv\.tmpmIGKAu
debug log info attached

[upgrade.log](https://github.com/user-attachments/files/17177102/upgrade.log)


---

_Comment by @m-aherron on 2024-11-04 19:26_

As of the .28 to .29 upgrade this issue seems to be fully resolved - Windows.

---

_Closed by @m-aherron on 2024-11-04 19:26_

---
