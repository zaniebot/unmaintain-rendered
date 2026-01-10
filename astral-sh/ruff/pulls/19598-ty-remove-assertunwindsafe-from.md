```yaml
number: 19598
title: "[ty] Remove `AssertUnwindSafe` from `BackgroundRequestHandler` api"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/assert-unwind-background-api
created_at: 2025-07-28T13:17:07Z
updated_at: 2025-07-28T13:30:17Z
url: https://github.com/astral-sh/ruff/pull/19598
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Remove `AssertUnwindSafe` from `BackgroundRequestHandler` api

---

_Pull request opened by @MichaReiser on 2025-07-28 13:17_

## Summary

The use of `AssertUnwindSafe` is an implementation detail (as is that we use `std::panic::catch_unwind`). 

## Test Plan

`cargo build`


---

_Review requested from @carljm by @MichaReiser on 2025-07-28 13:17_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-28 13:17_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-28 13:17_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-28 13:17_

---

_Label `internal` added by @MichaReiser on 2025-07-28 13:17_

---

_Label `server` added by @MichaReiser on 2025-07-28 13:17_

---

_Label `ty` added by @MichaReiser on 2025-07-28 13:17_

---

_@AlexWaygood reviewed on 2025-07-28 13:25_

---

_Review comment by @AlexWaygood on `crates/ty_server/src/server/api.rs`:222 on 2025-07-28 13:25_

what does this do?

---

_Comment by @github-actions[bot] on 2025-07-28 13:25_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-28 13:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @MichaReiser on 2025-07-28 13:28_

---

_Closed by @MichaReiser on 2025-07-28 13:28_

---

_Branch deleted on 2025-07-28 13:28_

---

_@MichaReiser reviewed on 2025-07-28 13:30_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api.rs`:222 on 2025-07-28 13:30_

It tells Rust to move the entire `AssertUnwindSafe` variable. Otherwise, it's too clever and only captures `snapshot.0`, which then results in a compile error that `ProjectDatabase` isn't `UnwindSafe`... 


Using `snapshot` forces Rust to capture the entire `AssertUnwindSafe` as the function closure and we then extract `.0` before passing it to the `run` function (but that doesn't change what the function closure captures)

---
