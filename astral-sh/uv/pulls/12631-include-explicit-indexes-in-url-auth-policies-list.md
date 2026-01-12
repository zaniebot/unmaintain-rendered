```yaml
number: 12631
title: Include explicit indexes in URL auth policies list
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/always-and-explicit
created_at: 2025-04-02T16:59:56Z
updated_at: 2025-04-02T17:54:33Z
url: https://github.com/astral-sh/uv/pull/12631
synced_at: 2026-01-12T16:10:20Z
```

# Include explicit indexes in URL auth policies list

---

_@jtfmumm_

This will in principle fix the problem reported in #12611 that `authenticate = "always"` is ignored for an index when `explicit = true`. This change ensures all indexes are added to the URL auth policies list passed to our auth middleware.

Incorporates #12624
Fixes #12611


---

_Label `bug` added by @jtfmumm on 2025-04-02 16:59_

---

_Review requested from @zanieb by @jtfmumm on 2025-04-02 17:11_

---

_@zanieb approved on 2025-04-02 17:54_

---

_Merged by @zanieb on 2025-04-02 17:54_

---

_Closed by @zanieb on 2025-04-02 17:54_

---

_Branch deleted on 2025-04-02 17:54_

---
