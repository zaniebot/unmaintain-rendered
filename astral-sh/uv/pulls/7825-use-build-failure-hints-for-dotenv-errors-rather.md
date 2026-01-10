```yaml
number: 7825
title: "Use build failure hints for `dotenv` errors, rather than in `uv add`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/hints
created_at: 2024-10-01T01:20:21Z
updated_at: 2024-10-01T16:31:28Z
url: https://github.com/astral-sh/uv/pull/7825
synced_at: 2026-01-10T12:53:57Z
```

# Use build failure hints for `dotenv` errors, rather than in `uv add`

---

_Pull request opened by @charliermarsh on 2024-10-01 01:20_

## Summary

We now display the "Did you mean `python-dotenv`?"-style errors on build failure, rather than in `uv add`. This is less opinionated and couples us less to specific content in the registry.

## Test Plan

![Screenshot 2024-09-30 at 10 15 05 PM](https://github.com/user-attachments/assets/5b6684e2-d992-4f20-82ab-05632779ba91)


---

_Label `error messages` added by @charliermarsh on 2024-10-01 01:20_

---

_Marked ready for review by @charliermarsh on 2024-10-01 01:20_

---

_Comment by @charliermarsh on 2024-10-01 01:23_

Hmm, this actually needs to go elsewhere.

---

_Review requested from @zanieb by @charliermarsh on 2024-10-01 02:14_

---

_@zanieb approved on 2024-10-01 14:10_

---

_Merged by @charliermarsh on 2024-10-01 16:31_

---

_Closed by @charliermarsh on 2024-10-01 16:31_

---

_Branch deleted on 2024-10-01 16:31_

---
