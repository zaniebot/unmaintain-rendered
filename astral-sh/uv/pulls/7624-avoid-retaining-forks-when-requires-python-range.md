```yaml
number: 7624
title: "Avoid retaining forks when `requires-python` range changes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/py
created_at: 2024-09-22T17:21:23Z
updated_at: 2024-09-23T07:33:38Z
url: https://github.com/astral-sh/uv/pull/7624
synced_at: 2026-01-10T12:53:51Z
```

# Avoid retaining forks when `requires-python` range changes

---

_Pull request opened by @charliermarsh on 2024-09-22 17:21_

## Summary

If the `requires-python` bound expands, the space covered by `resolution-markers` may no longer include all supported Python versions. In such cases, we need to avoid reusing the forks (but we _can_ reuse the preferred versions).

Closes https://github.com/astral-sh/uv/issues/7618.


---

_Label `bug` added by @charliermarsh on 2024-09-22 17:21_

---

_Merged by @charliermarsh on 2024-09-22 17:36_

---

_Closed by @charliermarsh on 2024-09-22 17:36_

---

_Branch deleted on 2024-09-22 17:36_

---

_@konstin reviewed on 2024-09-23 07:33_

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:593 on 2024-09-23 07:33_

This looks like it is now a
```
struct ValidatedLock {
    lock: Lock,
    versions_usable: bool,
    forks_usable: bool,
}

---
