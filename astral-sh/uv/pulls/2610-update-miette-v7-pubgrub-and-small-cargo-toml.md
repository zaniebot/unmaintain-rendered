```yaml
number: 2610
title: Update miette v7, pubgrub and small Cargo.toml cleanup
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/miette-7
created_at: 2024-03-22T10:31:06Z
updated_at: 2024-03-22T10:42:50Z
url: https://github.com/astral-sh/uv/pull/2610
synced_at: 2026-01-10T14:49:08Z
```

# Update miette v7, pubgrub and small Cargo.toml cleanup

---

_Pull request opened by @konstin on 2024-03-22 10:31_

I was going through the output of `cargo tree --duplicate -p uv`, not much success except these small cleanups.

---

_Label `internal` added by @konstin on 2024-03-22 10:31_

---

_Renamed from "Update miette to v7 and small Cargo.toml cleanup" to "Update miette v7, pubgrub and small Cargo.toml cleanup" by @konstin on 2024-03-22 10:34_

---

_Review comment by @konstin on `crates/distribution-filename/Cargo.toml`:23 on 2024-03-22 10:37_

rkyv depends on an old version of hashbrowns, but the next release will "probably take months" (https://github.com/rkyv/rkyv/issues/458#issuecomment-1959686122) and main still depends of hashbrown for std unconditionally.

---

_@konstin reviewed on 2024-03-22 10:37_

---

_Merged by @konstin on 2024-03-22 10:42_

---

_Closed by @konstin on 2024-03-22 10:42_

---

_Branch deleted on 2024-03-22 10:42_

---
