```yaml
number: 5865
title: "Avoid mismatch in `--locked` with Git dependencies"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/lock-git
created_at: 2024-08-07T15:07:42Z
updated_at: 2024-08-07T15:47:49Z
url: https://github.com/astral-sh/uv/pull/5865
synced_at: 2026-01-12T16:07:04Z
```

# Avoid mismatch in `--locked` with Git dependencies

---

_@charliermarsh_

## Summary

We were dropping the query and fragment in the wrong place, so the URLs didn't match up after resolving from an existing lockfile.

Closes https://github.com/astral-sh/uv/issues/5851.


---

_@charliermarsh reviewed on 2024-08-07 15:07_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:264 on 2024-08-07 15:07_

I'll follow-up to add this to all the test cases.

---

_Label `bug` added by @charliermarsh on 2024-08-07 15:09_

---

_Label `preview` added by @charliermarsh on 2024-08-07 15:09_

---

_Merged by @charliermarsh on 2024-08-07 15:47_

---

_Closed by @charliermarsh on 2024-08-07 15:47_

---

_Branch deleted on 2024-08-07 15:47_

---
