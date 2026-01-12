```yaml
number: 6400
title: "FR: Support installing and managing `uv` with `uv tool`"
type: issue
state: closed
author: weihenglim
labels:
  - wish
assignees: []
created_at: 2024-08-22T02:25:46Z
updated_at: 2024-12-16T04:44:55Z
url: https://github.com/astral-sh/uv/issues/6400
synced_at: 2026-01-12T15:59:04Z
```

# FR: Support installing and managing `uv` with `uv tool`

---

_@weihenglim_

Currently the documentation's [recommended way](https://docs.astral.sh/uv/getting-started/installation/#pypi) of installing `uv` is via `pipx`. With the release of `uv tool`, it would be great to let `uv` manage itself, similar to how you can do `pipx install pipx`.

When testing on my Windows 11 machine, `uv tool install uv` works but the tool breaks when attempting to upgrade `uv` to the latest version:
```sh
> uv tool upgrade uv 
Resolved 1 package in 38ms
Prepared 1 package in 1ms
Installed 1 package in 8ms
 + uv==0.3.1
error: failed to remove file `C:\Users\User\.local\bin\uv.exe`
  Caused by: Access is denied. (os error 5)

> uv tool list
warning: Ignoring malformed tool `uv` (run `uv tool uninstall uv` to remove)
```

---

_Comment by @charliermarsh on 2024-08-22 02:27_

Ahh yeah, that's a known issue on Windows: https://github.com/astral-sh/uv/issues/1368. You _should_ be able to do `uv self update` if you installed via the standalone installers (as opposed to through `pip` or similar).

---

_Comment by @weihenglim on 2024-08-22 02:41_

Ah that's a shame, was hoping it would be possible to just upgrade everything (including `uv` itself) with a single `uv tool upgrade --all` command. Guess I'll stick with the standalone installers for now.

---

_Label `wish` added by @zanieb on 2024-08-22 03:51_

---

_Comment by @zanieb on 2024-08-22 03:51_

I'm not sure it makes sense to manage uv with uv, it creates a weird chicken and egg problem. You can definitely do `uvx uv@version` though!

---

_Comment by @konstin on 2024-08-22 10:13_

`rustup update` updates both its managed toolchains, then does a self update, we should consider a similar convenience function for uv.

---

_Comment by @zanieb on 2024-08-22 14:29_

Hm like `uv upgrade`?

- Upgrade self
- Upgrade all tools
- Upgrade all Python versions

Could be cool.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-09 01:32_

---

_Closed by @charliermarsh on 2024-12-10 18:13_

---

_Comment by @weihenglim on 2024-12-16 02:05_

Hmm, I still can't seem to get `uv tool upgrade uv` to work on Windows using `v0.5.8`

```cmd
>uv version
uv 0.5.8 (80d41671b 2024-12-11)

>uv tool upgrade uv
Updated uv v0.5.8 -> v0.5.9
 - uv==0.5.8
 + uv==0.5.9
error: Failed to upgrade uv
  Caused by: Failed to install entrypoint
  Caused by: failed to copy file from C:\Users\User\AppData\Roaming\uv\data\tools\uv\Scripts\uv.exe to C:\Users\User\.local\bin\uv.exe: The process cannot access the file because it is being used by another process. (os error 32)

>uv tool list
uv v0.5.9
- uv.exe
- uvx.exe

>uv version
uv 0.5.8 (80d41671b 2024-12-11)
```

---
