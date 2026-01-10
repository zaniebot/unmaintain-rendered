```yaml
number: 849
title: "Remove unused `Result`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/remove-unused-option
created_at: 2024-01-09T16:25:19Z
updated_at: 2024-01-09T16:35:11Z
url: https://github.com/astral-sh/uv/pull/849
synced_at: 2026-01-10T15:44:44Z
```

# Remove unused `Result`

---

_Pull request opened by @konstin on 2024-01-09 16:25_

Remove some dead code, seems to be a refactoring oversight

---

_@charliermarsh reviewed on 2024-01-09 16:28_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/cached.rs`:147 on 2024-01-09 16:28_

These can now use `path.file_name()?`, I think...

---

_@konstin reviewed on 2024-01-09 16:30_

---

_Review comment by @konstin on `crates/distribution-types/src/cached.rs`:147 on 2024-01-09 16:30_

Good catch

---

_Merged by @konstin on 2024-01-09 16:35_

---

_Closed by @konstin on 2024-01-09 16:35_

---

_Branch deleted on 2024-01-09 16:35_

---
