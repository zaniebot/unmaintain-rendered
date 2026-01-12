```yaml
number: 17096
title: "Better error handling for `uv publish`"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/better-retry-handling
created_at: 2025-12-12T10:49:59Z
updated_at: 2025-12-12T17:02:42Z
url: https://github.com/astral-sh/uv/pull/17096
synced_at: 2026-01-12T16:12:36Z
```

# Better error handling for `uv publish`

---

_@konstin_

* Use `is_transient_network_error` as we do in all other cases, see also https://github.com/astral-sh/uv/pull/16245
* Don't report success in the progress reporter if the upload failed


---

_Label `enhancement` added by @konstin on 2025-12-12 10:50_

---

_Review requested from @zanieb by @konstin on 2025-12-12 17:02_

---

_Comment by @konstin on 2025-12-12 17:02_

Merging as requirement for https://github.com/astral-sh/uv/pull/17098

---

_Merged by @konstin on 2025-12-12 17:02_

---

_Closed by @konstin on 2025-12-12 17:02_

---

_Branch deleted on 2025-12-12 17:02_

---
