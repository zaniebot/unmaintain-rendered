```yaml
number: 6074
title: "Redact Git credentials from `pyproject.toml`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - security
  - preview
assignees: []
merged: true
base: main
head: charlie/git-creds-2
created_at: 2024-08-13T23:33:56Z
updated_at: 2024-08-14T01:30:03Z
url: https://github.com/astral-sh/uv/pull/6074
synced_at: 2026-01-10T13:09:50Z
```

# Redact Git credentials from `pyproject.toml`

---

_Pull request opened by @charliermarsh on 2024-08-13 23:33_

## Summary

We retain them if you use `--raw-sources`, but otherwise they're removed. We still respect them in the subsequent `uv.lock` via an in-process store.

Closes #6056.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-13 23:33_

---

_Label `security` added by @charliermarsh on 2024-08-13 23:34_

---

_Label `preview` added by @charliermarsh on 2024-08-13 23:34_

---

_@zanieb reviewed on 2024-08-14 00:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:460 on 2024-08-14 00:01_

?

---

_@zanieb reviewed on 2024-08-14 00:01_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:334 on 2024-08-14 00:01_

Should we say more about why? And the cache flow?

---

_@zanieb approved on 2024-08-14 00:02_

Nice.

---

_@charliermarsh reviewed on 2024-08-14 00:07_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/add.rs`:460 on 2024-08-14 00:07_

Solid

---

_Merged by @charliermarsh on 2024-08-14 01:30_

---

_Closed by @charliermarsh on 2024-08-14 01:30_

---

_Branch deleted on 2024-08-14 01:30_

---
