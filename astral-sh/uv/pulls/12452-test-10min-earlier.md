```yaml
number: 12452
title: Test 10min earlier
type: pull_request
state: closed
author: konstin
labels:
  - internal
assignees: []
base: main
head: konsti/setuptools-tests
created_at: 2025-03-24T21:30:25Z
updated_at: 2025-03-25T01:28:53Z
url: https://github.com/astral-sh/uv/pull/12452
synced_at: 2026-01-10T11:10:40Z
```

# Test 10min earlier

---

_Pull request opened by @konstin on 2025-03-24 21:30_

Setuptools released their version with the fix at 2025-03-24 19:50:41 UTC, while we use exclude newer with 2025-03-24 20:00:00 UTC. We fix this by lowering the bound to 2025-03-24 19:00:00.

---

_Label `internal` added by @konstin on 2025-03-24 21:30_

---

_@zanieb approved on 2025-03-24 21:32_

---

_Comment by @charliermarsh on 2025-03-25 01:28_

I ended up reverting so shouldn't be necessary. Thanks @konstin.

---

_Closed by @charliermarsh on 2025-03-25 01:28_

---
