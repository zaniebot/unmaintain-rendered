```yaml
number: 11805
title: Remove the winreg crate
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: konsti/remove-winreg
created_at: 2025-02-26T17:13:26Z
updated_at: 2025-02-26T17:56:17Z
url: https://github.com/astral-sh/uv/pull/11805
synced_at: 2026-01-10T11:10:39Z
```

# Remove the winreg crate

---

_Pull request opened by @konstin on 2025-02-26 17:13_

Currently, we're using both the official `windows-*` with `windows-registry` crates as well as `winreg`, an older, community-maintained crate.

To unify the codebase, we follow the lead of rustup that already performed this migration (https://github.com/rust-lang/rustup/commit/bce3ed67d219a2b754857f9e231287794d8c770d). This is also a prerequisite to unblock the unification of the windows-sys crate versions.

I've manually tested that `uv tool update-shell` works for adding to PATH and correctly detects when PATH was already added.

---

_Label `enhancement` added by @konstin on 2025-02-26 17:13_

---

_Label `windows` added by @konstin on 2025-02-26 17:13_

---

_Review requested from @Gankra by @konstin on 2025-02-26 17:13_

---

_Review requested from @charliermarsh by @konstin on 2025-02-26 17:13_

---

_@konstin reviewed on 2025-02-26 17:14_

---

_Review comment by @konstin on `crates/uv-shell/src/windows.rs`:41 on 2025-02-26 17:14_

Not sure why this `create` and not `open`, I've taken this from rustup

---

_@konstin reviewed on 2025-02-26 17:16_

---

_Review comment by @konstin on `crates/uv-shell/src/windows.rs`:41 on 2025-02-26 17:16_

This might actually be https://github.com/rust-lang/rustup/issues/3779, so this PR also fixes a bug.

---

_@charliermarsh approved on 2025-02-26 17:44_

---

_Merged by @konstin on 2025-02-26 17:56_

---

_Closed by @konstin on 2025-02-26 17:56_

---

_Branch deleted on 2025-02-26 17:56_

---
