```yaml
number: 15452
title: Allow more trailing null bytes in zip files
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2025-08-22T13:34:29Z
updated_at: 2025-08-22T13:50:09Z
url: https://github.com/astral-sh/uv/pull/15452
synced_at: 2026-01-12T16:11:44Z
```

# Allow more trailing null bytes in zip files

---

_@charliermarsh_

## Summary

There isn't any risk here, and we have reports of at least one zip file with more than one (but fewer than, e.g., 10) null bytes.

Closes https://github.com/astral-sh/uv/issues/15451.


---

_Review requested from @woodruffw by @charliermarsh on 2025-08-22 13:34_

---

_Label `bug` added by @charliermarsh on 2025-08-22 13:34_

---

_Marked ready for review by @charliermarsh on 2025-08-22 13:34_

---

_Label `compatibility` added by @charliermarsh on 2025-08-22 13:34_

---

_@woodruffw approved on 2025-08-22 13:44_

---

_Merged by @charliermarsh on 2025-08-22 13:50_

---

_Closed by @charliermarsh on 2025-08-22 13:50_

---

_Branch deleted on 2025-08-22 13:50_

---
