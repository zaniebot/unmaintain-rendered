```yaml
number: 1811
title: Use more universal windows install instructions
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/update-powershell-instructions
created_at: 2024-02-21T14:34:43Z
updated_at: 2024-02-22T09:25:34Z
url: https://github.com/astral-sh/uv/pull/1811
synced_at: 2026-01-10T14:54:43Z
```

# Use more universal windows install instructions

---

_Pull request opened by @konstin on 2024-02-21 14:34_

Recommend installing uv on windows with

```
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"
```

instead of

```
irm https://astral.sh/uv/install.ps1 | iex
```

to support installing on cmd.exe, the classic non-powershell windows command prompt.

See https://github.com/axodotdev/cargo-dist/issues/458 for background. This will also be included in the next cargo-dist release.

I have confirmed this passes on
 * Command Prompt
 * Windows PowerShell
 * PowerShell
 * git bash

Closes #1750

CC @12932 this fixes the uv command prompt installation.

---

_Label `enhancement` added by @konstin on 2024-02-21 14:34_

---

_@charliermarsh approved on 2024-02-21 14:42_

---

_Comment by @MichaReiser on 2024-02-21 16:11_

Seems reasonable to me. Running it locally made me aware that this calls the Windows Powershell installation (I have profile customizations that aren't compatible with the windows power shell and they fail). Do we know if this works on systems that only have powershell installed but not the system powershell? I believe GitHub Windows uses pws7.exe. Can we try if the command works inside of a runner (sorry, I know it's pain to try these things out)

---

_@zanieb approved on 2024-02-21 16:48_

---

_Label `documentation` added by @zanieb on 2024-02-21 16:48_

---

_Label `enhancement` removed by @zanieb on 2024-02-21 16:48_

---

_Comment by @konstin on 2024-02-22 09:16_

Works on github actions: https://github.com/astral-sh/uv/actions/runs/8002095518/job/21854609649?pr=1811

---

_Merged by @konstin on 2024-02-22 09:25_

---

_Closed by @konstin on 2024-02-22 09:25_

---

_Branch deleted on 2024-02-22 09:25_

---
