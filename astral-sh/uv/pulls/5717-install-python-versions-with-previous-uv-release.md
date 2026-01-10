```yaml
number: 5717
title: Install Python versions with previous uv release
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/ci-python-install
created_at: 2024-08-01T23:38:18Z
updated_at: 2024-08-01T23:59:30Z
url: https://github.com/astral-sh/uv/pull/5717
synced_at: 2026-01-10T13:37:23Z
```

# Install Python versions with previous uv release

---

_Pull request opened by @zanieb on 2024-08-01 23:38_

Part of https://github.com/astral-sh/uv/issues/5713

Shaves 50s or ~25% off the Ubuntu test run. Maybe 30s or 8% off macOS.
Windows already uses the GitHub distributions.

Note this is some of our only test coverage for Python version installs, we may want to add separate coverage to compensate. 

---

_Label `testing` added by @zanieb on 2024-08-01 23:38_

---

_@charliermarsh approved on 2024-08-01 23:47_

---

_Marked ready for review by @zanieb on 2024-08-01 23:52_

---

_Merged by @zanieb on 2024-08-01 23:59_

---

_Closed by @zanieb on 2024-08-01 23:59_

---

_Branch deleted on 2024-08-01 23:59_

---
