```yaml
number: 12839
title: "Set 4MB stack size for all threads, introduce `UV_STACK_SIZE`"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - windows
assignees: []
merged: true
base: main
head: konsti/set-thread-size-for-rayon-and-tokio-too
created_at: 2025-04-11T16:05:43Z
updated_at: 2025-04-16T07:27:47Z
url: https://github.com/astral-sh/uv/pull/12839
synced_at: 2026-01-12T16:10:24Z
```

# Set 4MB stack size for all threads, introduce `UV_STACK_SIZE`

---

_@konstin_

See #12769 for the motivation. We set the 4MB not only for the main thread, but also for all tokio and rayon threads to fix a stack overflow while unpacking wheels in production on Windows.

There are two variables for setting the stack size: A new `UV_STACK_SIZE` that takes precedent, and the existing `RUST_MIN_STACK`. When setting the stack size, `UV_STACK_SIZE` should be preferred, since `RUST_MIN_STACK` affects all Rust applications, including build backends we call (e.g., maturin). The minimum stack size is set to 1MB, the lowest stack size we observed on a platform (Windows main thread).

Fixes #12769

## Test Plan

Tested manually with the example from #12769

---

_Label `bug` added by @konstin on 2025-04-11 16:05_

---

_Label `windows` added by @konstin on 2025-04-11 16:05_

---

_Review requested from @Gankra by @konstin on 2025-04-11 16:05_

---

_Review comment by @Gankra on `crates/uv-configuration/src/threading.rs`:32 on 2025-04-11 16:14_

the `unwrap_or(0).max(UV_MIN_STACK_DEFAULT)` was imo important for ensuring no errant/unrelated uses of RUST_MIN_STACK broke uv.

---

_@Gankra approved on 2025-04-11 16:19_

---

_@konstin reviewed on 2025-04-14 08:52_

---

_Review comment by @konstin on `crates/uv-configuration/src/threading.rs`:32 on 2025-04-14 08:52_

Doesn't `unwrap_or(0).max(UV_MIN_STACK_DEFAULT)` mean that we never allow to set a smaller stack size, even with `RUST_MIN_STACK`? My idea was that we allow `RUST_MIN_STACK` in both directions, so that we can both test the effect of lower values (is the bound too high?) and allow bumping it higher (for testing in cases https://github.com/astral-sh/uv/issues/12769 like where we need to bump it higher).

---

_@Gankra reviewed on 2025-04-14 12:58_

---

_Review comment by @Gankra on `crates/uv-configuration/src/threading.rs`:32 on 2025-04-14 12:58_

Yeah but at this point we "know" the current value is insufficient, so it's not safe to let people set it lower. And someone might set it in their system for another thing and call an accident? Maybe I'm being too paranoid.

---

_Renamed from "Set 4MB stack size for all threads" to "Set 4MB stack size for all threads, introduce `UV_STACK_SIZE`" by @konstin on 2025-04-14 13:55_

---

_@konstin reviewed on 2025-04-14 13:59_

---

_Review comment by @konstin on `crates/uv-configuration/src/threading.rs`:32 on 2025-04-14 13:59_

I put a 1MB limit in place and switched to `UV_STACK_SIZE`

---

_Merged by @konstin on 2025-04-16 07:27_

---

_Closed by @konstin on 2025-04-16 07:27_

---

_Branch deleted on 2025-04-16 07:27_

---
