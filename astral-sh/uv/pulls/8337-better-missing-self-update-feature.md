```yaml
number: 8337
title: Better missing self update feature
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - error messages
assignees: []
merged: true
base: main
head: konsti/missing-self-update
created_at: 2024-10-18T16:06:56Z
updated_at: 2025-04-08T09:00:15Z
url: https://github.com/astral-sh/uv/pull/8337
synced_at: 2026-01-12T16:08:16Z
```

# Better missing self update feature

---

_@konstin_

Show the user a proper error message when built without self-update instead of pretending the feature doesn't exist.

```
error: uv was installed through an external package manager, and self-update is not available. Please use your package manager to update uv.
```

Fixes #8318

---

_Label `enhancement` added by @konstin on 2024-10-18 16:06_

---

_@zanieb approved on 2024-10-18 16:31_

---

_Label `error messages` added by @zanieb on 2024-10-18 16:31_

---

_Merged by @konstin on 2024-10-18 17:38_

---

_Closed by @konstin on 2024-10-18 17:38_

---

_Branch deleted on 2024-10-18 17:38_

---

_@SteveLauC reviewed on 2025-04-08 08:28_

---

_Review comment by @SteveLauC on `crates/uv-cli/src/lib.rs`:466 on 2025-04-08 08:28_

Hi there, may I ask why these `#[cfg(feature = "self-update")]` changes were reverted? 

---

_@konstin reviewed on 2025-04-08 08:42_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:466 on 2025-04-08 08:42_

With those guards and `--no-default-features`, uv pretends the subcommands doesn't exist; with this change, it shows an error message hinting the user to upgrade using their package manager.

---

_@SteveLauC reviewed on 2025-04-08 09:00_

---

_Review comment by @SteveLauC on `crates/uv-cli/src/lib.rs`:466 on 2025-04-08 09:00_

Get it, thanks!

---
