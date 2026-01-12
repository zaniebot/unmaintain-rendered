```yaml
number: 2186
title: Fallback to non-range requests when HEAD returns 404
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/head
created_at: 2024-03-04T23:19:52Z
updated_at: 2024-03-05T03:18:50Z
url: https://github.com/astral-sh/uv/pull/2186
synced_at: 2026-01-12T16:04:54Z
```

# Fallback to non-range requests when HEAD returns 404

---

_@charliermarsh_

## Summary

We have at least one reported case of this happening. It's preferable IMO to move on rather than fail hard despite sub-pbar registry behavior.

Closes https://github.com/astral-sh/uv/issues/2099.

---

_Review requested from @zanieb by @charliermarsh on 2024-03-05 00:34_

---

_Marked ready for review by @charliermarsh on 2024-03-05 00:35_

---

_Label `compatibility` added by @charliermarsh on 2024-03-05 00:35_

---

_@zanieb approved on 2024-03-05 03:13_

---

_Merged by @charliermarsh on 2024-03-05 03:18_

---

_Closed by @charliermarsh on 2024-03-05 03:18_

---

_Branch deleted on 2024-03-05 03:18_

---
