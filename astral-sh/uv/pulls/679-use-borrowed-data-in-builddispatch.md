```yaml
number: 679
title: "Use borrowed data in `BuildDispatch`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/clone
created_at: 2023-12-18T02:42:40Z
updated_at: 2023-12-18T16:43:05Z
url: https://github.com/astral-sh/uv/pull/679
synced_at: 2026-01-12T16:04:07Z
```

# Use borrowed data in `BuildDispatch`

---

_@charliermarsh_

This PR uses borrowed data in `BuildDispatch` which makes creating a `BuildDispatch` extremely cheap (only one allocation, for the Python executable). I can be talked out of this, it will have no measurable impact.

---

_Review requested from @konstin by @charliermarsh on 2023-12-18 02:42_

---

_@konstin approved on 2023-12-18 09:33_

We should create only one build dispatch per run so there should be no impact, but we're also always passing an `&BuildDispatch` anyway so it's fine.

---

_Merged by @charliermarsh on 2023-12-18 16:43_

---

_Closed by @charliermarsh on 2023-12-18 16:43_

---

_Branch deleted on 2023-12-18 16:43_

---
