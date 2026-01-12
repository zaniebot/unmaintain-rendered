```yaml
number: 1170
title: Use tokio_tar instead of async_tar
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/tokio-tar
created_at: 2024-01-29T13:39:50Z
updated_at: 2024-01-29T15:00:31Z
url: https://github.com/astral-sh/uv/pull/1170
synced_at: 2026-01-12T16:04:29Z
```

# Use tokio_tar instead of async_tar

---

_@charliermarsh_

## Summary

`tokio_tar` is a fork of `async_tar` that uses Tokio instead of `async-std`. Using it removes a significant dependency from our tree.

(There is an open PR (https://github.com/dignifiedquire/async-tar/pull/41) in `async-tar` to add Tokio support, but it's over a year old.)

See: https://github.com/astral-sh/puffin/pull/1157#discussion_r1469190249.


---

_Review requested from @konstin by @charliermarsh on 2024-01-29 13:39_

---

_Label `internal` added by @charliermarsh on 2024-01-29 13:44_

---

_@konstin approved on 2024-01-29 14:59_

---

_Merged by @charliermarsh on 2024-01-29 15:00_

---

_Closed by @charliermarsh on 2024-01-29 15:00_

---

_Branch deleted on 2024-01-29 15:00_

---
