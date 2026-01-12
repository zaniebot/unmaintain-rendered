```yaml
number: 14252
title: Add check for using minor version link when creating a venv on Windows
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/win-venv
created_at: 2025-06-25T07:45:39Z
updated_at: 2025-06-25T08:12:34Z
url: https://github.com/astral-sh/uv/pull/14252
synced_at: 2026-01-12T16:11:06Z
```

# Add check for using minor version link when creating a venv on Windows

---

_@jtfmumm_

There was a regression introduced in #13954 on Windows where creating a venv behaved as if there was a minor version link even if none existed. This PR adds a check to fix this.

Closes #14249.


---

_Label `bug` added by @jtfmumm on 2025-06-25 07:45_

---

_Review requested from @konstin by @jtfmumm on 2025-06-25 07:45_

---

_@konstin approved on 2025-06-25 07:47_

---

_Merged by @jtfmumm on 2025-06-25 08:12_

---

_Closed by @jtfmumm on 2025-06-25 08:12_

---

_Branch deleted on 2025-06-25 08:12_

---
