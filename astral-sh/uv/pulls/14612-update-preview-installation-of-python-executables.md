```yaml
number: 14612
title: Update preview installation of Python executables to be non-fatal
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/install-non-fatal
created_at: 2025-07-14T18:05:00Z
updated_at: 2025-07-15T17:12:37Z
url: https://github.com/astral-sh/uv/pull/14612
synced_at: 2026-01-10T06:53:02Z
```

# Update preview installation of Python executables to be non-fatal

---

_Pull request opened by @zanieb on 2025-07-14 18:05_

Previously, if installation of executables into the bin directory failed we'd with a non-zero code. However, if we make this behavior the default we don't want it to be fatal. There's a `--bin` opt-in to _require_ successful executable installation and a `--no-bin` opt-out to silence the warning / opt-out of installation entirely.

Part of https://github.com/astral-sh/uv/issues/14296 — we need this before we can stabilize the behavior.

In #14614 we do the same for writing entries to the Windows registry.

---

_Label `preview` added by @zanieb on 2025-07-14 18:05_

---

_Comment by @zanieb on 2025-07-14 19:25_

Bikeshed? `--executable` / `--no-executable`? `--install-exe` / `--no-install-exe`?

---

_Marked ready for review by @zanieb on 2025-07-14 19:30_

---

_@zanieb reviewed on 2025-07-14 22:38_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:718 on 2025-07-14 22:38_

Oops, this is correct but per the variable name should be inverted. Fixed in https://github.com/astral-sh/uv/pull/14614

---

_Review requested from @charliermarsh by @zanieb on 2025-07-15 12:43_

---

_Review requested from @konstin by @zanieb on 2025-07-15 12:43_

---

_@zanieb reviewed on 2025-07-15 13:44_

---

_Review comment by @zanieb on `crates/uv-python/src/windows_registry.rs`:151 on 2025-07-15 13:44_

I didn't want to have to expose my `InstallErrorKind` to this crate, so we just defer pushing errors to the caller. 

---

_@charliermarsh reviewed on 2025-07-15 14:02_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:139 on 2025-07-15 14:02_

I would typically derive at least Debug and Copy

---

_@charliermarsh approved on 2025-07-15 15:56_

---

_Merged by @zanieb on 2025-07-15 17:12_

---

_Closed by @zanieb on 2025-07-15 17:12_

---

_Branch deleted on 2025-07-15 17:12_

---
