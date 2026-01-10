```yaml
number: 10858
title: Invalidate lockfile when static versions change
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/v
created_at: 2025-01-22T14:38:19Z
updated_at: 2025-01-22T17:58:16Z
url: https://github.com/astral-sh/uv/pull/10858
synced_at: 2026-01-10T11:45:14Z
```

# Invalidate lockfile when static versions change

---

_Pull request opened by @charliermarsh on 2025-01-22 14:38_

## Summary

We should only be ignoring changes in `version` for dynamic projects; for static projects, it should still be enforced. We should also be invalidating the lockfile if a project goes from static to dynamic or vice versa.

Closes #10852.


---

_Label `bug` added by @charliermarsh on 2025-01-22 14:38_

---

_Review requested from @konstin by @zanieb on 2025-01-22 15:29_

---

_Merged by @charliermarsh on 2025-01-22 17:58_

---

_Closed by @charliermarsh on 2025-01-22 17:58_

---

_Branch deleted on 2025-01-22 17:58_

---
