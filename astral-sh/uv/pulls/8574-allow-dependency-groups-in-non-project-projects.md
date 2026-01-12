```yaml
number: 8574
title: "Allow `[dependency-groups]` in non-`[project]` projects"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/dep-groups
created_at: 2024-10-25T18:44:38Z
updated_at: 2024-10-25T18:57:08Z
url: https://github.com/astral-sh/uv/pull/8574
synced_at: 2026-01-12T16:08:23Z
```

# Allow `[dependency-groups]` in non-`[project]` projects

---

_@charliermarsh_

## Summary

We already support `tool.uv.dev-dependencies` in the legacy non-`[project]` projects. This adds equivalent support for `[dependency-groups]`, e.g.:

```toml
[tool.uv.workspace]

[dependency-groups]
lint = ["ruff"]
```

---

_Label `enhancement` added by @charliermarsh on 2024-10-25 18:44_

---

_@charliermarsh reviewed on 2024-10-25 18:45_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/dependency_groups.rs`:95 on 2024-10-25 18:45_

This whole piece moved over from `requires_dist.rs`; no changes. I just wrapped it in a struct so that I could abstract it away and reuse it elsewhere.

---

_@charliermarsh reviewed on 2024-10-25 18:45_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/requires_dist.rs`:168 on 2024-10-25 18:45_

No changes here; just indentation from moving to `FlatDependencyGroups::from_dependency_groups`.

---

_Review requested from @zanieb by @charliermarsh on 2024-10-25 18:45_

---

_Comment by @zanieb on 2024-10-25 18:46_

This does not change the definition of non-project workspace roots right? `[tool.uv.workspace]` is still required?

---

_@zanieb approved on 2024-10-25 18:47_

---

_Comment by @charliermarsh on 2024-10-25 18:55_

Yeah that's correct.

---

_Merged by @zanieb on 2024-10-25 18:57_

---

_Closed by @zanieb on 2024-10-25 18:57_

---

_Branch deleted on 2024-10-25 18:57_

---
