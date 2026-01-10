```yaml
number: 4000
title: Make target Python version an optional field
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/req-i
created_at: 2024-06-03T22:15:28Z
updated_at: 2024-06-03T22:37:17Z
url: https://github.com/astral-sh/uv/pull/4000
synced_at: 2026-01-10T13:54:02Z
```

# Make target Python version an optional field

---

_Pull request opened by @charliermarsh on 2024-06-03 22:15_

## Summary

Instead of checking if the target and installed version are the same, we model the data such that the target version is only present if it was specified by the user. This also means that we correctly say "requested version" even if the two happen to be the same.


---

_Marked ready for review by @charliermarsh on 2024-06-03 22:29_

---

_Label `internal` added by @charliermarsh on 2024-06-03 22:29_

---

_Merged by @charliermarsh on 2024-06-03 22:37_

---

_Closed by @charliermarsh on 2024-06-03 22:37_

---

_Branch deleted on 2024-06-03 22:37_

---
