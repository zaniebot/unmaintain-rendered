```yaml
number: 6569
title: Fix basic authentication tests to reflect proxy changes
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/fix-auth
created_at: 2024-08-24T05:52:24Z
updated_at: 2024-08-24T05:59:15Z
url: https://github.com/astral-sh/uv/pull/6569
synced_at: 2026-01-12T16:07:25Z
```

# Fix basic authentication tests to reflect proxy changes

---

_@zanieb_

Updates the snapshot for the deployment from https://github.com/astral-sh/pypi-proxy/pull/9 — for a while now, we've only been failing on file requests not registry requests because the proxy auth was setup wrong.

---

_Label `internal` added by @zanieb on 2024-08-24 05:52_

---

_@zanieb reviewed on 2024-08-24 05:54_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:4068 on 2024-08-24 05:54_

Interestingly, these should be unauthorized requests as well? We might need to add error hints for this case.

---

_Merged by @zanieb on 2024-08-24 05:59_

---

_Closed by @zanieb on 2024-08-24 05:59_

---

_Branch deleted on 2024-08-24 05:59_

---
