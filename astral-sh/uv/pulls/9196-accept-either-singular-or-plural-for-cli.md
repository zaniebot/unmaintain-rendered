```yaml
number: 9196
title: Accept either singular or plural for CLI constraints
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/c
created_at: 2024-11-18T13:46:57Z
updated_at: 2024-11-20T15:31:24Z
url: https://github.com/astral-sh/uv/pull/9196
synced_at: 2026-01-10T12:00:00Z
```

# Accept either singular or plural for CLI constraints

---

_Pull request opened by @charliermarsh on 2024-11-18 13:46_

## Summary

I find myself messing this up with `--build-constraint` vs. `--build-constraints`, and it turns out our own CLI isn't fully consistent here either.


---

_Label `cli` added by @charliermarsh on 2024-11-18 13:47_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-18 17:14_

---

_Comment by @charliermarsh on 2024-11-18 17:15_

I'd be fine to make plural the default too (and singular an alias).

---

_@zanieb approved on 2024-11-20 14:51_

Yeah I'm all for reducing rough edges in this way

---

_Merged by @charliermarsh on 2024-11-20 15:31_

---

_Closed by @charliermarsh on 2024-11-20 15:31_

---

_Branch deleted on 2024-11-20 15:31_

---
