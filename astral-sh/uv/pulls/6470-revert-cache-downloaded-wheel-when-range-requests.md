```yaml
number: 6470
title: "Revert \"Cache downloaded wheel when range requests aren't supported\""
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/m
created_at: 2024-08-22T23:13:35Z
updated_at: 2024-08-23T14:05:05Z
url: https://github.com/astral-sh/uv/pull/6470
synced_at: 2026-01-10T13:09:51Z
```

# Revert "Cache downloaded wheel when range requests aren't supported"

---

_Pull request opened by @charliermarsh on 2024-08-22 23:13_

## Summary

This reverts commit 7d92915f3dec3e7e76fa4c5ec5af7c17e3ca176c.

I thought this would be a net performance improvement, but we've now had multiple reports that this made locking _extremely_ slow. I also tested this today with a very large codebase against a registry that does not support range requests, and the number of downloads was sort of wild to watch. Reverting the reduced resolution time by over 50%.

Closes #6104.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-22 23:14_

---

_@zanieb approved on 2024-08-22 23:14_

---

_Label `performance` added by @zanieb on 2024-08-22 23:14_

---

_Merged by @charliermarsh on 2024-08-22 23:54_

---

_Closed by @charliermarsh on 2024-08-22 23:54_

---

_Branch deleted on 2024-08-22 23:54_

---
