```yaml
number: 7305
title: Avoid enforcing platform compatibility when validating lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/ext-warn
created_at: 2024-09-11T19:09:56Z
updated_at: 2024-09-11T19:17:08Z
url: https://github.com/astral-sh/uv/pull/7305
synced_at: 2026-01-10T12:53:44Z
```

# Avoid enforcing platform compatibility when validating lockfile

---

_Pull request opened by @charliermarsh on 2024-09-11 19:09_

## Summary

We have to call `to_dist` to get metadata while validating the lockfile, but some of the distributions won't match the current platform -- and that's fine!


---

_Label `bug` added by @charliermarsh on 2024-09-11 19:10_

---

_Label `error messages` added by @charliermarsh on 2024-09-11 19:10_

---

_Merged by @charliermarsh on 2024-09-11 19:17_

---

_Closed by @charliermarsh on 2024-09-11 19:17_

---

_Branch deleted on 2024-09-11 19:17_

---
