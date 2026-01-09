---
number: 9421
title: "Feature request: `uv self install [--user] [--system]`"
type: issue
state: open
author: chrisrodrigue
labels:
  - enhancement
  - cli
assignees: []
created_at: 2024-11-25T15:38:40Z
updated_at: 2025-05-30T15:10:23Z
url: https://github.com/astral-sh/uv/issues/9421
synced_at: 2026-01-07T13:12:18-06:00
---

# Feature request: `uv self install [--user] [--system]`

---

_Issue opened by @chrisrodrigue on 2024-11-25 15:38_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Since the `uv` binary is portable, it might not make sense to build an installer (such as a `.msi`), but it makes sense for the main `uv` binary to have the capability to install itself so that an offline user (or a user who isn't permitted to use the official installation scripts) doesn't need to install it manually.

A command such as `uv self install [--system] [--user]` should make the `uv` executable move/copy itself to a canonical system directory (`C:\Program Files\uv`, `C:\Windows`, or `C:\Windows\System32`) or user directory (`%USERPROFILE%\.local\bin`) and ensure that the install location is on the path.

---

_Comment by @zanieb on 2025-01-07 19:40_

Interesting.

cc @Gankra curious if you have thoughts on this pattern.

---

_Label `enhancement` added by @zanieb on 2025-01-07 19:40_

---

_Label `cli` added by @zanieb on 2025-01-07 19:40_

---

_Comment by @Gankra on 2025-01-08 16:58_

It's an interesting situation where you're prevented from using the installation scripts but do have presumably-admin-permissions to throw things in Program Files (I'd certainly believe there's systems configured like that).

However I could certainly see "tell us a directory and we can move stuff there". That said it would almost certainly need some upstream collaborative work with dist/axoupdater to make sure all the update machinery we use doesn't get confused.

---

_Referenced in [astral-sh/uv#11456](../../astral-sh/uv/issues/11456.md) on 2025-02-12 21:13_

---

_Comment by @Nugine on 2025-05-30 14:18_

One of my servers cannot access GitHub for some reason. We need to install uv offline and use our internal mirror. 

It would be great if the installer can support "offline mode". I don't find how to do it in the docs.

---

_Comment by @chrisrodrigue on 2025-05-30 15:09_

I do wonder why maintaining separate PowerShell and Bash installer scripts is preferred over adding self install machinery to the binary itself. 

I advocate for the latter approach since it couples the installation logic with the binary. It works offline and only depends on curl’ing the script (curl should available on all platforms, even on Windows as a cmdlet). It’s also more secure, since a self-contained binary is more resistant to tampering compared to an installer script which can be modified before execution to execute malicious code.

---
