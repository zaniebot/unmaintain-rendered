```yaml
number: 3606
title: "Remove `resolve_cli.rs` and `resolve_many.rs`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/r
created_at: 2024-05-15T15:57:54Z
updated_at: 2024-05-15T16:04:49Z
url: https://github.com/astral-sh/uv/pull/3606
synced_at: 2026-01-12T16:05:44Z
```

# Remove `resolve_cli.rs` and `resolve_many.rs`

---

_@charliermarsh_

## Summary

These depend on some APIs that I want to be internal-only for the resolver crate, and we're no longer using them.

---

_Review requested from @konstin by @charliermarsh on 2024-05-15 15:57_

---

_Label `internal` added by @charliermarsh on 2024-05-15 15:58_

---

_Marked ready for review by @charliermarsh on 2024-05-15 15:58_

---

_@zanieb approved on 2024-05-15 16:04_

Yeah updating them all the time doesn't feel worth it

---

_Merged by @charliermarsh on 2024-05-15 16:04_

---

_Closed by @charliermarsh on 2024-05-15 16:04_

---

_Branch deleted on 2024-05-15 16:04_

---
