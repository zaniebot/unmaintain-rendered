```yaml
number: 7417
title: Fix release validation step
type: pull_request
state: merged
author: zanieb
labels:
  - release
assignees: []
merged: true
base: main
head: zanie/fix-release-checks
created_at: 2023-09-15T19:56:18Z
updated_at: 2023-09-15T21:08:19Z
url: https://github.com/astral-sh/ruff/pull/7417
synced_at: 2026-01-12T02:39:10Z
```

# Fix release validation step

---

_Pull request opened by @zanieb on 2023-09-15 19:56_

Proof of concept at #7416
Fixes `main` branch check added in #7279 (see [failure](https://github.com/astral-sh/ruff/actions/runs/6201772425/job/16839150669))
Removes the meaningless "SHA consistency" check (since we literally check out the SHA now)

---

_Label `release` added by @zanieb on 2023-09-15 19:56_

---

_@charliermarsh approved on 2023-09-15 20:12_

---

_Merged by @zanieb on 2023-09-15 20:18_

---

_Closed by @zanieb on 2023-09-15 20:18_

---

_Branch deleted on 2023-09-15 20:18_

---

_Comment by @zanieb on 2023-09-15 21:08_

:) https://github.com/astral-sh/ruff/actions/runs/6202724487/job/16842052746

---
