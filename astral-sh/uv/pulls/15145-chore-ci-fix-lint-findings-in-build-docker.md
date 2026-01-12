```yaml
number: 15145
title: "chore(ci): fix lint findings in build-docker"
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/build-docker-lint
created_at: 2025-08-07T19:06:37Z
updated_at: 2025-08-07T19:58:15Z
url: https://github.com/astral-sh/uv/pull/15145
synced_at: 2026-01-12T16:11:36Z
```

# chore(ci): fix lint findings in build-docker

---

_@woodruffw_

## Summary

Addresses zizmor findings in `build-docker.yml`.

Key changes: primarily removing template expansions and restricting some permissions.

## Test Plan

Let the CI run.

---

_Assigned to @woodruffw by @woodruffw on 2025-08-07 19:06_

---

_Label `internal` added by @woodruffw on 2025-08-07 19:06_

---

_Review comment by @woodruffw on `.github/workflows/build-docker.yml`:39 on 2025-08-07 19:07_

This drops workflow-wide permissions; I confirmed that each of the jobs below that uses the `GITHUB_TOKEN` has its own permissions explicitly set.

---

_@woodruffw reviewed on 2025-08-07 19:07_

---

_Review requested from @zanieb by @woodruffw on 2025-08-07 19:27_

---

_Review requested from @konstin by @woodruffw on 2025-08-07 19:27_

---

_@zanieb approved on 2025-08-07 19:58_

---

_Merged by @zanieb on 2025-08-07 19:58_

---

_Closed by @zanieb on 2025-08-07 19:58_

---

_Branch deleted on 2025-08-07 19:58_

---
