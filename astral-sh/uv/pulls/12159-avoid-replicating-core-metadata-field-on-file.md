```yaml
number: 12159
title: "Avoid replicating core-metadata field on `File` struct"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/de
created_at: 2025-03-14T00:24:05Z
updated_at: 2025-03-14T14:03:11Z
url: https://github.com/astral-sh/uv/pull/12159
synced_at: 2026-01-10T11:10:39Z
```

# Avoid replicating core-metadata field on `File` struct

---

_Pull request opened by @charliermarsh on 2025-03-14 00:24_

## Summary

A long-standing TODO: we don't need to store three copies of this just to ensure that Serde considers all three fields.


---

_Renamed from "Avoid replicating core-metadata field on File struct" to "Avoid replicating core-metadata field on `File` struct" by @charliermarsh on 2025-03-14 00:24_

---

_Review requested from @konstin by @charliermarsh on 2025-03-14 00:24_

---

_Label `performance` added by @charliermarsh on 2025-03-14 00:24_

---

_@konstin approved on 2025-03-14 10:24_

---

_Merged by @charliermarsh on 2025-03-14 14:03_

---

_Closed by @charliermarsh on 2025-03-14 14:03_

---

_Branch deleted on 2025-03-14 14:03_

---
