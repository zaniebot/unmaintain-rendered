```yaml
number: 15636
title: "Add pyx support to `uv auth` commands"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/auth-pyx
created_at: 2025-09-02T18:45:15Z
updated_at: 2025-09-03T09:01:21Z
url: https://github.com/astral-sh/uv/pull/15636
synced_at: 2026-01-12T16:11:51Z
```

# Add pyx support to `uv auth` commands

---

_@charliermarsh_

## Summary

This PR adds support for pyx to `uv auth login`, `uv auth logout`, and `uv auth token`. These are generic uv commands that can be used to store credentials for arbitrary indexes and other URLs, but we include a fast-path for pyx that initiates the appropriate login or logout flow.


---

_Review requested from @zanieb by @charliermarsh on 2025-09-02 18:45_

---

_Review requested from @konstin by @charliermarsh on 2025-09-02 18:45_

---

_Marked ready for review by @charliermarsh on 2025-09-02 18:45_

---

_Label `enhancement` added by @charliermarsh on 2025-09-02 18:45_

---

_@zanieb reviewed on 2025-09-02 19:07_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:472 on 2025-09-02 19:07_

Needs update, right?

---

_@zanieb approved on 2025-09-02 19:38_

---

_Merged by @charliermarsh on 2025-09-02 22:18_

---

_Closed by @charliermarsh on 2025-09-02 22:18_

---

_Branch deleted on 2025-09-02 22:18_

---
