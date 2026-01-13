```yaml
number: 17417
title: uv self update not working for fresh install (also uvw.exe quarantined by Windows Defender)
type: issue
state: open
author: ryan-kipawa
labels:
  - bug
assignees: []
created_at: 2026-01-12T15:21:50Z
updated_at: 2026-01-13T08:43:44Z
url: https://github.com/astral-sh/uv/issues/17417
synced_at: 2026-01-13T09:21:21Z
```

# uv self update not working for fresh install (also uvw.exe quarantined by Windows Defender)

---

_@ryan-kipawa_

### Summary

Hi!

I followed the instructions for uv's standalone installer:

https://docs.astral.sh/uv/getting-started/installation/#standalone-installer

The version I have installed is `uv 0.9.24 (0fda1525e 2026-01-09)` after that step. When I run `uv self update`, I get the following errors:

```
error: Self-update is only available for uv binaries installed via the standalone installation scripts.

If you installed uv with pip, brew, or another package manager, update uv with `pip install --upgrade`, `brew upgrade`, or similar.
``` 

I have previously installed uv, but have uninstalled it for the purpose of testing internal guidelines. We also have fresh PC installs that replicate this issue. 

I am not sure if it's related, but a coworker also had uvw
.exe quarantined by Windows Defender:

<img width="856" height="544" alt="Image" src="https://github.com/user-attachments/assets/ed9f28e4-b28a-45f3-a264-7dc7a2a49c6f" />

### Platform

Windows 11 x86_x64

### Version

uv 0.9.24 (0fda1525e 2026-01-09)

### Python version

_No response_

---

_Label `bug` added by @ryan-kipawa on 2026-01-12 15:21_

---

_Comment by @zanieb on 2026-01-12 15:33_

Are you sure that the `uv` you're calling is the one you just installed? e.g., what is `where uv` showing?

---

_Comment by @zanieb on 2026-01-12 15:34_

Also on the Windows Defender part, they recently changed something that is causing false positives for our new releases. e.g., see https://github.com/astral-sh/uv/issues/17403

---

_Comment by @ryan-kipawa on 2026-01-12 15:36_

I installed 0.9.23 and it works fine. If I then run `uv self update`, I get this in powershell:

```
uv self update
info: Checking for updates...
error: The installation failed. Output from the installer: Downloading uv 0.9.24 (x86_64-pc-windows-msvc)
Installing to C:\Users\ryan\.local\bin
  uv.exe
  uvx.exe
We encountered an error trying to perform the installation;
please review the error messages below.

Operation did not complete successfully because the file contains a virus or potentially unwanted software.
``` 

---

_Comment by @ryan-kipawa on 2026-01-12 15:42_

> Are you sure that the `uv` you're calling is the one you just installed? e.g., what is `where uv` showing?

yep!

---

_Comment by @zanieb on 2026-01-12 15:54_

Can you share `uv self update -vv`?

---

_Comment by @ryan-kipawa on 2026-01-13 08:43_

> Can you share `uv self update -vv`?

The issue with Windows Defender is no longer happening today, so this more or less produces no relevant output.

I suppose the issue could be closed now and/or lumped into #17403 for future consideration.



---
