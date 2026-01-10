```yaml
number: 1086
title: Remove UNC prefixes on Windows
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dunce
created_at: 2024-01-24T22:27:42Z
updated_at: 2024-01-25T16:44:23Z
url: https://github.com/astral-sh/uv/pull/1086
synced_at: 2026-01-10T15:39:03Z
```

# Remove UNC prefixes on Windows

---

_Pull request opened by @charliermarsh on 2024-01-24 22:27_

## Summary

This PR adds a `NormalizedDisplay` trait that we can use for user-facing paths, to strip the UNC prefix on Windows.

On other platforms, the implementation is a no-op (vs. `Display`).

I audited all usages of `.display()`, and changed any that were user-facing, either via `println!` or `eprintln!`, or by way of being included in error messages. I did _not_ change uses that were only in tests or only went to tracing.

Closes https://github.com/astral-sh/puffin/issues/1084.


---

_Review requested from @konstin by @charliermarsh on 2024-01-24 22:27_

---

_Label `bug` added by @charliermarsh on 2024-01-24 22:27_

---

_Comment by @charliermarsh on 2024-01-24 22:28_

Tested this on my Windows box too.

---

_Review comment by @konstin on `crates/puffin-path/Cargo.toml`:1 on 2024-01-25 08:39_

Should this be part of puffin-fs? The crate list is beginning to get long

---

_@konstin approved on 2024-01-25 08:40_

---

_Merged by @charliermarsh on 2024-01-25 16:44_

---

_Closed by @charliermarsh on 2024-01-25 16:44_

---

_Branch deleted on 2024-01-25 16:44_

---
