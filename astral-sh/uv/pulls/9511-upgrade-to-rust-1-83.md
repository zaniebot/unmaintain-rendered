```yaml
number: 9511
title: Upgrade to Rust 1.83
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/rust-1.83
created_at: 2024-11-28T17:41:42Z
updated_at: 2024-11-29T17:04:24Z
url: https://github.com/astral-sh/uv/pull/9511
synced_at: 2026-01-12T16:08:50Z
```

# Upgrade to Rust 1.83

---

_@charliermarsh_

## Summary

A lot of good new lints, and most importantly, error stabilizations. I tried to find a few usages of the new stabilizations, but I'm sure there are more.

IIUC, this _does_ require bumping our MSRV.

---

_Label `internal` added by @charliermarsh on 2024-11-28 17:41_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-28 18:05_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-28 18:05_

---

_Review requested from @konstin by @charliermarsh on 2024-11-28 18:05_

---

_Review comment by @konstin on `crates/uv-distribution/src/source/mod.rs`:1752 on 2024-11-29 10:55_

Yes!

---

_Review comment by @konstin on `crates/uv-python/src/discovery.rs`:1170 on 2024-11-29 10:56_

to me that reads worse

---

_@konstin approved on 2024-11-29 10:57_

---

_Merged by @charliermarsh on 2024-11-29 17:04_

---

_Closed by @charliermarsh on 2024-11-29 17:04_

---

_Branch deleted on 2024-11-29 17:04_

---
