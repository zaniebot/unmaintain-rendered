```yaml
number: 3457
title: " Preserve parsed url in ResolvedDist -> Requirement "
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/parse-unnamed-url
created_at: 2024-05-08T12:53:28Z
updated_at: 2024-05-14T01:47:21Z
url: https://github.com/astral-sh/uv/pull/3457
synced_at: 2026-01-12T16:05:39Z
```

#  Preserve parsed url in ResolvedDist -> Requirement 

---

_@konstin_

Lose less information in the `ResolvedDist` -> `Requirement` conversion.

---

_Label `internal` added by @konstin on 2024-05-08 12:53_

---

_Marked ready for review by @konstin on 2024-05-08 13:41_

---

_@konstin reviewed on 2024-05-08 18:05_

---

_Review comment by @konstin on `crates/uv-installer/src/satisfies.rs`:135 on 2024-05-08 18:05_

We need to compare installed with precise because e.g. the requested reference is the default branch, but we resolved and therefore know that the installed is at the same commit as the default branch is at.

---

_@charliermarsh approved on 2024-05-14 01:37_

---

_Merged by @charliermarsh on 2024-05-14 01:47_

---

_Closed by @charliermarsh on 2024-05-14 01:47_

---

_Branch deleted on 2024-05-14 01:47_

---
