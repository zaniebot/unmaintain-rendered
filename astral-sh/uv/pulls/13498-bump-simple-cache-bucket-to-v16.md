```yaml
number: 13498
title: Bump simple cache bucket to v16
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/simple-v16
created_at: 2025-05-16T20:41:14Z
updated_at: 2025-05-19T06:03:23Z
url: https://github.com/astral-sh/uv/pull/13498
synced_at: 2026-01-10T11:10:41Z
```

# Bump simple cache bucket to v16

---

_Pull request opened by @konstin on 2025-05-16 20:41_

We broke the rkyv deserialization in https://github.com/astral-sh/uv/commit/e70cf25ea72c94502d08637474e9e4ad576bf342#diff-348b24d7a84672ab2873833988156191995ff467619a77f548adbd9808549999L30-R41 by placing `.tar.bz2` in the position of `.tar.gz`. We need to invalidate the cache bucket.

Fixes #13492


---

_Review requested from @charliermarsh by @konstin on 2025-05-16 20:41_

---

_Label `bug` added by @konstin on 2025-05-16 20:41_

---

_Merged by @charliermarsh on 2025-05-17 00:17_

---

_Closed by @charliermarsh on 2025-05-17 00:17_

---

_Branch deleted on 2025-05-17 00:17_

---
