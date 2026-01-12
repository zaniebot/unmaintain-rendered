```yaml
number: 17641
title: "[red-knot] Capture backtrace in \"check-failed\" diagnostic"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/capture-backtrace
created_at: 2025-04-26T10:14:05Z
updated_at: 2025-04-29T16:59:00Z
url: https://github.com/astral-sh/ruff/pull/17641
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Capture backtrace in "check-failed" diagnostic

---

_@MichaReiser_

## Summary

Capture and include the full backtrace in the "check-failed" diagnostic. This also prevents that the default panic handler prints to stdout. 

## Test Plan

```
error: panic: Panicked at /Users/micha/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/87bf6b6/src/function/fetch.rs:167:25 when checking `/Users/micha/astral/ecosystem/hydpy/hydpy/auxs/statstools.py`: `dependency graph cycle querying try_metaclass_(Id(23c4b)); set cycle_fn/cycle_initial to fixpoint iterate`
info: This indicates a bug in Red Knot.
info: If you could open an issue at https://github.com/astral-sh/ruff/issues/new?title=%5Bred-knot%5D:%20panic we'd be very appreciative!
info: Platform: macos aarch64
info: Args: ["../../ruff/target/debug/red_knot", "check", "--python", ".venv", "hydpy"]
info: Backtrace:
   0: std::backtrace_rs::backtrace::libunwind::trace
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/../../backtrace/src/backtrace/libunwind.rs:117:9
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/../../backtrace/src/backtrace/mod.rs:66:14
   2: std::backtrace::Backtrace::create
             at /rustc/05f9846f893b09a1be1fc8560e33fc3c815cfecb/library/std/src/backtrace.rs:331:13
   3: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at /Users/micha/astral/ruff/crates/ruff_db/src/panic.rs:59:34
   4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
...
```


---

_Renamed from "[red-knot] Capture backtrace" to "[red-knot] Capture backtrace in "check-failed" diagnostic" by @MichaReiser on 2025-04-26 10:14_

---

_Comment by @github-actions[bot] on 2025-04-26 10:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @MichaReiser on 2025-04-26 11:50_

---

_Marked ready for review by @MichaReiser on 2025-04-26 11:50_

---

_Review requested from @carljm by @MichaReiser on 2025-04-26 11:50_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-04-26 11:50_

---

_Review requested from @sharkdp by @MichaReiser on 2025-04-26 11:50_

---

_Review requested from @dcreager by @MichaReiser on 2025-04-26 11:50_

---

_Review comment by @sharkdp on `crates/ruff_db/src/panic.rs`:112 on 2025-04-28 07:31_

I'm not sure if I would understand this message without the context of this PR. Maybe something like the following?
```suggestion
    #[ignore = "super::catch_unwind installs a custom panic handler, which could effect test isolation"]
```

---

_@sharkdp approved on 2025-04-28 07:31_

Thank you.

---

_Review request for @carljm removed by @carljm on 2025-04-28 23:13_

---

_Merged by @MichaReiser on 2025-04-29 16:58_

---

_Closed by @MichaReiser on 2025-04-29 16:58_

---

_Branch deleted on 2025-04-29 16:59_

---
