```yaml
number: 4787
title: Add standard filters for virtual environment bin directories and python executables
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/filter-bin
created_at: 2024-07-03T18:34:02Z
updated_at: 2024-07-10T15:09:45Z
url: https://github.com/astral-sh/uv/pull/4787
synced_at: 2026-01-10T13:42:52Z
```

# Add standard filters for virtual environment bin directories and python executables

---

_Pull request opened by @zanieb on 2024-07-03 18:34_

Needed over in https://github.com/astral-sh/uv/pull/4674

These filters are relatively aggressive and may have a high false positive rate, we don't need them for most tests so we leave them as opt-in for now.

---

_Label `internal` added by @zanieb on 2024-07-03 18:34_

---

_Label `testing` added by @zanieb on 2024-07-03 18:34_

---

_Marked ready for review by @zanieb on 2024-07-10 14:56_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-10 14:56_

---

_Review requested from @konstin by @zanieb on 2024-07-10 14:56_

---

_Comment by @zanieb on 2024-07-10 14:57_

I think we'll need these over in https://github.com/astral-sh/uv/pull/4835 too

---

_@konstin approved on 2024-07-10 15:06_

Can you add to the PR description why these are not filtered by default?

---

_Merged by @zanieb on 2024-07-10 15:09_

---

_Closed by @zanieb on 2024-07-10 15:09_

---

_Branch deleted on 2024-07-10 15:09_

---
