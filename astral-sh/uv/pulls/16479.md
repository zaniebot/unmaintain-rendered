```yaml
number: 16479
title: Only run debug assertion tests when debug assertions are active
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/cfg-debug-assertion-tests
created_at: 2025-10-28T11:39:16Z
updated_at: 2025-10-28T12:21:59Z
url: https://github.com/astral-sh/uv/pull/16479
synced_at: 2026-01-10T06:36:16Z
```

# Only run debug assertion tests when debug assertions are active

---

_Pull request opened by @konstin on 2025-10-28 11:39_

These tests currently fail when running tests in release mode.

---

_Label `internal` added by @konstin on 2025-10-28 11:39_

---

_Renamed from "Only run debug assertions test when debug assertions are active" to "Only run debug assertion tests when debug assertions are active" by @konstin on 2025-10-28 11:39_

---

_@zanieb approved on 2025-10-28 12:04_

---

_Comment by @zanieb on 2025-10-28 12:05_

Could we instead assert it returns `None` in production instead of turning off the whole test?

---

_Comment by @zanieb on 2025-10-28 12:21_

Awesome thanks!

---

_Merged by @konstin on 2025-10-28 12:21_

---

_Closed by @konstin on 2025-10-28 12:21_

---

_Branch deleted on 2025-10-28 12:21_

---
