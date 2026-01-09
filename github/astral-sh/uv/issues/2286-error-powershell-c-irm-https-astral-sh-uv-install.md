---
number: 2286
title: "ERROR: powershell -c \"irm https://astral.sh/uv/install.ps1 | iex\" Error: PowerShell requires an execution policy in [Unrestricted, RemoteSigned, ByPass] to run uv."
type: issue
state: closed
author: 2zzly
labels:
  - external
assignees: []
created_at: 2024-03-07T20:29:37Z
updated_at: 2024-03-08T05:12:16Z
url: https://github.com/astral-sh/uv/issues/2286
synced_at: 2026-01-07T13:12:17-06:00
---

# ERROR: powershell -c "irm https://astral.sh/uv/install.ps1 | iex" Error: PowerShell requires an execution policy in [Unrestricted, RemoteSigned, ByPass] to run uv.

---

_Issue opened by @2zzly on 2024-03-07 20:29_

powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
Error: PowerShell requires an execution policy in [Unrestricted, RemoteSigned, ByPass] to run uv. For example, to set the execution policy to 'RemoteSigned' please run:

    Set-ExecutionPolicy RemoteSigned -scope CurrentUser

---

_Comment by @samypr100 on 2024-03-08 03:12_

It's possible you don't have developer mode set on your windows machine, or in the corporate case there is a Group Policy on your computer to prevent running PowerShell scripts that are not signed. Couple possible options here:
1) Install via `pipx` or friends (See [Getting Started](https://github.com/astral-sh/uv?tab=readme-ov-file#getting-started))
2) Download directly from https://github.com/astral-sh/uv/releases
3) Temporarily adjust your PowerShell policy to allow remote powershell scripts to run without signing.

---

_Comment by @charliermarsh on 2024-03-08 03:32_

Thanks! There's an upstream issue filed with the installer here: https://github.com/axodotdev/cargo-dist/issues/833.

---

_Closed by @charliermarsh on 2024-03-08 03:32_

---

_Label `upstream` added by @zanieb on 2024-03-08 05:12_

---

_Referenced in [astral-sh/uv#6505](../../astral-sh/uv/issues/6505.md) on 2024-08-23 10:01_

---
