```yaml
number: 459
title: Serde support for WheelFilename through str repr
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: wheel-filename-serde
created_at: 2023-11-19T19:36:38Z
updated_at: 2023-11-19T20:02:48Z
url: https://github.com/astral-sh/uv/pull/459
synced_at: 2026-01-12T16:03:58Z
```

# Serde support for WheelFilename through str repr

---

_@konstin_

I need this later, splitting out for PR size

---

_Merged by @konstin on 2023-11-19 19:43_

---

_Closed by @konstin on 2023-11-19 19:43_

---

_Branch deleted on 2023-11-19 19:43_

---

_@charliermarsh reviewed on 2023-11-19 19:43_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/wheel.rs`:1 on 2023-11-19 19:43_

Can we make sure serde is an optional feature?

---

_@charliermarsh reviewed on 2023-11-19 19:43_

---

_Review comment by @charliermarsh on `crates/distribution-filename/Cargo.toml`:17 on 2023-11-19 19:43_

We should try to make serde and clap optional wherever we can.

---

_@konstin reviewed on 2023-11-19 19:47_

---

_Review comment by @konstin on `crates/distribution-filename/Cargo.toml`:17 on 2023-11-19 19:47_

#461 

When would we not want this feature?

---

_@charliermarsh reviewed on 2023-11-19 20:02_

---

_Review comment by @charliermarsh on `crates/distribution-filename/Cargo.toml`:17 on 2023-11-19 20:02_

We always want it, but they're heavy deps and these are fairly general crates, so I'd prefer to make context-specific deps (like clap) optional.

---
