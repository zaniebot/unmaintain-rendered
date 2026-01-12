```yaml
number: 14887
title: Remove prioritized dist duplication
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/remove-prioritized-dist-redudancy
created_at: 2025-07-25T10:27:01Z
updated_at: 2025-07-25T15:18:26Z
url: https://github.com/astral-sh/uv/pull/14887
synced_at: 2026-01-12T16:11:28Z
```

# Remove prioritized dist duplication

---

_@konstin_

`Candidate` has an optional field `prioritized`, which was mostly redundant with `CandidateDist`. Specifically, it was only `None`, if `CandidateDist` was `Installed`. This commit removes this duplication.


---

_Review requested from @charliermarsh by @konstin on 2025-07-25 10:27_

---

_Label `internal` added by @konstin on 2025-07-25 10:27_

---

_@jtfmumm approved on 2025-07-25 11:31_

---

_@zanieb approved on 2025-07-25 12:55_

---

_Merged by @konstin on 2025-07-25 15:18_

---

_Closed by @konstin on 2025-07-25 15:18_

---

_Branch deleted on 2025-07-25 15:18_

---
