```yaml
number: 16092
title: Fix pubgrub grouping in renovate
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/renovate-root-cargo-toml
created_at: 2025-10-01T20:37:51Z
updated_at: 2025-10-02T07:40:26Z
url: https://github.com/astral-sh/uv/pull/16092
synced_at: 2026-01-12T16:12:06Z
```

# Fix pubgrub grouping in renovate

---

_@konstin_

`packageName` was wrong, we need `depName`

---

_Renamed from "Add root `Cargo.toml` to renovate" to "Fix pubgrub grouping in renovate" by @konstin on 2025-10-01 21:03_

---

_@konstin reviewed on 2025-10-01 21:04_

---

_Review comment by @konstin on `.github/renovate.json5`:19 on 2025-10-01 21:04_

This seems to have been overlooked https://github.com/astral-sh/uv/pull/2659

This is an attempt at debugging why the pubgrub/version-ranges grouping rule doesn't work, but it seems reasonable anyway.

---

_Review requested from @zanieb by @konstin on 2025-10-02 07:29_

---

_Label `internal` added by @konstin on 2025-10-02 07:29_

---

_Merged by @konstin on 2025-10-02 07:40_

---

_Closed by @konstin on 2025-10-02 07:40_

---

_Branch deleted on 2025-10-02 07:40_

---
