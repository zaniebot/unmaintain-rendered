```yaml
number: 11149
title: Build a separate ARM wheel for macOS
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/arm
created_at: 2024-04-25T16:44:19Z
updated_at: 2024-04-25T19:06:48Z
url: https://github.com/astral-sh/ruff/pull/11149
synced_at: 2026-01-12T15:55:35Z
```

# Build a separate ARM wheel for macOS

---

_@charliermarsh_

## Summary

Since we already build an x86 wheel, we can just build an ARM wheel rather than cross-compiling to universal.

The build time is ~3 minutes vs. > 20 minutes and the resulting artifact is much smaller, which is also a win for users.


---

_Label `release` added by @charliermarsh on 2024-04-25 16:44_

---

_Review requested from @konstin by @charliermarsh on 2024-04-25 17:08_

---

_Marked ready for review by @charliermarsh on 2024-04-25 17:08_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-25 17:25_

---

_@konstin approved on 2024-04-25 19:06_

---

_Merged by @charliermarsh on 2024-04-25 19:06_

---

_Closed by @charliermarsh on 2024-04-25 19:06_

---

_Branch deleted on 2024-04-25 19:06_

---
