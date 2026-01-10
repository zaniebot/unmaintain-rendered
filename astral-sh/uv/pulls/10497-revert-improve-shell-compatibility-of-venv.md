```yaml
number: 10497
title: "Revert \"improve shell compatibility of venv activate scripts (#10397)\""
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rev
created_at: 2025-01-11T14:10:02Z
updated_at: 2025-01-11T14:23:09Z
url: https://github.com/astral-sh/uv/pull/10497
synced_at: 2026-01-10T11:44:52Z
```

# Revert "improve shell compatibility of venv activate scripts (#10397)"

---

_Pull request opened by @charliermarsh on 2025-01-11 14:10_

## Summary

This reverts commit 2f7f9ea571f4fa8d2eefbfa4a95f6bc8fd18175c (https://github.com/astral-sh/uv/pull/10397). We're seeing some user-reported failures, so we need to investigate further before re-shipping.

Re-opens https://github.com/astral-sh/uv/issues/7480.

Closes https://github.com/astral-sh/uv/issues/10487.


---

_Review requested from @Gankra by @charliermarsh on 2025-01-11 14:10_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-11 14:10_

---

_Label `bug` added by @charliermarsh on 2025-01-11 14:10_

---

_Comment by @charliermarsh on 2025-01-11 14:10_

(There may be pieces of this commit that are safe to retain but IMO easier to do a clean revert for now.)

---

_Merged by @charliermarsh on 2025-01-11 14:23_

---

_Closed by @charliermarsh on 2025-01-11 14:23_

---

_Branch deleted on 2025-01-11 14:23_

---
