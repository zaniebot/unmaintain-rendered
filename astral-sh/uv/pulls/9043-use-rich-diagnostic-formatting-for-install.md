```yaml
number: 9043
title: Use rich diagnostic formatting for install failures
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/prepare
created_at: 2024-11-12T03:23:39Z
updated_at: 2024-11-12T03:54:41Z
url: https://github.com/astral-sh/uv/pull/9043
synced_at: 2026-01-12T16:08:37Z
```

# Use rich diagnostic formatting for install failures

---

_@charliermarsh_

## Summary

Shows similar diagnostics for failures that happen at install time, rather than resolve time. This will ultimately feed into https://github.com/astral-sh/uv/issues/8962 since we'll now have consolidated handling for these kinds of failures.


---

_Label `error messages` added by @charliermarsh on 2024-11-12 03:23_

---

_Merged by @charliermarsh on 2024-11-12 03:54_

---

_Closed by @charliermarsh on 2024-11-12 03:54_

---

_Branch deleted on 2024-11-12 03:54_

---

_Comment by @charliermarsh on 2024-11-12 03:54_

Again, this will all be DRYed up, promise!

---
