---
number: 16417
title: "Installation fails on Windows: quit after show 'downloading'"
type: issue
state: open
author: lumos0
labels:
  - needs-mre
assignees: []
created_at: 2025-10-23T08:22:41Z
updated_at: 2025-10-23T10:23:57Z
url: https://github.com/astral-sh/uv/issues/16417
synced_at: 2026-01-07T13:12:19-06:00
---

# Installation fails on Windows: quit after show 'downloading'

---

_Issue opened by @lumos0 on 2025-10-23 08:22_

### Question

Hello, I'm encountering an issue when trying to install uv on Windows. When running the official installation command in PowerShell:
`powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`

<img width="990" height="62" alt="Image" src="https://github.com/user-attachments/assets/bff4c014-9a53-48e3-a002-24aa30c2468b" />

The process starts and shows "Downloading uv 0.9.5 (x86_64-pc-windows-msvc)", but then exits abruptly without any error messages or further progress. 

I've already tried the following troubleshooting steps:
- Running PowerShell as Administrator
- Configuring system proxy settings for PowerShell 


Any guidance or fixes would be appreciated. Thanks!


### Platform

Windows11 x86_64

### Version

_No response_

---

_Label `question` added by @lumos0 on 2025-10-23 08:22_

---

_Label `question` removed by @konstin on 2025-10-23 10:23_

---

_Label `needs-mre` added by @konstin on 2025-10-23 10:23_

---

_Comment by @konstin on 2025-10-23 10:23_

That's strange, I haven't seen that happening before. Do you have any security software or other settings that could interfere with the installer?

---
