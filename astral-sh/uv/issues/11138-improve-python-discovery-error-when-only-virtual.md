```yaml
number: 11138
title: Improve Python discovery error when only virtual environments are allowed
type: issue
state: closed
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2025-01-31T18:23:48Z
updated_at: 2025-01-31T21:54:01Z
url: https://github.com/astral-sh/uv/issues/11138
synced_at: 2026-01-10T03:50:31Z
```

# Improve Python discovery error when only virtual environments are allowed

---

_Issue opened by @zanieb on 2025-01-31 18:23_

### Summary

e.g., in https://github.com/astral-sh/uv/issues/8344#issuecomment-2627744140 we are only searching for Python interpreters in virtual environments but

1) We query all the system interpreters still
2) We throw an invalid system interpreter message as the final error, which is entirely irrelevant 

We can do better here.

### Example

_No response_

---

_Label `enhancement` added by @zanieb on 2025-01-31 18:23_

---

_Label `error messages` added by @zanieb on 2025-01-31 18:23_

---

_Assigned to @zanieb by @zanieb on 2025-01-31 18:23_

---

_Label `enhancement` removed by @zanieb on 2025-01-31 18:23_

---

_Closed by @zanieb on 2025-01-31 21:54_

---

_Closed by @zanieb on 2025-01-31 21:54_

---
