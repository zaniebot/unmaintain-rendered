```yaml
number: 11251
title: "Allow the project `VIRTUAL_ENV` warning to be silenced with `--no-active`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/fix-active
created_at: 2025-02-05T16:27:06Z
updated_at: 2025-02-05T19:44:48Z
url: https://github.com/astral-sh/uv/pull/11251
synced_at: 2026-01-10T11:10:34Z
```

# Allow the project `VIRTUAL_ENV` warning to be silenced with `--no-active`

---

_Pull request opened by @zanieb on 2025-02-05 16:27_

Follow-up to https://github.com/astral-sh/uv/pull/11189

Closes https://github.com/astral-sh/uv-pre-commit/issues/36

---

_Label `cli` added by @zanieb on 2025-02-05 16:27_

---

_@zanieb reviewed on 2025-02-05 16:28_

---

_Review comment by @zanieb on `crates/uv-workspace/src/workspace.rs`:637 on 2025-02-05 16:28_

This looked improperly guarded

---

_@zanieb reviewed on 2025-02-05 16:28_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:237 on 2025-02-05 16:28_

Embarassingly, these were swapped. Hence we should not pass bools everywhere.

---

_Marked ready for review by @zanieb on 2025-02-05 16:28_

---

_@charliermarsh approved on 2025-02-05 19:36_

---

_Merged by @zanieb on 2025-02-05 19:44_

---

_Closed by @zanieb on 2025-02-05 19:44_

---

_Branch deleted on 2025-02-05 19:44_

---
