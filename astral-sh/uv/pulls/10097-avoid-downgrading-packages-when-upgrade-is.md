```yaml
number: 10097
title: "Avoid downgrading packages when `--upgrade` is provided"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/downgrade
created_at: 2024-12-22T16:08:57Z
updated_at: 2025-01-06T22:41:45Z
url: https://github.com/astral-sh/uv/pull/10097
synced_at: 2026-01-12T16:09:07Z
```

# Avoid downgrading packages when `--upgrade` is provided

---

_@charliermarsh_

## Summary

When `--upgrade` is provided, we should retain already-installed packages _if_ they're newer than whatever is available from the registry.

Closes https://github.com/astral-sh/uv/issues/10089.


---

_Label `bug` added by @charliermarsh on 2024-12-22 16:10_

---

_Converted to draft by @charliermarsh on 2024-12-22 16:17_

---

_Comment by @charliermarsh on 2024-12-22 16:17_

(No reviews please, need to get tests passing.)

---

_Review requested from @zanieb by @charliermarsh on 2024-12-22 17:44_

---

_Marked ready for review by @charliermarsh on 2024-12-22 17:44_

---

_@zanieb approved on 2025-01-06 21:58_

---

_Merged by @charliermarsh on 2025-01-06 22:41_

---

_Closed by @charliermarsh on 2025-01-06 22:41_

---

_Branch deleted on 2025-01-06 22:41_

---
