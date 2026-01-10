```yaml
number: 3049
title: Enable global configuration files
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/workspace-v
created_at: 2024-04-16T00:21:17Z
updated_at: 2024-04-17T17:59:51Z
url: https://github.com/astral-sh/uv/pull/3049
synced_at: 2026-01-10T14:43:31Z
```

# Enable global configuration files

---

_Pull request opened by @charliermarsh on 2024-04-16 00:21_

## Summary

Enables `uv` to read configuration from (e.g.) `/Users/crmarsh/.config/uv/uv.toml` on macOS and Linux, and `C:\Users\Charlie\AppData\Roaming\uv\uv.toml` on Windows.

This differs slightly from Ruff, which uses the `Application Support` directory on macOS. But I've deviated here. based on the preferences expressed in https://github.com/astral-sh/ruff/issues/10739.


---

_@charliermarsh reviewed on 2024-04-16 00:22_

---

_Review comment by @charliermarsh on `Cargo.toml`:78 on 2024-04-16 00:22_

This is already a dependency because it powers `directories` under-the-hood, but exposes the low-level methods we need to use the custom XDG behavior.

---

_Label `configuration` added by @charliermarsh on 2024-04-16 00:22_

---

_Marked ready for review by @charliermarsh on 2024-04-16 00:22_

---

_@zanieb approved on 2024-04-16 02:30_

---

_@zanieb reviewed on 2024-04-16 02:32_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:18 on 2024-04-16 02:32_

Can we call this "user" instead? It's not the "global" or "system" workspace, it sounds like it should always be scoped to a user e.g. `XDG_CONFIG_HOME` is for user-specific configuration.

---

_Review comment by @konstin on `crates/uv-workspace/src/workspace.rs`:71 on 2024-04-16 09:34_

I'm somewhat hesitant to diverge from the rust standard of using `dirs` here, can we file an upstream issue and link to the github discussion for this?

---

_@konstin approved on 2024-04-16 09:34_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:71 on 2024-04-17 00:44_

You should chime in here: https://github.com/astral-sh/ruff/issues/10739

---

_@charliermarsh reviewed on 2024-04-17 00:44_

---

_Merged by @charliermarsh on 2024-04-17 17:59_

---

_Closed by @charliermarsh on 2024-04-17 17:59_

---

_Branch deleted on 2024-04-17 17:59_

---
