```yaml
number: 14380
title: Use parsed URLs for conflicting URL error message
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/conf
created_at: 2025-07-01T00:07:41Z
updated_at: 2025-07-01T12:18:03Z
url: https://github.com/astral-sh/uv/pull/14380
synced_at: 2026-01-12T16:11:11Z
```

# Use parsed URLs for conflicting URL error message

---

_@charliermarsh_

## Summary

There's a good example of the downside of using verbatim URLs here: https://github.com/astral-sh/uv/pull/14197#discussion_r2163599625 (we show two relative paths that point to the same directory, but it's not clear from the error message).

The diff:

```
    2     2 │ ----- stdout -----
    3     3 │
    4     4 │ ----- stderr -----
    5     5 │ error: Requirements contain conflicting URLs for package `library` in all marker environments:
    6       │-- ../../library
    7       │-- ./library
          6 │+- file://[TEMP_DIR]/library
          7 │+- file://[TEMP_DIR]/library (editable)
```


---

_Label `error messages` added by @charliermarsh on 2025-07-01 00:07_

---

_Review requested from @konstin by @charliermarsh on 2025-07-01 00:14_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-01 00:19_

---

_Marked ready for review by @charliermarsh on 2025-07-01 00:19_

---

_@konstin approved on 2025-07-01 09:50_

nice

---

_Merged by @charliermarsh on 2025-07-01 12:18_

---

_Closed by @charliermarsh on 2025-07-01 12:18_

---

_Branch deleted on 2025-07-01 12:18_

---
