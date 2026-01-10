```yaml
number: 1739
title: Backport changes from publish crates
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/crate-export-backport
created_at: 2024-02-20T08:10:22Z
updated_at: 2024-02-20T18:33:28Z
url: https://github.com/astral-sh/uv/pull/1739
synced_at: 2026-01-10T15:33:24Z
```

# Backport changes from publish crates

---

_Pull request opened by @konstin on 2024-02-20 08:10_

Backport of changes for the published new versions of pep440_rs and pep508_rs to make it easier to keep them in sync.


---

_Label `internal` added by @konstin on 2024-02-20 08:10_

---

_@charliermarsh approved on 2024-02-20 14:51_

---

_@charliermarsh reviewed on 2024-02-20 14:52_

---

_Review comment by @charliermarsh on `Cargo.toml`:72 on 2024-02-20 14:52_

Can you double-check that this is using our local versions of the PEP crates, patched below? I think it is, since you bumped the version, but please double-check.

---

_@konstin reviewed on 2024-02-20 18:33_

---

_Review comment by @konstin on `Cargo.toml`:72 on 2024-02-20 18:33_

Good point, i checked and we only have one version of both in Cargo.lock.

---

_Merged by @konstin on 2024-02-20 18:33_

---

_Closed by @konstin on 2024-02-20 18:33_

---

_Branch deleted on 2024-02-20 18:33_

---
