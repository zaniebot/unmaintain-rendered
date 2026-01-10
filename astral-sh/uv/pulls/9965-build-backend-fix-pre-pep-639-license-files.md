```yaml
number: 9965
title: "Build backend: Fix pre-PEP 639 license files"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/fix-license-inclusions
created_at: 2024-12-17T11:04:00Z
updated_at: 2024-12-17T14:52:52Z
url: https://github.com/astral-sh/uv/pull/9965
synced_at: 2026-01-10T12:00:01Z
```

# Build backend: Fix pre-PEP 639 license files

---

_Pull request opened by @konstin on 2024-12-17 11:04_

We were not copying the license file from a pre-PEP 639 declaration to the source distribution.

Fixes #9947

---

_Label `bug` added by @konstin on 2024-12-17 11:04_

---

_Label `preview` added by @zanieb on 2024-12-17 14:15_

---

_Review requested from @BurntSushi by @zanieb on 2024-12-17 14:15_

---

_Merged by @konstin on 2024-12-17 14:20_

---

_Closed by @konstin on 2024-12-17 14:20_

---

_Branch deleted on 2024-12-17 14:20_

---

_@BurntSushi reviewed on 2024-12-17 14:47_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/wheel.rs`:189 on 2024-12-17 14:47_

You could maybe use a peekable iterator above to avoid re-creating it here? But it doesn't look like iterator construction is especially costly here.

---

_@konstin reviewed on 2024-12-17 14:52_

---

_Review comment by @konstin on `crates/uv-build-backend/src/wheel.rs`:189 on 2024-12-17 14:52_

I decided to just go for simplicity here

---
