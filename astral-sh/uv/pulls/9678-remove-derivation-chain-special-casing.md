```yaml
number: 9678
title: Remove derivation chain special casing
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/remove-derivation-chain-special-casing
created_at: 2024-12-06T12:34:44Z
updated_at: 2024-12-06T13:05:05Z
url: https://github.com/astral-sh/uv/pull/9678
synced_at: 2026-01-10T12:00:01Z
```

# Remove derivation chain special casing

---

_Pull request opened by @konstin on 2024-12-06 12:34_

Instead of modifying the error to replace a dummy derivation chain from construction with the real one, build the error with the real derivation chain directly.


---

_Label `internal` added by @konstin on 2024-12-06 12:34_

---

_Review comment by @konstin on `crates/uv-requirements/src/lib.rs`:23 on 2024-12-06 12:35_

This derivation chain was always empty.

---

_@konstin reviewed on 2024-12-06 12:35_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/derivation.rs`:26 on 2024-12-06 12:35_

Moved onto `DerivationChain` without functional changes.

---

_@konstin reviewed on 2024-12-06 12:35_

---

_Merged by @konstin on 2024-12-06 13:05_

---

_Closed by @konstin on 2024-12-06 13:05_

---

_Branch deleted on 2024-12-06 13:05_

---
