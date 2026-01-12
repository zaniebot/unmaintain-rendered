```yaml
number: 10428
title: "Doc and install: avoid security issue on Windows"
type: issue
state: open
author: paugier
labels:
  - releases
assignees: []
created_at: 2025-01-09T14:01:05Z
updated_at: 2025-04-21T20:35:11Z
url: https://github.com/astral-sh/uv/issues/10428
synced_at: 2026-01-12T16:00:13Z
```

# Doc and install: avoid security issue on Windows

---

_@paugier_

In https://docs.astral.sh/uv/getting-started/installation/#installation-methods, the given install command is currently for Windows

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

I was not able to run this because of a security software.

However, I was able to follow the installation instructions "Install via [Scoop](https://scoop.sh/):" in https://pipx.pypa.io/stable/installation/ for which one needs to execute:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
```

Therefore, I guess installation instructions for UV (or the script I don't know) could be improved to avoid such security blockage.


---

_Label `releases` added by @zanieb on 2025-01-09 14:35_

---

_Comment by @zanieb on 2025-01-09 14:35_

What did your security software do? Was there an error?

---

_Comment by @paugier on 2025-01-09 15:41_

The security software closed the terminal and reported an issue to my company 🙂 (I know that because I was told about).

---

_Comment by @zanieb on 2025-01-09 17:15_

Do you know for what reason? This doesn't really sound like something we can fix unless we sign the installers or something (ref #10336)

---

_Comment by @paugier on 2025-01-10 09:06_

I was finally able to install UV with the .ps1 installer with

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
irm https://astral.sh/uv/install.ps1 -OutFile install-uv.ps1
iex .\install-uv.ps1
```

So it might just be that what does not like the security software is only the `| iex` ? However, `| Invoke-Expression` worked with Scoop, so I don't know.

---

_Comment by @paugier on 2025-01-10 09:15_

It might be useful to improve the documentation. About this and also not using short Powershell aliases. I guess very few people know "irm" and "iex" so it is really "copy paste this command that you don't understand". The longer version with few steps and with a local (and checkable) `install-uv.ps1` script might be better.

---

_Comment by @OMFCP on 2025-01-14 13:05_

We have a user try to install this and our AV blocked it as well.  The specific error was "The application powershell.exe attempted to execute fileless content that contains known malware. This content performs highly suspicious process injection behavior." I'm trying to figure out what specifically in there it flagged as known malware, but I don't have anything specific at this point.

---

_Comment by @paugier on 2025-01-14 14:24_

> The specific error was "The application powershell.exe attempted to execute fileless content that contains known malware. 

It seems to indicate that it can be an issue with `| iex` (executing without file on disk).


---

_Comment by @jpipas on 2025-04-21 20:35_

I got this error when trying to run in powershell:  "PowerShell tried to load a malicious resource detected as Heur.BZC.ZFV.Boxter.341.2562F5C6 and was blocked"

---
