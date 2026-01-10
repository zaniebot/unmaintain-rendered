```yaml
number: 1193
title: Enable macOS checks on CI
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/mac-os
created_at: 2024-01-30T21:08:09Z
updated_at: 2024-01-31T15:27:05Z
url: https://github.com/astral-sh/uv/pull/1193
synced_at: 2026-01-10T15:39:03Z
```

# Enable macOS checks on CI

---

_Pull request opened by @charliermarsh on 2024-01-30 21:08_

## Summary

Enables tests for macOS in CI, using the M1 runners (which are free in public, but count against our quota in private 
repos). For now, I'm just running them on `main` to save quota.

I did the math, and the M1 runners are the best value:

![Screenshot 2024-01-30 at 9 33 36 PM](https://github.com/astral-sh/puffin/assets/1309177/bd5a14b6-740c-487f-bcad-81c0fce5b62e)

Closes #1053.

---

_Label `internal` added by @charliermarsh on 2024-01-30 21:08_

---

_Marked ready for review by @charliermarsh on 2024-01-30 21:28_

---

_Converted to draft by @charliermarsh on 2024-01-31 02:14_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-31 02:34_

---

_Review requested from @konstin by @charliermarsh on 2024-01-31 02:34_

---

_Marked ready for review by @charliermarsh on 2024-01-31 02:34_

---

_@charliermarsh reviewed on 2024-01-31 02:34_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:78 on 2024-01-31 02:34_

I'm open to just not bothering with this, and turning these on now.

---

_@konstin approved on 2024-01-31 12:34_

---

_@zanieb reviewed on 2024-01-31 14:41_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:78 on 2024-01-31 14:41_

I also see no problem just turning them on now.

---

_Merged by @charliermarsh on 2024-01-31 15:27_

---

_Closed by @charliermarsh on 2024-01-31 15:27_

---

_Branch deleted on 2024-01-31 15:27_

---
