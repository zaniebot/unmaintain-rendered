```yaml
number: 8987
title: Fix prune unzipped snapshot
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/prune-unzipped
created_at: 2024-11-10T12:41:31Z
updated_at: 2024-11-10T17:44:45Z
url: https://github.com/astral-sh/uv/pull/8987
synced_at: 2026-01-10T12:00:00Z
```

# Fix prune unzipped snapshot

---

_Pull request opened by @konstin on 2024-11-10 12:41_

Fixes the tests on main, cause unknown.

---

_Label `internal` added by @konstin on 2024-11-10 12:41_

---

_Review requested from @charliermarsh by @konstin on 2024-11-10 12:41_

---

_Merged by @konstin on 2024-11-10 12:52_

---

_Closed by @konstin on 2024-11-10 12:52_

---

_Branch deleted on 2024-11-10 12:52_

---

_Comment by @charliermarsh on 2024-11-10 16:58_

Now this is failing for me locally? Confusing. I guess I'll make all these ignore counts.

---

_Comment by @konstin on 2024-11-10 17:43_

Huh hat number seems to be somewhat random? https://github.com/astral-sh/uv/actions/runs/11767132015/job/32775477102 (agreed that we should mask it, the exact number doesn't matter here, we can check that the relevant directories disappeared)

---

_Comment by @charliermarsh on 2024-11-10 17:44_

It really should be consistent -- I don't see why it would vary. But I don't want to investigate it right now.

---
