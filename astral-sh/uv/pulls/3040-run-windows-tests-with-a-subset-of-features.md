```yaml
number: 3040
title: Run Windows tests with a subset of features
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/test-flags
created_at: 2024-04-15T20:03:46Z
updated_at: 2024-04-16T13:14:20Z
url: https://github.com/astral-sh/uv/pull/3040
synced_at: 2026-01-10T14:43:31Z
```

# Run Windows tests with a subset of features

---

_Pull request opened by @zanieb on 2024-04-15 20:03_

Separates Windows tests from the rest because it's a pain and drops the `python-patch` feature from testing so we can use the GitHub Actions Python versions which bootstrap in 0 seconds instead of 2 minutes.

---

_Label `internal` added by @zanieb on 2024-04-15 20:03_

---

_Marked ready for review by @zanieb on 2024-04-15 21:03_

---

_Review requested from @konstin by @zanieb on 2024-04-15 21:03_

---

_@konstin approved on 2024-04-16 07:57_

love it :100:

---

_Merged by @zanieb on 2024-04-16 13:14_

---

_Closed by @zanieb on 2024-04-16 13:14_

---

_Branch deleted on 2024-04-16 13:14_

---
