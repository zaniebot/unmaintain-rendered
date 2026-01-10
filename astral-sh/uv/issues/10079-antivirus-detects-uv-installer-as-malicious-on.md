---
number: 10079
title: "Antivirus detects `uv` installer as malicious on Windows"
type: issue
state: closed
author: ShaiAvr
labels: []
assignees: []
created_at: 2024-12-21T14:11:04Z
updated_at: 2024-12-21T16:32:44Z
url: https://github.com/astral-sh/uv/issues/10079
synced_at: 2026-01-10T01:24:49Z
---

# Antivirus detects `uv` installer as malicious on Windows

---

_Issue opened by @ShaiAvr on 2024-12-21 14:11_

I wanted to give a try to `uv` to see if I like it as a project and dependencies manager. I tried installing it on my Windows 11 PC using the command: `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
However, I get an error stating that this script contains malicious content and has been blocked by my antivirus software:

![Image](https://github.com/user-attachments/assets/ad5f17a0-b532-4137-8c30-5e194f41d5d9)
My antivirus (BitDefender) indeed believes this script is malicious:

![Image](https://github.com/user-attachments/assets/813413a9-7585-4efd-aa06-d208242e9303)
Why does this happen? How can I fix this and install `uv` on my Windows system?

---

_Comment by @FishAlchemist on 2024-12-21 14:30_

I remember that the UV team actually couldn't do much about it, so you could try downloading the UV using other tools.

e.g. from PyPi
https://docs.astral.sh/uv/getting-started/installation/#pypi

---

_Comment by @ShaiAvr on 2024-12-21 15:49_

Weird. Other tools like `pdm` and `rustup` also have installer scripts and I never had problems with them.

---

_Comment by @FishAlchemist on 2024-12-21 15:54_

@ShaiAvr Perhaps the downloaded content triggered the virus detection? Since you're using a Windows 11 PC x86-64, this should be the correct link. Could you please scan this file?
https://github.com/astral-sh/uv/releases/download/0.5.11/uv-x86_64-pc-windows-msvc.zip

Edit:
Since PDM installer is essentially a Python script, it's not typically flagged by virus scanners.
As for Rustup, given its widespread use, I assume many antivirus programs have already scanned and added it to their virus databases.


---

_Comment by @zanieb on 2024-12-21 16:32_

Nothing we can do here but it submit it as valid software to the antivirus
 
- #4300
- #9144 

---

_Closed by @zanieb on 2024-12-21 16:32_

---

_Comment by @Stokestack on 2025-12-11 20:00_

For what it's worth, I just got the same thing:

<img width="528" height="271" alt="Image" src="https://github.com/user-attachments/assets/bf81643e-b39d-46fc-8836-9bd05364ec7e" />

---
