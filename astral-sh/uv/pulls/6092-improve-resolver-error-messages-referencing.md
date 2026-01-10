```yaml
number: 6092
title: Improve resolver error messages referencing workspace members
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
  - preview
assignees: []
merged: true
base: main
head: zb/resolver-errors-workspaces
created_at: 2024-08-14T19:47:35Z
updated_at: 2024-08-15T13:20:12Z
url: https://github.com/astral-sh/uv/pull/6092
synced_at: 2026-01-10T13:09:50Z
```

# Improve resolver error messages referencing workspace members

---

_Pull request opened by @zanieb on 2024-08-14 19:47_

An extension of #6090 that replaces #6066.

In brief, 

1. Workspace member names are passed to the resolver for no solution errors
2. There is a new derivation tree pre-processing step that trims `NoVersion` incompatibilities for workspace members from the derivation tree. This avoids showing redundant clauses like `Because only bird==0.1.0 is available and bird==0.1.0 depends on anyio==4.3.0, we can conclude that all versions of bird depend on anyio==4.3.0.`. As a minor note, we use a custom incompatibility kind to mark these incompatibilities at resolution-time instead of afterwards. 
3. Root dependencies on workspace members say `your workspace requires bird` rather than `you require bird`
4. Workspace member package display omits the version, e.g., `bird` instead of `bird==0.1.0`
5. Instead of reporting a workspace member as unusable we note that its requirements cannot be solved, e.g., `bird's requirements are unsatisfiable` instead of `bird cannot be used`.
6. Instead of saying `your requirements are unsatisfiable` we say `your workspace's requirements are unsatisfiable` when in a workspace, since we're not in a "provide direct requirements" paradigm. 

As an annoying but minor implementation detail, `PackageRange` now requires access to the `PubGrubReportFormatter` so it can determine if it is formatting a workspace member or not. We could probably improve the abstractions in the future.

As a follow-up, we should additional special casing for "single project" workspaces to avoid mention of the workspace concept in simple projects. However, it looks like this will require additional tree manipulations so I'm going to keep it separate.

---

_Label `error messages` added by @zanieb on 2024-08-14 20:11_

---

_Label `preview` added by @zanieb on 2024-08-14 20:11_

---

_Marked ready for review by @zanieb on 2024-08-14 20:19_

---

_Review requested from @charliermarsh by @zanieb on 2024-08-14 21:18_

---

_Review requested from @BurntSushi by @zanieb on 2024-08-14 21:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:390 on 2024-08-14 23:56_

"the" is maybe suspect here, is it better?

---

_@charliermarsh reviewed on 2024-08-14 23:56_

---

_@charliermarsh approved on 2024-08-14 23:56_

Rad.

---

_@charliermarsh reviewed on 2024-08-14 23:57_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:990 on 2024-08-14 23:57_

Yeah that feels a bit off. But at least it's all contained in this file.

---

_@zanieb reviewed on 2024-08-15 00:03_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:390 on 2024-08-15 00:03_

No, sorry I should have called this out. This made it easier to consolidate some logic without changing a _ton_ of snapshots. I'm planning to change it to "your" everywhere in a follow-up.

---

_Merged by @zanieb on 2024-08-15 02:41_

---

_Closed by @zanieb on 2024-08-15 02:41_

---

_Branch deleted on 2024-08-15 02:41_

---

_Review comment by @BurntSushi on `crates/uv/tests/edit.rs`:2582 on 2024-08-15 13:19_

This looks much nicer.

---

_@BurntSushi reviewed on 2024-08-15 13:20_

Nice!

---
