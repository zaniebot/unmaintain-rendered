```yaml
number: 13736
title: "Don't fail direct URL hash checking with dependency metadata"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/hash-checking-direct-url-metadata
created_at: 2025-05-30T17:23:02Z
updated_at: 2025-05-30T17:39:42Z
url: https://github.com/astral-sh/uv/pull/13736
synced_at: 2026-01-12T16:10:50Z
```

# Don't fail direct URL hash checking with dependency metadata

---

_@konstin_

Fixes #12512

---

_Review requested from @charliermarsh by @konstin on 2025-05-30 17:23_

---

_Label `bug` added by @konstin on 2025-05-30 17:23_

---

_@konstin reviewed on 2025-05-30 17:23_

---

_Review comment by @konstin on `crates/uv-distribution/src/distribution_database.rs`:435 on 2025-05-30 17:23_

I don't understand why index deps work but not direct deps, but this does fix it?

---

_@charliermarsh approved on 2025-05-30 17:27_

---

_Merged by @konstin on 2025-05-30 17:39_

---

_Closed by @konstin on 2025-05-30 17:39_

---

_Branch deleted on 2025-05-30 17:39_

---
