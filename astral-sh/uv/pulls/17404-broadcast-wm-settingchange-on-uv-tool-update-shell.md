```yaml
number: 17404
title: Broadcast WM_SETTINGCHANGE on uv tool update-shell
type: pull_request
state: merged
author: samypr100
labels:
  - windows
assignees: []
merged: true
base: main
head: wm-settingchange
created_at: 2026-01-11T05:02:22Z
updated_at: 2026-01-11T14:25:02Z
url: https://github.com/astral-sh/uv/pull/17404
synced_at: 2026-01-12T16:12:46Z
```

# Broadcast WM_SETTINGCHANGE on uv tool update-shell

---

_@samypr100_

## Summary

Closes https://github.com/astral-sh/uv/issues/17331

Certain applications on windows expect to be notified when environment variables change such as conhost.exe (traditional cmd.exe host).
Without this notification conhost.exe will not pick up changes to environment variables regardless of how many times conhost.exe is re-launched after running `uv tool update-shell`.

## Test Plan

Before this change

1. Removed `%USERPROFILE%\.local\bin` from environment variables via UI (which sends `WM_SETTINGCHANGE`)
2. Launched `%SYSTEMROOT%\System32\conhost.exe` and attempted to run any tool preivously installed. It fails to find any.
3. Ran `uv tool update-shell`. Confirmed `HKEY_CURRENT_USER\Environment\Path` was updated in registry.
4. Launched new `%SYSTEMROOT%\System32\conhost.exe` session. **Fails to find installed tools**.

After this change

1. Removed `%USERPROFILE%\.local\bin` from environment variables via UI (which sends `WM_SETTINGCHANGE`)
2. Launched `%SYSTEMROOT%\System32\conhost.exe` and attempted to run any tool preivously installed. It fails to find any.
3. Ran `uv tool update-shell`. Confirmed `HKEY_CURRENT_USER\Environment\Path` was updated in registry.
4. Launched new `%SYSTEMROOT%\System32\conhost.exe` session. **Finds the installed tools**.

---

_Label `windows` added by @samypr100 on 2026-01-11 05:02_

---

_@samypr100 reviewed on 2026-01-11 05:03_

---

_Review comment by @samypr100 on `Cargo.toml`:199 on 2026-01-11 05:03_

Sorted and added `Win32_UI_WindowsAndMessaging` explicitly.

---

_Marked ready for review by @samypr100 on 2026-01-11 05:09_

---

_@charliermarsh approved on 2026-01-11 13:58_

---

_Merged by @charliermarsh on 2026-01-11 14:10_

---

_Closed by @charliermarsh on 2026-01-11 14:10_

---

_Branch deleted on 2026-01-11 14:25_

---
