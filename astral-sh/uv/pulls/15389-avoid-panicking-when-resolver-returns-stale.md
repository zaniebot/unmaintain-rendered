```yaml
number: 15389
title: Avoid panicking when resolver returns stale distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/plan
created_at: 2025-08-20T10:31:05Z
updated_at: 2025-08-20T14:04:40Z
url: https://github.com/astral-sh/uv/pull/15389
synced_at: 2026-01-12T16:11:43Z
```

# Avoid panicking when resolver returns stale distributions

---

_@charliermarsh_

## Summary

I've written a reasonably-long comment to explain what's going on here. We should fix this, but it's better to continue using a potentially-stale distribution than to panic.

Closes https://github.com/astral-sh/uv/issues/15386.


---

_Label `bug` added by @charliermarsh on 2025-08-20 10:31_

---

_Marked ready for review by @charliermarsh on 2025-08-20 10:31_

---

_@zanieb approved on 2025-08-20 13:52_

---

_Merged by @zanieb on 2025-08-20 14:04_

---

_Closed by @zanieb on 2025-08-20 14:04_

---

_Branch deleted on 2025-08-20 14:04_

---
