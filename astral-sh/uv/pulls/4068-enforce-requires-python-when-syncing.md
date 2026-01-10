```yaml
number: 4068
title: "Enforce `Requires-Python` when syncing"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/requires-python-ii
created_at: 2024-06-05T20:01:22Z
updated_at: 2024-06-05T20:22:09Z
url: https://github.com/astral-sh/uv/pull/4068
synced_at: 2026-01-10T13:54:02Z
```

# Enforce `Requires-Python` when syncing

---

_Pull request opened by @charliermarsh on 2024-06-05 20:01_

## Summary

Ensures that we raise if the user attempts to use a Python version that wasn't included in the locked range.

Closes https://github.com/astral-sh/uv/issues/4052.


---

_Label `preview` added by @charliermarsh on 2024-06-05 20:01_

---

_Marked ready for review by @charliermarsh on 2024-06-05 20:01_

---

_@charliermarsh reviewed on 2024-06-05 20:09_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:85 on 2024-06-05 20:09_

Should this be a warning?

---

_Review requested from @zanieb by @charliermarsh on 2024-06-05 20:09_

---

_Review requested from @konstin by @charliermarsh on 2024-06-05 20:09_

---

_@zanieb reviewed on 2024-06-05 20:11_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/sync.rs`:85 on 2024-06-05 20:11_

Probably not, I feel like for a lockfile it makes sense to fail?

---

_@zanieb approved on 2024-06-05 20:11_

---

_Merged by @charliermarsh on 2024-06-05 20:22_

---

_Closed by @charliermarsh on 2024-06-05 20:22_

---

_Branch deleted on 2024-06-05 20:22_

---
