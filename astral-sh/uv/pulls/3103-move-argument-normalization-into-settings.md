```yaml
number: 3103
title: Move argument normalization into settings construction
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/dup
created_at: 2024-04-17T19:03:34Z
updated_at: 2024-04-19T23:45:09Z
url: https://github.com/astral-sh/uv/pull/3103
synced_at: 2026-01-12T16:05:25Z
```

# Move argument normalization into settings construction

---

_@charliermarsh_

## Summary

No behavior changes, but the idea here is that we move the argument normalization code (e.g., create an `Upgrade` struct from `--upgrade` and `--upgrade-package`) into the `settings.rs` file, where we build the common settings structs.

This reduces a lot of the logic and duplication across commands in `main.rs`.

---

_Review requested from @zanieb by @charliermarsh on 2024-04-17 19:03_

---

_Review requested from @konstin by @charliermarsh on 2024-04-17 19:03_

---

_Label `internal` added by @charliermarsh on 2024-04-17 19:03_

---

_Marked ready for review by @charliermarsh on 2024-04-17 19:03_

---

_Comment by @charliermarsh on 2024-04-19 23:40_

Merging as it's just a simple internal refactor.

---

_Merged by @charliermarsh on 2024-04-19 23:45_

---

_Closed by @charliermarsh on 2024-04-19 23:45_

---

_Branch deleted on 2024-04-19 23:45_

---
