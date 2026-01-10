```yaml
number: 7832
title: Reduce file reading boilerplace in tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: konsti/read-lock
created_at: 2024-10-01T10:59:21Z
updated_at: 2024-10-11T10:37:24Z
url: https://github.com/astral-sh/uv/pull/7832
synced_at: 2026-01-10T12:53:57Z
```

# Reduce file reading boilerplace in tests

---

_Pull request opened by @konstin on 2024-10-01 10:59_

Add a convenience function around reading files for snapshotting them from the temporary directory.

---

_@zanieb approved on 2024-10-01 12:10_

Thanks!

---

_Label `internal` added by @zanieb on 2024-10-01 12:10_

---

_Label `testing` added by @zanieb on 2024-10-01 12:10_

---

_Merged by @konstin on 2024-10-01 13:39_

---

_Closed by @konstin on 2024-10-01 13:39_

---

_Branch deleted on 2024-10-01 13:40_

---

_@fasterthanlime reviewed on 2024-10-11 10:37_

---

_Review comment by @fasterthanlime on `scripts/scenarios/templates/lock.mustache`:64 on 2024-10-11 10:37_

this seems wrong, I'm drive-by fixing it in #8093 cc @konstin 

---
