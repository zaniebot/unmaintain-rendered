```yaml
number: 4880
title: Show user-facing warning when falling back to copy installs
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/warn
created_at: 2024-07-08T00:35:41Z
updated_at: 2024-07-08T13:35:59Z
url: https://github.com/astral-sh/uv/pull/4880
synced_at: 2026-01-10T13:42:52Z
```

# Show user-facing warning when falling back to copy installs

---

_Pull request opened by @charliermarsh on 2024-07-08 00:35_

## Summary

This has come up a few times including in a recent email to me.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-08 00:35_

---

_Review requested from @konstin by @charliermarsh on 2024-07-08 00:35_

---

_Label `error messages` added by @charliermarsh on 2024-07-08 00:35_

---

_Comment by @konstin on 2024-07-08 08:44_

There are some valid use cases, mainly docker, where we expect the source and target to be on different file systems. We should add how to silence the warning (`--link-mode copy`) for those.

---

_@konstin approved on 2024-07-08 08:45_

---

_Comment by @charliermarsh on 2024-07-08 13:03_

I did have that initially but it felt a bit long and hard to explain. I'll add it back.

---

_@charliermarsh reviewed on 2024-07-08 13:20_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:5109 on 2024-07-08 13:20_

This is an actual bug so fixing separately.

---

_Merged by @charliermarsh on 2024-07-08 13:35_

---

_Closed by @charliermarsh on 2024-07-08 13:35_

---

_Branch deleted on 2024-07-08 13:35_

---
