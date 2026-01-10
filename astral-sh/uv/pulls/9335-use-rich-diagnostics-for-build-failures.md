```yaml
number: 9335
title: Use rich diagnostics for build failures
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/er
created_at: 2024-11-21T19:33:16Z
updated_at: 2024-11-26T20:06:01Z
url: https://github.com/astral-sh/uv/pull/9335
synced_at: 2026-01-10T12:00:00Z
```

# Use rich diagnostics for build failures

---

_Pull request opened by @charliermarsh on 2024-11-21 19:33_

## Summary

Closes https://github.com/astral-sh/uv/issues/9323.


---

_Label `error messages` added by @charliermarsh on 2024-11-21 19:33_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-21 19:43_

---

_@charliermarsh reviewed on 2024-11-21 19:44_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/build.rs`:237 on 2024-11-21 19:44_

I don't really know what exactly we want here... Right now, we only show the package name if they attempt to build multiple. We could also say "Failed to build source distribution for...", but it says "Building source distribution above". Or we could omit this line entirely.

---

_Merged by @charliermarsh on 2024-11-26 20:06_

---

_Closed by @charliermarsh on 2024-11-26 20:06_

---

_Branch deleted on 2024-11-26 20:06_

---
