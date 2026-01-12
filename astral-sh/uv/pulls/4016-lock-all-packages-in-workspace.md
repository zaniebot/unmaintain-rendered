```yaml
number: 4016
title: Lock all packages in workspace
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/lock-all-packages-in-workspace
created_at: 2024-06-04T17:19:49Z
updated_at: 2024-06-06T19:09:46Z
url: https://github.com/astral-sh/uv/pull/4016
synced_at: 2026-01-12T16:05:59Z
```

# Lock all packages in workspace

---

_@konstin_

When creating a lockfile, lock the combined dependencies for all packages in a workspace. This make the lockfile independent of where you are in the workspace.

Fixes #3983

---

_Label `preview` added by @konstin on 2024-06-04 17:19_

---

_Review requested from @charliermarsh by @konstin on 2024-06-04 17:19_

---

_@charliermarsh reviewed on 2024-06-04 19:49_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/workspace.rs`:111 on 2024-06-04 19:49_

What's the motivation for this change? Why doesn't it return `pyproject_toml`?

---

_Comment by @charliermarsh on 2024-06-05 00:59_

I think we need to find some solution to `Requires-Python` now. What is the `Requires-Python` of the workspace?

---

_@konstin reviewed on 2024-06-05 08:15_

---

_Review comment by @konstin on `crates/uv-distribution/src/workspace.rs`:111 on 2024-06-05 08:15_

An oversight, fixed now

---

_Comment by @konstin on 2024-06-05 08:39_

I can either be the intersection or the union of the python requirement of the constituent packages. We could also force them to identical but that seems to strict.

Let's say A has python >= 3.9 and B python >=3.10.
Union: We require the workspace to be compatible with >= 3.9. This means we can install A alone on 3.9, but requires all dependencies of B to be compatible with 3.9 too (unless you go for marker tricks)
Intersection: We require the workspace to be compatible with >=3.10. This means you can't test with A on 3.9, but B only gets resolved on >=3.10 only.

From that, the union seems slightly better.

---

_Comment by @konstin on 2024-06-05 10:20_

Created https://github.com/astral-sh/uv/pull/4041 that solves this properly

---

_@charliermarsh reviewed on 2024-06-05 16:32_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/workspace.rs`:111 on 2024-06-05 16:32_

But what does it mean? What case is this?

---

_@konstin reviewed on 2024-06-06 10:06_

---

_Review comment by @konstin on `crates/uv-distribution/src/workspace.rs`:111 on 2024-06-06 10:06_

Oh you meant the branch not the line, this is for supporting implicit workspace roots, i've added a comment now.

---

_Comment by @charliermarsh on 2024-06-06 14:30_

I will review and merge today when I'm out of meetings.

---

_@charliermarsh approved on 2024-06-06 19:03_

---

_Merged by @charliermarsh on 2024-06-06 19:09_

---

_Closed by @charliermarsh on 2024-06-06 19:09_

---

_Branch deleted on 2024-06-06 19:09_

---
