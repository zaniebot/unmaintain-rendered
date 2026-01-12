```yaml
number: 3324
title: Centralize installed dist satisfies requirement check
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/satisfies2
created_at: 2024-04-30T16:11:17Z
updated_at: 2024-04-30T16:45:06Z
url: https://github.com/astral-sh/uv/pull/3324
synced_at: 2026-01-12T16:05:34Z
```

# Centralize installed dist satisfies requirement check

---

_@konstin_

Another split out from https://github.com/astral-sh/uv/pull/3263. This abstracts the copy&pasted check whether an installed distribution satisfies a requirement used by both plan.rs and site_packages.rs into a shared module. It's less useful here than with the new requirement but helps with reducing https://github.com/astral-sh/uv/pull/3263 diff size.

---

_Label `internal` added by @konstin on 2024-04-30 16:11_

---

_@konstin reviewed on 2024-04-30 16:11_

---

_Review comment by @konstin on `crates/pep508-rs/src/lib.rs`:466 on 2024-04-30 16:11_

This pacifies clippy's needless pass by value

---

_@konstin reviewed on 2024-04-30 16:12_

---

_Review comment by @konstin on `crates/uv-installer/src/satisfies.rs`:23 on 2024-04-30 16:12_

The `Ref` type is due to `entry.requirement.version_or_url()`.

---

_@charliermarsh reviewed on 2024-04-30 16:13_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:466 on 2024-04-30 16:13_

Yeah fine for this to be copy.

---

_@charliermarsh reviewed on 2024-04-30 16:13_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:400 on 2024-04-30 16:13_

I generally prefer `VersionOrUrlRef::from` to make the types clear to the reader. Or does `as_deref` work?

---

_@konstin reviewed on 2024-04-30 16:34_

---

_Review comment by @konstin on `crates/uv-installer/src/site_packages.rs`:400 on 2024-04-30 16:34_

`as_deref` doesn't work in this case since it wants to return a `Option<&T>` but we need an `Option<VersionOrUrlRef>`

---

_@charliermarsh approved on 2024-04-30 16:36_

---

_Merged by @konstin on 2024-04-30 16:45_

---

_Closed by @konstin on 2024-04-30 16:45_

---

_Branch deleted on 2024-04-30 16:45_

---
