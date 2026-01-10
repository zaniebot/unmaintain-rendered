```yaml
number: 8946
title: "Respect comma-separated extras in `--with`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ex-split
created_at: 2024-11-08T15:23:28Z
updated_at: 2024-11-08T20:13:31Z
url: https://github.com/astral-sh/uv/pull/8946
synced_at: 2026-01-10T12:00:00Z
```

# Respect comma-separated extras in `--with`

---

_Pull request opened by @charliermarsh on 2024-11-08 15:23_

## Summary

We need to treat `flask,anyio` as two requirements, but `psycopg[binary,pool]` as a single requirement.

Closes #8918.


---

_Label `bug` added by @charliermarsh on 2024-11-08 15:23_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-08 15:23_

---

_@zanieb approved on 2024-11-08 15:28_

---

_@zanieb reviewed on 2024-11-08 15:28_

---

_Review comment by @zanieb on `crates/uv-cli/src/comma.rs`:42 on 2024-11-08 15:28_

You always write these so nice

---

_Merged by @charliermarsh on 2024-11-08 20:13_

---

_Closed by @charliermarsh on 2024-11-08 20:13_

---

_Branch deleted on 2024-11-08 20:13_

---
