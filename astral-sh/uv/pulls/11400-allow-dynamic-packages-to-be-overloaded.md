```yaml
number: 11400
title: Allow dynamic packages to be overloaded
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/ambiguous
created_at: 2025-02-10T20:07:18Z
updated_at: 2025-02-10T20:21:03Z
url: https://github.com/astral-sh/uv/pull/11400
synced_at: 2026-01-10T11:10:37Z
```

# Allow dynamic packages to be overloaded

---

_Pull request opened by @charliermarsh on 2025-02-10 20:07_

## Summary

Now that `version` is an optional field, we shouldn't error if an unambiguous package is lacking a version. We can still enforce the same guarantees via `source`, since we always set version and source together, if the package is unambiguous. I also retained the same error for non-local packages that lack a version like this.

Closes https://github.com/astral-sh/uv/issues/11384.


---

_Label `bug` added by @charliermarsh on 2025-02-10 20:07_

---

_Merged by @charliermarsh on 2025-02-10 20:21_

---

_Closed by @charliermarsh on 2025-02-10 20:21_

---

_Branch deleted on 2025-02-10 20:21_

---
