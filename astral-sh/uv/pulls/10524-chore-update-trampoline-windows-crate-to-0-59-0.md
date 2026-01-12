```yaml
number: 10524
title: "chore: update trampoline windows crate to 0.59.0"
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: update-windows-crate
created_at: 2025-01-11T21:40:15Z
updated_at: 2025-01-14T15:49:49Z
url: https://github.com/astral-sh/uv/pull/10524
synced_at: 2026-01-12T16:09:21Z
```

# chore: update trampoline windows crate to 0.59.0

---

_@samypr100_

## Summary

* Closes https://github.com/astral-sh/uv/pull/10515
* Bumps Rust Nightly to 1.85 Beta
* Removes old dev dependencies

## Test Plan

Existing tests.
Note, binaries need to be rebuilt for integrity before merging.


---

_@samypr100 reviewed on 2025-01-11 21:43_

---

_Review comment by @samypr100 on `crates/uv-trampoline/src/bounce.rs`:496 on 2025-01-11 21:43_

Double Some() because of signature change, which is now `SetConsoleCtrlHandler(handlerroutine: Option<PHANDLER_ROUTINE>, add: bool)` where PHANDLER_ROUTINE is Option<fn(ctrltype: u32) -> super::super::Foundation::BOOL>

---

_Marked ready for review by @samypr100 on 2025-01-11 21:53_

---

_Review requested from @zanieb by @zanieb on 2025-01-11 22:44_

---

_@zanieb approved on 2025-01-14 14:54_

---

_Merged by @zanieb on 2025-01-14 14:54_

---

_Closed by @zanieb on 2025-01-14 14:54_

---

_Branch deleted on 2025-01-14 15:49_

---
