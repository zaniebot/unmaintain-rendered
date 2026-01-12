```yaml
number: 3195
title: Fix Docker publish permissions in release pipeline
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/fix-docker-ci
created_at: 2024-04-22T19:44:49Z
updated_at: 2024-04-23T02:20:25Z
url: https://github.com/astral-sh/uv/pull/3195
synced_at: 2026-01-12T16:05:29Z
```

# Fix Docker publish permissions in release pipeline

---

_@zanieb_

The publish failed due to missing permissions. We add them here so the next release will include Docker builds.

---

_Label `release` added by @zanieb on 2024-04-22 19:44_

---

_@zanieb reviewed on 2024-04-22 19:45_

---

_Review comment by @zanieb on `.github/workflows/release.yml`:105 on 2024-04-22 19:45_

See failure at https://github.com/astral-sh/uv/actions/runs/8789651447

---

_@zanieb reviewed on 2024-04-22 19:45_

---

_Review comment by @zanieb on `Cargo.toml`:248 on 2024-04-22 19:45_

See failure at https://github.com/astral-sh/uv/actions/runs/8789388941

---

_Assigned to @charliermarsh by @zanieb on 2024-04-22 19:48_

---

_Merged by @charliermarsh on 2024-04-22 23:05_

---

_Closed by @charliermarsh on 2024-04-22 23:05_

---

_Branch deleted on 2024-04-22 23:05_

---
