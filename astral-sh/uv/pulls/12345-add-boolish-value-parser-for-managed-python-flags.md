```yaml
number: 12345
title: "Add boolish value parser for *MANAGED_PYTHON flags"
type: pull_request
state: merged
author: jtfmumm
labels:
  - bug
assignees: []
merged: true
base: main
head: jtfm/fix-uv-managed-flag
created_at: 2025-03-20T18:33:48Z
updated_at: 2025-03-20T18:59:48Z
url: https://github.com/astral-sh/uv/pull/12345
synced_at: 2026-01-12T16:10:14Z
```

# Add boolish value parser for *MANAGED_PYTHON flags

---

_@jtfmumm_

There was a bug where `UV_MANAGED_PYTHON` and `UV_NO_MANAGED_PYTHON` only accepted `true` or `false`. This switches to the boolish value parser for those flags.

Closes #12336 


---

_Label `bug` added by @jtfmumm on 2025-03-20 18:33_

---

_@zanieb approved on 2025-03-20 18:34_

ty

---

_Merged by @jtfmumm on 2025-03-20 18:59_

---

_Closed by @jtfmumm on 2025-03-20 18:59_

---

_Branch deleted on 2025-03-20 18:59_

---
