```yaml
number: 10868
title: Remove unnecessary build backend from tests
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/backends
created_at: 2025-01-22T18:12:17Z
updated_at: 2025-01-22T18:51:00Z
url: https://github.com/astral-sh/uv/pull/10868
synced_at: 2026-01-10T11:45:15Z
```

# Remove unnecessary build backend from tests

---

_Pull request opened by @charliermarsh on 2025-01-22 18:12_

## Summary

These tests don't need a build backend. If we omit it, the project is treated as virtual, and we avoid building and installing it.

The only changes in the snapshots should be a decrement in resolve or install count, since we're often now omitting the project itself.

I left the build backend for anything borderline, including workspace members within tests.


---

_Label `testing` added by @charliermarsh on 2025-01-22 18:12_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-22 18:12_

---

_Review requested from @konstin by @charliermarsh on 2025-01-22 18:12_

---

_@zanieb approved on 2025-01-22 18:16_

---

_Merged by @charliermarsh on 2025-01-22 18:50_

---

_Closed by @charliermarsh on 2025-01-22 18:50_

---

_Branch deleted on 2025-01-22 18:51_

---
