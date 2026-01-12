```yaml
number: 14792
title: Introduce a generic type for list operations
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/marker-list
created_at: 2025-07-21T15:48:19Z
updated_at: 2025-07-21T16:21:48Z
url: https://github.com/astral-sh/uv/pull/14792
synced_at: 2026-01-12T16:11:25Z
```

# Introduce a generic type for list operations

---

_@konstin_

We currently have two marker keys that a list, `extras` and `dependency_groups`, both from PEP 751. With the variants PEP, we will add three more. This change is broken out of the wheel variants PR to introduce generic marker list support, plus a change to use `ContainerOperator` in more places.


---

_Review requested from @charliermarsh by @konstin on 2025-07-21 15:48_

---

_Label `internal` added by @konstin on 2025-07-21 15:48_

---

_Marked ready for review by @konstin on 2025-07-21 15:48_

---

_@konstin reviewed on 2025-07-21 15:49_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/parse.rs`:246 on 2025-07-21 15:49_

This is a bit more strict than before, since there is no legacy with those markers.

---

_@konstin reviewed on 2025-07-21 15:49_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/parse.rs`:334 on 2025-07-21 15:49_

Here we'll add additional branches for the variant markers

---

_@konstin reviewed on 2025-07-21 15:50_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/lowering.rs`:168 on 2025-07-21 15:50_

I failed to come up with a better name for this type

---

_@charliermarsh approved on 2025-07-21 15:51_

---

_@konstin reviewed on 2025-07-21 15:53_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/lowering.rs`:173 on 2025-07-21 15:53_

Here we'll add additional branches for the variant markers

---

_Merged by @konstin on 2025-07-21 16:21_

---

_Closed by @konstin on 2025-07-21 16:21_

---

_Branch deleted on 2025-07-21 16:21_

---
