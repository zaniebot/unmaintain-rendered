```yaml
number: 2272
title: Fix uv-fs extras by removing dead code
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-pe508-tests
created_at: 2024-03-07T12:09:55Z
updated_at: 2024-03-07T12:16:31Z
url: https://github.com/astral-sh/uv/pull/2272
synced_at: 2026-01-12T16:04:57Z
```

# Fix uv-fs extras by removing dead code

---

_@konstin_

Running the pep508_rs tests was failing due to uv-fs depending on `fs_err::tokio` even when not selected. But the function that used it is unused anyway, so i removed it.

---

_Label `internal` added by @konstin on 2024-03-07 12:09_

---

_Merged by @konstin on 2024-03-07 12:16_

---

_Closed by @konstin on 2024-03-07 12:16_

---

_Branch deleted on 2024-03-07 12:16_

---
