```yaml
number: 12610
title: Make server panic hook more error resilient
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: server-panic-hook-error-resilience
created_at: 2024-08-01T11:14:38Z
updated_at: 2024-08-02T14:54:56Z
url: https://github.com/astral-sh/ruff/pull/12610
synced_at: 2026-01-12T15:55:41Z
```

# Make server panic hook more error resilient

---

_@MichaReiser_

## Summary

`eprintln` can panic when the stderr pipe is broken which is very unfortunate if it happens in the server's panic hook.

This PR uses `writeln` with `.ok()` as a more forgiving way to write the error message. 

This should fix https://github.com/astral-sh/ruff/issues/12545 although it is still unclear to me how to get into the broken pipe case. 

## Test Plan

I added a panic in the diagnostics request:

* NeoVim shows an error that the ruff server panicked
* The log contains the panic
  ```
	  0.008529319s ERROR ruff:worker:0 ruff_server::message: panicked at crates/ruff_server/src/server/api/requests/diagnostic.rs:23:9:
	no diagnostics for you
	   0: ruff_server::message::init_messenger::{{closure}}
	   1: std::panicking::rust_panic_with_hook
	   2: std::panicking::begin_panic_handler::{{closure}}
	   3: std::sys_common::backtrace::__rust_end_short_backtrace
	   4: rust_begin_unwind
	   5: core::panicking::panic_fmt
	   6: <ruff_server::server::api::requests::diagnostic::DocumentDiagnostic as ruff_server::server::api::traits::BackgroundDocumentRequestHandler>::run_with_snapshot
	   7: core::ops::function::FnOnce::call_once{{vtable.shim}}
	   8: core::ops::function::FnOnce::call_once{{vtable.shim}}
	   9: std::sys_common::backtrace::__rust_begin_short_backtrace
	  10: core::ops::function::FnOnce::call_once{{vtable.shim}}
	  11: std::sys::pal::unix::thread::Thread::new::thread_start
	  12: <unknown>
	  13: <unknown>
  ```


---

_Renamed from "Make ruff-server panic hook more error resilient" to "Make server panic hook more error resilient" by @MichaReiser on 2024-08-01 11:15_

---

_Label `bug` added by @MichaReiser on 2024-08-01 11:15_

---

_Label `server` added by @MichaReiser on 2024-08-01 11:15_

---

_Marked ready for review by @MichaReiser on 2024-08-01 11:18_

---

_@MichaReiser reviewed on 2024-08-01 11:29_

---

_Review comment by @MichaReiser on `crates/ruff/src/main.rs`:97 on 2024-08-01 11:29_

Looking at the original trace, I strongly suspect that ruff panics here... when trying to print the reason why ruff errored:

```rust
failed printing to stderr: Broken pipe (os error 32)
   0: ruff_server::message::init_messenger::{{closure}}
   1: std::panicking::rust_panic_with_hook
   2: std::panicking::begin_panic_handler::{{closure}}
   3: std::sys_common::backtrace::__rust_end_short_backtrace
   4: rust_begin_unwind
   5: core::panicking::panic_fmt
   6: std::io::stdio::_eprint
   7: ruff::main
   8: std::sys_common::backtrace::__rust_begin_short_backtrace
   9: std::rt::lang_start::{{closure}}
  10: std::rt::lang_start_internal
  11: main
  12: <unknown>
  13: __libc_start_main
  14: <unknown>
```



---

_Comment by @github-actions[bot] on 2024-08-01 11:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dhruvmanila by @MichaReiser on 2024-08-01 21:35_

---

_@MichaReiser reviewed on 2024-08-02 09:55_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/server.rs`:167 on 2024-08-02 09:55_

The main change is to use `writeln` here

---

_@MichaReiser reviewed on 2024-08-02 09:56_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/message.rs`:42 on 2024-08-02 09:56_

I decided to move this out of Messenger to ensure that we unset the panic hook when the server shuts down. We may consider using `catch_unwind` on a per request/response model in the future. But I think that's good enough for now

---

_@dhruvmanila approved on 2024-08-02 09:57_

This seems reasonable although I've never faced this panic.

---

_Merged by @MichaReiser on 2024-08-02 10:10_

---

_Closed by @MichaReiser on 2024-08-02 10:10_

---

_Branch deleted on 2024-08-02 10:10_

---
