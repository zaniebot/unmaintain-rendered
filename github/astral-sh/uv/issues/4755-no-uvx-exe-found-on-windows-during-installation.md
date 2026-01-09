---
number: 4755
title: no uvx.exe found on windows during installation
type: issue
state: closed
author: njzjz
labels:
  - bug
  - windows
  - releases
assignees: []
created_at: 2024-07-03T03:29:40Z
updated_at: 2024-07-03T04:50:36Z
url: https://github.com/astral-sh/uv/issues/4755
synced_at: 2026-01-07T13:12:17-06:00
---

# no uvx.exe found on windows during installation

---

_Issue opened by @njzjz on 2024-07-03 03:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```
downloading uv 0.2.20 x86_64-pc-windows-gnu
installing to /c/Users/runneradmin/.cargo/bin
  uv.exe
mv: cannot stat '/tmp/tmp.NnFUbsjG4h/uvx.exe': No such file or directory
ERROR: command failed: mv /tmp/tmp.NnFUbsjG4h/uvx.exe /c/Users/runneradmin/.cargo/bin
Error: Process completed with exit code 1.
```

A simple GitHub Action can reproduce it:

```yml
name: Test

on:
  push:

jobs:
  test:
    name: test windows uv
    runs-on: windows-latest
    steps:
    - run: curl -LsSf https://astral.sh/uv/install.sh | sh
      shell: sh
```

When using the powershell instead of shell, it still cannot find `uvx.exe`, but prints `everything's installed!` and does not fail the script (is it expected?).

```
Run powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
  powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
  shell: C:\Program Files\PowerShell\7\pwsh.EXE -command ". '{0}'"
Downloading uv 0.[2](https://github.com/njzjz/gist/actions/runs/9771365816/job/26974013770#step:2:2).20 (x86_64-pc-windows-msvc)
Installing to C:\Users\runneradmin\.cargo\bin
  uv.exe
Copy-Item : Cannot find path 'C:\Users\runneradmin\AppData\Local\Temp\a68bd8a8-a5da-48ff-8d6[3](https://github.com/njzjz/gist/actions/runs/9771365816/job/26974013770#step:2:3)-5151c2b99750\uvx.exe' 
because it does not exist.
At line:244 char:5
+     Copy-Item "$bin_path" -Destination "$dest_dir"
+     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\runner...2b99750\uvx.exe:String) [Copy-Item], ItemNotFoundExce 
   ption
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.CopyItemCommand
 
Remove-Item : Cannot find path 'C:\Users\runneradmin\AppData\Local\Temp\a68bd8a8-a5da-[4](https://github.com/njzjz/gist/actions/runs/9771365816/job/26974013770#step:2:5)8ff-8d63-5151c2b99750\uvx.exe' 
because it does not exist.
At line:24[5](https://github.com/njzjz/gist/actions/runs/9771365816/job/26974013770#step:2:6) char:5
+     Remove-Item "$bin_path" -Recurse -Force
+     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\runner...2b99[7](https://github.com/njzjz/gist/actions/runs/9771365816/job/26974013770#step:2:8)50\uvx.exe:String) [Remove-Item], ItemNotFoundEx 
   ception
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.RemoveItemCommand
 
  uvx.exe
everything's installed!
```

```yml
name: Test

on:
  push:

jobs:
  test:
    name: test windows uv
    runs-on: windows-latest
    steps:
    - run: powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

---

_Comment by @zanieb on 2024-07-03 03:53_

Thanks for the report. I'd recommend pinning to 0.2.18 in the meantime: 

```
powershell -c "irm https://astral.sh/uv/0.2.18/install.ps1 | iex"
```

We'll resolve this ASAP though.

---

_Label `bug` added by @zanieb on 2024-07-03 03:54_

---

_Label `windows` added by @zanieb on 2024-07-03 03:54_

---

_Label `release` added by @zanieb on 2024-07-03 03:54_

---

_Referenced in [astral-sh/uv#4756](../../astral-sh/uv/pulls/4756.md) on 2024-07-03 03:57_

---

_Closed by @zanieb on 2024-07-03 04:16_

---

_Referenced in [axodotdev/cargo-dist#1183](../../axodotdev/cargo-dist/issues/1183.md) on 2024-07-03 04:27_

---

_Comment by @zanieb on 2024-07-03 04:50_

This should be fixed in 0.2.21

---
