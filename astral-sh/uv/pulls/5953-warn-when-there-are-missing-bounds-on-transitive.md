```yaml
number: 5953
title: Warn when there are missing bounds on transitive deps in lowest
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/warn-unbound-indirect-deps
created_at: 2024-08-09T08:54:39Z
updated_at: 2024-08-09T17:55:18Z
url: https://github.com/astral-sh/uv/pull/5953
synced_at: 2026-01-10T13:31:54Z
```

# Warn when there are missing bounds on transitive deps in lowest

---

_Pull request opened by @konstin on 2024-08-09 08:54_

Warn when there are missing bounds on transitive dependencies with `--resolution lowest`.

Implemented as a lazy resolution graph check. Dev deps are odd because they are missing the edge from the root that extras have (they are currently orphans in the resolution graph), but this is more complex to solve properly because we can put dev dep information in a `Requirement` so i special cased them here.

Closes #2797
Should help with #1718

---

_Label `enhancement` added by @konstin on 2024-08-09 08:54_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolution/graph.rs`:699 on 2024-08-09 09:29_

Nit:
```suggestion
            if requirement.name != *package_name {
```

---

_@ibraheemdev approved on 2024-08-09 09:31_

---

_@konstin reviewed on 2024-08-09 11:04_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolution/graph.rs`:699 on 2024-08-09 11:04_

Do they operate differently?

---

_@zanieb reviewed on 2024-08-09 13:50_

---

_Review comment by @zanieb on `crates/uv/tests/lock.rs`:5309 on 2024-08-09 13:50_

Perhaps we should say "Consider adding a lower bound with a constraint when using...".

Do we have a version we could use in a hint? Or is that impossible?

---

_Merged by @konstin on 2024-08-09 17:55_

---

_Closed by @konstin on 2024-08-09 17:55_

---

_Branch deleted on 2024-08-09 17:55_

---
