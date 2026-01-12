```yaml
number: 5483
title: Redact packse version in snapshots
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/redact-packse-version
created_at: 2024-07-26T14:27:18Z
updated_at: 2024-07-29T15:04:48Z
url: https://github.com/astral-sh/uv/pull/5483
synced_at: 2026-01-12T16:06:50Z
```

# Redact packse version in snapshots

---

_@konstin_

Every packse version update is currently causing a huge diff (the size of the `lock_scenarios.rs` diff in this PR). By redacting the version from the snapshots, we will only have the actual change in the diff and not the redundant version change noise.

The second commit moves all remaining packse url arg values to `common/mod.rs`, which acts as a single source of truth for the packse version.

---

_Label `internal` added by @konstin on 2024-07-26 14:27_

---

_Review requested from @zanieb by @konstin on 2024-07-26 14:27_

---

_Marked ready for review by @konstin on 2024-07-26 14:27_

---

_Renamed from "Konsti/redact packse version" to "Redact packse version in snapshots" by @konstin on 2024-07-26 14:30_

---

_@BurntSushi approved on 2024-07-26 14:31_

Love it

---

_Comment by @charliermarsh on 2024-07-29 14:47_

@konstin -- Should we get this merged to reduce conflicts?

---

_Merged by @konstin on 2024-07-29 15:04_

---

_Closed by @konstin on 2024-07-29 15:04_

---

_Branch deleted on 2024-07-29 15:04_

---
