```yaml
number: 5261
title: "Add an automated job to sync `python-build-standalone` releases"
type: pull_request
state: merged
author: charliermarsh
labels:
  - releases
assignees: []
merged: true
base: main
head: charlie/sync-python
created_at: 2024-07-21T17:27:42Z
updated_at: 2024-07-22T18:55:49Z
url: https://github.com/astral-sh/uv/pull/5261
synced_at: 2026-01-10T13:37:23Z
```

# Add an automated job to sync `python-build-standalone` releases

---

_Pull request opened by @charliermarsh on 2024-07-21 17:27_

## Summary

Perhaps in the future we can trigger this directly on release in `python-build-standalone`, but for now it's a cron job.


---

_Label `release` added by @charliermarsh on 2024-07-21 17:36_

---

_Review requested from @zanieb by @charliermarsh on 2024-07-21 17:37_

---

_@charliermarsh reviewed on 2024-07-21 19:09_

---

_Review comment by @charliermarsh on `.github/workflows/python-build-standalone.yml`:30 on 2024-07-21 19:09_

This skips creating the PR if there are no changes, etc.

---

_Merged by @charliermarsh on 2024-07-22 18:55_

---

_Closed by @charliermarsh on 2024-07-22 18:55_

---

_Branch deleted on 2024-07-22 18:55_

---
