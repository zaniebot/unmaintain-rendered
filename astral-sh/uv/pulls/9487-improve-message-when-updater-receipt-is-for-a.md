```yaml
number: 9487
title: Improve message when updater receipt is for a different uv executable
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/self-update-receipt
created_at: 2024-11-27T23:59:47Z
updated_at: 2024-12-04T01:26:33Z
url: https://github.com/astral-sh/uv/pull/9487
synced_at: 2026-01-12T16:08:50Z
```

# Improve message when updater receipt is for a different uv executable

---

_@zanieb_

Attempts to improve confusing messaging in cases like https://github.com/astral-sh/uv/issues/6774#issuecomment-2504950681, when the receipt is for a different uv executable. 

```
❯ cargo run --all-features -q -- self update
warning: Self-update is only available for uv binaries installed via the standalone installation scripts.

The current executable is at `/Users/zb/workspace/uv/target/debug/uv` but the standalone installer was used to install uv to `/Users/zb/.cargo`. Are multiple copies of uv installed?
```

Requires https://github.com/axodotdev/axoupdater/pull/221
Closes https://github.com/astral-sh/uv/issues/6774

---

_Label `error messages` added by @zanieb on 2024-11-27 23:59_

---

_Renamed from "zb/self update receipt" to "Improve message when updater receipt is for a different uv executable" by @zanieb on 2024-11-28 00:01_

---

_Review comment by @konstin on `Cargo.toml`:79 on 2024-11-28 11:27_

Github complains about that rev:

> This commit does not belong to any branch on this repository, and may belong to a fork outside of the repository.

r+ with https://github.com/axodotdev/axoupdater/pull/221 landing on main.

---

_@konstin reviewed on 2024-11-28 11:27_

---

_@zanieb reviewed on 2024-11-28 14:00_

---

_Review comment by @zanieb on `Cargo.toml`:79 on 2024-11-28 14:00_

Yeah that's my commit :) I am planning to wait.

---

_Merged by @zanieb on 2024-12-04 01:26_

---

_Closed by @zanieb on 2024-12-04 01:26_

---

_Branch deleted on 2024-12-04 01:26_

---
