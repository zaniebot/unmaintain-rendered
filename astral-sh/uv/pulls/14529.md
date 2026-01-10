```yaml
number: 14529
title: "Default to `--workspace` when adding subdirectories"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - breaking
assignees: []
merged: true
base: release/080
head: charlie/add-workspace
created_at: 2025-07-09T21:15:35Z
updated_at: 2025-07-11T02:05:51Z
url: https://github.com/astral-sh/uv/pull/14529
synced_at: 2026-01-10T06:53:02Z
```

# Default to `--workspace` when adding subdirectories

---

_Pull request opened by @charliermarsh on 2025-07-09 21:15_

## Summary

If `--workspace` is provided, we add all paths as workspace members.

If `--no-workspace` is provided, we add all paths as direct path dependencies.

If neither is provided, then we add any paths that are under the workspace root as workspace members, and the rest as direct path dependencies.

Closes #14524.


---

_Label `cli` added by @charliermarsh on 2025-07-09 21:15_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-09 21:15_

---

_Comment by @charliermarsh on 2025-07-09 21:16_

One question behavior is that we add as a workspace member even if there's no existing explicit `tool.uv.workspace` definition. So if you `uv init foo`, `cd foo`, create `bar`, then `uv add ./bar`, we'll create a new `tool.uv.workspace` in `foo`. I think that's fine, since it mirrors running `uv init bar` from `foo`.

---

_Label `breaking` added by @charliermarsh on 2025-07-09 21:16_

---

_Added to milestone `v0.8.0` by @charliermarsh on 2025-07-09 21:16_

---

_@zanieb approved on 2025-07-09 21:18_

---

_Marked ready for review by @charliermarsh on 2025-07-11 01:48_

---

_Merged by @charliermarsh on 2025-07-11 02:05_

---

_Closed by @charliermarsh on 2025-07-11 02:05_

---

_Branch deleted on 2025-07-11 02:05_

---
