```yaml
number: 10984
title: Improve publish test script resilience
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/publish-test-script
created_at: 2025-01-27T12:04:06Z
updated_at: 2025-01-27T19:20:35Z
url: https://github.com/astral-sh/uv/pull/10984
synced_at: 2026-01-12T16:09:37Z
```

# Improve publish test script resilience

---

_@konstin_

I went through the recent failures on main and tried to add resilience loops against those cases.

Github actions logs don't properly separate stdout and stderr written at different times, so we write everything to stderr now.

---

_Label `internal` added by @konstin on 2025-01-27 12:04_

---

_Review requested from @zanieb by @konstin on 2025-01-27 16:41_

---

_@zanieb approved on 2025-01-27 17:55_

---

_Merged by @konstin on 2025-01-27 19:20_

---

_Closed by @konstin on 2025-01-27 19:20_

---

_Branch deleted on 2025-01-27 19:20_

---
