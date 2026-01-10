```yaml
number: 16629
title: Fix inclusive constraints on available package versions in resolver errors
type: pull_request
state: merged
author: zanieb
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: zb/fix-bounds
created_at: 2025-11-07T05:22:15Z
updated_at: 2025-11-07T15:23:39Z
url: https://github.com/astral-sh/uv/pull/16629
synced_at: 2026-01-10T06:28:12Z
```

# Fix inclusive constraints on available package versions in resolver errors

---

_Pull request opened by @zanieb on 2025-11-07 05:22_

Closes https://github.com/astral-sh/uv/issues/16626


---

_Label `bug` added by @zanieb on 2025-11-07 05:22_

---

_Label `error messages` added by @zanieb on 2025-11-07 05:22_

---

_Comment by @zanieb on 2025-11-07 05:28_

It's plausible we should strip these earlier? I could look into that, but this seems okay too and is reasonably localized.

---

_Review requested from @konstin by @zanieb on 2025-11-07 15:17_

---

_@konstin approved on 2025-11-07 15:21_

---

_Merged by @zanieb on 2025-11-07 15:23_

---

_Closed by @zanieb on 2025-11-07 15:23_

---

_Branch deleted on 2025-11-07 15:23_

---
