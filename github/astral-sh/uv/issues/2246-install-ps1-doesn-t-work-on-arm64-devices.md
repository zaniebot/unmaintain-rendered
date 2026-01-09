---
number: 2246
title: "install.ps1 doesn't work on ARM64 devices"
type: issue
state: closed
author: kbattocchi
labels:
  - bug
  - installer
  - external
assignees: []
created_at: 2024-03-06T17:36:57Z
updated_at: 2024-03-08T21:37:10Z
url: https://github.com/astral-sh/uv/issues/2246
synced_at: 2026-01-07T13:12:17-06:00
---

# install.ps1 doesn't work on ARM64 devices

---

_Issue opened by @kbattocchi on 2024-03-06 17:36_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I'm on a Surface Pro 9 (5G), and running `powershell -c "irm https://astral.sh/uv/install.ps1 | iex"` fails with:

> Downloading uv 0.1.15 (x86_64-pc-windows-msvc)
> Installing to C:\Users\kebatt\.cargo\bin
> &nbsp;&nbsp;uv.exe
> Cannot index into a null array.

The problem seems to be that the `Download` function in the PowerShell script correctly falls back to `x86_64-pc-windows-msvc` but `Invoke-Installer` doesn't have any corresponding logic to do that.  (I'd raise a PR fixing it, but I don't see the install script in the repo)

---

_Comment by @charliermarsh on 2024-03-06 17:39_

Thank you! Do you mind filing an issue or PR on https://github.com/axodotdev/cargo-dist?

---

_Referenced in [axodotdev/cargo-dist#834](../../axodotdev/cargo-dist/issues/834.md) on 2024-03-06 18:21_

---

_Label `installer` added by @charliermarsh on 2024-03-06 20:18_

---

_Comment by @charliermarsh on 2024-03-08 21:01_

I'm gonna close in favor of the issue in `cargo-dist` (though this is not resolved).

---

_Closed by @charliermarsh on 2024-03-08 21:01_

---

_Label `bug` added by @charliermarsh on 2024-03-08 21:01_

---

_Label `upstream` added by @zanieb on 2024-03-08 21:37_

---
