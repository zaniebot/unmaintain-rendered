```yaml
number: 7110
title: "Add `--no-emit-project` and friends to `uv export`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/no-emit
created_at: 2024-09-06T00:44:08Z
updated_at: 2024-09-06T01:01:47Z
url: https://github.com/astral-sh/uv/pull/7110
synced_at: 2026-01-10T12:53:40Z
```

# Add `--no-emit-project` and friends to `uv export`

---

_Pull request opened by @charliermarsh on 2024-09-06 00:44_

## Summary

Like `uv sync`, you can omit the current project (`--no-emit-project`), a specific package (`--no-emit-package`), or the entire workspace (`--no-emit-workspace`).

Closes https://github.com/astral-sh/uv/issues/6960.

Closes #6995.


---

_Label `enhancement` added by @charliermarsh on 2024-09-06 00:44_

---

_Label `cli` added by @charliermarsh on 2024-09-06 00:44_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-06 00:44_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2989 on 2024-09-06 00:46_

```suggestion
    /// Do not emit the given package(s).
```

---

_@zanieb reviewed on 2024-09-06 00:46_

---

_@zanieb approved on 2024-09-06 00:47_

---

_Merged by @charliermarsh on 2024-09-06 01:01_

---

_Closed by @charliermarsh on 2024-09-06 01:01_

---

_Branch deleted on 2024-09-06 01:01_

---
