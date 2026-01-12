```yaml
number: 2618
title: Update minimum rust version (cargo) to 1.76
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/update-rust-version
created_at: 2024-03-22T18:35:51Z
updated_at: 2024-03-22T18:45:17Z
url: https://github.com/astral-sh/uv/pull/2618
synced_at: 2026-01-12T16:05:08Z
```

# Update minimum rust version (cargo) to 1.76

---

_@konstin_

This should address (fix?) #2442, it blocks building with a version that doesn't support return type impl trait.

```
$ cargo +1.74 check
  error: package `pep508_rs v0.4.2 (/home/konsti/projects/uv/crates/pep508-rs)` cannot be built because it requires rustc 1.76 or newer, while the currently active rustc version is 1.74.1
```

While we should encourage our dependencies to set a msrv, if we set our rust-toolchain.toml version as cargo msrv our users on no-wheel no-installer platforms will also be fine (or at least get helpful error messages).

---

_Label `error messages` added by @konstin on 2024-03-22 18:35_

---

_Review requested from @zanieb by @konstin on 2024-03-22 18:35_

---

_@charliermarsh approved on 2024-03-22 18:37_

---

_@zanieb approved on 2024-03-22 18:37_

---

_Merged by @charliermarsh on 2024-03-22 18:45_

---

_Closed by @charliermarsh on 2024-03-22 18:45_

---

_Branch deleted on 2024-03-22 18:45_

---
