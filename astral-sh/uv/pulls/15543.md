```yaml
number: 15543
title: Support file or directory removal for Windows symlinks
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: charlie/rm
created_at: 2025-08-27T02:02:30Z
updated_at: 2025-08-27T11:43:04Z
url: https://github.com/astral-sh/uv/pull/15543
synced_at: 2026-01-10T06:44:33Z
```

# Support file or directory removal for Windows symlinks

---

_Pull request opened by @charliermarsh on 2025-08-27 02:02_

## Summary

Similar to https://github.com/rust-lang/cargo/pull/13910.

I think this should close https://github.com/astral-sh/uv/issues/15541 since we're indiscriminately calling `remove_dir` on that dangling symlink.


---

_Label `bug` added by @charliermarsh on 2025-08-27 02:02_

---

_Label `windows` added by @charliermarsh on 2025-08-27 02:02_

---

_Marked ready for review by @charliermarsh on 2025-08-27 11:43_

---

_Merged by @charliermarsh on 2025-08-27 11:43_

---

_Closed by @charliermarsh on 2025-08-27 11:43_

---

_Branch deleted on 2025-08-27 11:43_

---
