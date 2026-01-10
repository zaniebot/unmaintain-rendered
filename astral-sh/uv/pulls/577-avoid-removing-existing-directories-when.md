```yaml
number: 577
title: Avoid removing existing directories when unzipping and building
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rmn
created_at: 2023-12-06T02:30:45Z
updated_at: 2023-12-06T02:36:13Z
url: https://github.com/astral-sh/uv/pull/577
synced_at: 2026-01-10T15:44:44Z
```

# Avoid removing existing directories when unzipping and building

---

_Pull request opened by @charliermarsh on 2023-12-06 02:30_

Now that we don't store zipped and unzipped wheels at the same location, we can avoid these safeguards that entail removing existing directories when writing. This supersedes https://github.com/astral-sh/puffin/pull/545.

Closes https://github.com/astral-sh/puffin/issues/554.

---

_@charliermarsh reviewed on 2023-12-06 02:31_

---

_Review comment by @charliermarsh on `crates/puffin-build/src/lib.rs`:429 on 2023-12-06 02:31_

We now assume `to` is a file, so renaming is fine. (If it's not a file, we should error.)

---

_Label `bug` added by @charliermarsh on 2023-12-06 02:33_

---

_Merged by @charliermarsh on 2023-12-06 02:36_

---

_Closed by @charliermarsh on 2023-12-06 02:36_

---

_Branch deleted on 2023-12-06 02:36_

---
