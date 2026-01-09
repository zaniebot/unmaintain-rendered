---
number: 13577
title: "System global .python-version `uv python pin --system`"
type: issue
state: open
author: MattiasDC
labels:
  - enhancement
assignees: []
created_at: 2025-05-21T13:46:27Z
updated_at: 2025-06-17T05:58:57Z
url: https://github.com/astral-sh/uv/issues/13577
synced_at: 2026-01-07T13:12:18-06:00
---

# System global .python-version `uv python pin --system`

---

_Issue opened by @MattiasDC on 2025-05-21 13:46_

### Summary

Currently it's possible for a user to `uv python pin --global` to configure the default python version to use when creating for example new environments. It would create a `.python-version` file in the user configuration directory. It's however not possible for a sysadmin to configure a default for all users on a system. This issue is for the discussion/feature request of a `uv python pin --system` (or any other flag) option that would create a `.python-version` file on the same locations as system configuration files of [uv.toml](https://docs.astral.sh/uv/configuration/files/#configuration-files) would reside

### Example

`uv python pin 3.13.3 --system` creates `/etc/uv/.python-version` with content "3.13.3". This file is interpreted as less important compared with file outputted by `uv python pin 3.13.3 --global`.

---

_Label `enhancement` added by @MattiasDC on 2025-05-21 13:46_

---

_Renamed from "System global .python-version" to "System global .python-version `uv python pin --system`" by @MattiasDC on 2025-05-26 07:38_

---

_Comment by @iwinux on 2025-06-17 05:58_

This would be great if implemented. Currently it's very difficult for Nix users to pin Python versions. I can only rely on `UV_PYTHON` to set global default Python version.

---

_Referenced in [astral-sh/uv#10711](../../astral-sh/uv/issues/10711.md) on 2025-06-17 06:17_

---
