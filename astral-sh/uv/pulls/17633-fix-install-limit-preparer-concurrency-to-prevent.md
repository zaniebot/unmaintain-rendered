```yaml
number: 17633
title: "fix(install): limit preparer concurrency to prevent file handle exhaustion"
type: pull_request
state: open
author: denyszhak
labels:
  - bug
assignees: []
base: main
head: fix/installer-concurrency
created_at: 2026-01-20T22:54:03Z
updated_at: 2026-01-21T14:07:07Z
url: https://github.com/astral-sh/uv/pull/17633
synced_at: 2026-01-21T15:06:00Z
```

# fix(install): limit preparer concurrency to prevent file handle exhaustion

---

_@denyszhak_

## Summary

Resolves #17512 

Fixes an issue where uv could exhaust the operating system's file descriptor limit when installing projects with a large number of local dependencies, causing a "Too many open files (os error 24)" crash.

## Test Plan

1. Built uv version that has an issue
2. Issued `ulimit -n 256` to set number of allowed descriptors to a low value
3. `mkdir -p /tmp/airflow-test`
4. `git clone --depth 1 https://github.com/apache/airflow /tmp/airflow-test`
5. `cd /tmp/airflow-test`
6. `/Users/denyszhak/personal-repos/uv/target/debug/uv sync`

Bug: Hit `Too many open files (os error 24) at path "/Users/denyszhak/.cache/uv/sdists-v9/editable/136c133a3434a0fa/.tmpZtqKQt"`

7. Built uv version with the fix (source branch)
8. `/Users/denyszhak/personal-repos/uv/target/debug/uv sync`

Fix: Successfully `Resolved 922 packages in 10.82s`

---

_Comment by @denyszhak on 2026-01-20 23:06_

Not sure why test job is failing, seems unrelated to the change at first.

---

_Comment by @konstin on 2026-01-21 11:53_

Yep the failure is unrelated: https://github.com/astral-sh/uv/pull/17637

---

_Label `bug` added by @konstin on 2026-01-21 12:00_

---

_Comment by @konstin on 2026-01-21 12:04_

Interesting, I thought we were already using limits early but it seems we aren't.

CC @charliermarsh for the preparer code.

---

_Review requested from @charliermarsh by @konstin on 2026-01-21 12:04_

---

_@konstin reviewed on 2026-01-21 12:07_

---

_Review comment by @konstin on `crates/uv/src/commands/pip/operations.rs`:752 on 2026-01-21 12:07_

this limits both download and build concurrency by build concurrency. It means that e.g. a machine with 8 threads and default settings would get only 8 parallel downloads instead of 50.

---

_@denyszhak reviewed on 2026-01-21 14:07_

---

_Review comment by @denyszhak on `crates/uv/src/commands/pip/operations.rs`:752 on 2026-01-21 14:07_

Thanks, I missed that


---
