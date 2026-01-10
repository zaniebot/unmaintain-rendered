```yaml
number: 911
title: Server panics when making changes while the workspace diagnostics request is in flight
type: issue
state: closed
author: MichaReiser
labels:
  - server
  - fatal
assignees: []
created_at: 2025-07-29T12:12:15Z
updated_at: 2025-07-30T15:40:43Z
url: https://github.com/astral-sh/ty/issues/911
synced_at: 2026-01-10T02:06:24Z
```

# Server panics when making changes while the workspace diagnostics request is in flight

---

_Issue opened by @MichaReiser on 2025-07-29 12:12_

### Summary

* Open a larger project (e.g. langchain)
* Wait for workspace diagnostics to start
* Make changes (make sure you hit save)


```
panicked at crates/ty_server/src/session.rs:532:44:
called `Option::unwrap()` on a `None` value
   0: std::backtrace_rs::backtrace::libunwind::trace
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/../../backtrace/src/backtrace/libunwind.rs:117:9
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/../../backtrace/src/backtrace/mod.rs:66:14
   2: std::backtrace::Backtrace::create
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/backtrace.rs:331:13
   3: ty_server::server::ServerPanicHookHandler::new::{{closure}}
             at /Users/micha/astral/ruff/crates/ty_server/src/server.rs:274:29
   4: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at /Users/micha/astral/ruff/crates/ruff_db/src/panic.rs:87:24
   5: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/alloc/src/boxed.rs:1980:9
   6: std::panicking::rust_panic_with_hook
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:841:13
   7: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:699:13
   8: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/sys/backtrace.rs:168:18
   9: __rustc::rust_begin_unwind
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:697:5
  10: core::panicking::panic_fmt
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/panicking.rs:75:14
  11: core::panicking::panic
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/panicking.rs:145:5
  12: core::option::unwrap_failed
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/option.rs:2040:5
  13: core::option::Option<T>::unwrap
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1003:21
  14: ty_server::session::Session::index_mut
             at /Users/micha/astral/ruff/crates/ty_server/src/session.rs:532:21
  15: ty_server::session::Session::update_text_document
             at /Users/micha/astral/ruff/crates/ty_server/src/session.rs:492:9
  16: <ty_server::server::api::notifications::did_change::DidChangeTextDocumentHandler as ty_server::server::api::traits::SyncNotificationHandler>::run
             at /Users/micha/astral/ruff/crates/ty_server/src/server/api/notifications/did_change.rs:39:9
  17: ty_server::server::api::sync_notification_task::{{closure}}
             at /Users/micha/astral/ruff/crates/ty_server/src/server/api.rs:351:27
  18: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  19: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed.rs:1966:9
  20: ty_server::server::schedule::Scheduler::dispatch
             at /Users/micha/astral/ruff/crates/ty_server/src/server/schedule.rs:52:17
  21: ty_server::server::main_loop::<impl ty_server::server::Server>::main_loop
             at /Users/micha/astral/ruff/crates/ty_server/src/server/main_loop.rs:102:21
  22: ty_server::server::Server::run::{{closure}}
             at /Users/micha/astral/ruff/crates/ty_server/src/server.rs:173:33
  23: ty_server::server::schedule::thread::Builder::spawn::{{closure}}
             at /Users/micha/astral/ruff/crates/ty_server/src/server/schedule/thread.rs:70:13
  24: std::sys::backtrace::__rust_begin_short_backtrace
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152:18
  25: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559:17
  26: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272:9
  27: std::panicking::try::do_call
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  28: ___rust_try
  29: std::panicking::try
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  30: std::panic::catch_unwind
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  31: std::thread::Builder::spawn_unchecked_::{{closure}}
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/mod.rs:557:30
  32: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  33: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/alloc/src/boxed.rs:1966:9
  34: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/alloc/src/boxed.rs:1966:9
  35: std::sys::pal::unix::thread::Thread::new::thread_start
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/sys/pal/unix/thread.rs:97:17
  36: __pthread_cond_wait

panicked at /Users/micha/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/jod-thread-1.0.0/src/lib.rs:33:22:
called `Result::unwrap()` on an `Err` value: Any { .. }
   0: std::backtrace_rs::backtrace::libunwind::trace
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/../../backtrace/src/backtrace/libunwind.rs:117:9
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/../../backtrace/src/backtrace/mod.rs:66:14
   2: std::backtrace::Backtrace::create
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/backtrace.rs:331:13
   3: ty_server::server::ServerPanicHookHandler::new::{{closure}}
             at /Users/micha/astral/ruff/crates/ty_server/src/server.rs:274:29
   4: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at /Users/micha/astral/ruff/crates/ruff_db/src/panic.rs:87:24
   5: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/alloc/src/boxed.rs:1980:9
   6: std::panicking::rust_panic_with_hook
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:841:13
   7: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:706:13
   8: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/sys/backtrace.rs:168:18
   9: __rustc::rust_begin_unwind
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:697:5
  10: core::panicking::panic_fmt
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/panicking.rs:75:14
  11: core::result::unwrap_failed
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/result.rs:1732:5
  12: core::result::Result<T,E>::unwrap
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/result.rs:1137:23
  13: jod_thread::JoinHandle<T>::join
             at /Users/micha/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/jod-thread-1.0.0/src/lib.rs:33:9
  14: ty_server::server::schedule::thread::JoinHandle<T>::join
             at /Users/micha/astral/ruff/crates/ty_server/src/server/schedule/thread.rs:89:9
  15: ty_server::server::Server::run
             at /Users/micha/astral/ruff/crates/ty_server/src/server.rs:173:9
  16: ty_server::run_server
             at /Users/micha/astral/ruff/crates/ty_server/src/lib.rs:50:25
  17: ty::run
             at /Users/micha/astral/ruff/crates/ty/src/lib.rs:44:28
  18: ty::main
             at /Users/micha/astral/ruff/crates/ty/src/main.rs:6:5
  19: core::ops::function::FnOnce::call_once
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  20: std::sys::backtrace::__rust_begin_short_backtrace
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152:18
  21: std::rt::lang_start::{{closure}}
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/rt.rs:199:18
  22: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/core/src/ops/function.rs:284:13
  23: std::panicking::try::do_call
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:589:40
  24: std::panicking::try
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:552:19
  25: std::panic::catch_unwind
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panic.rs:359:14
  26: std::rt::lang_start_internal::{{closure}}
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/rt.rs:168:24
  27: std::panicking::try::do_call
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:589:40
  28: std::panicking::try
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panicking.rs:552:19
  29: std::panic::catch_unwind
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/panic.rs:359:14
  30: std::rt::lang_start_internal
             at /rustc/6b00bc3880198600130e1cf62b8f8a93494488cc/library/std/src/rt.rs:164:5
  31: std::rt::lang_start
             at /Users/micha/.rustup/toolchains/1.88-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/rt.rs:198:5
  32: _main

```

I was mainly able to reproduce this with Zed using the following configuration:

```json
  "lsp": {
    "ty": {
      "binary": {
        "path": "/Users/micha/astral/ruff/target/debug/ty",
        "arguments": ["server"]
      },
      "initialization_options": {
        "settings": {
          "logLevel": "debug",
          "logFile": "/Users/micha/astral/ruff/target/ty_server.log",
          "diagnosticMode": "workspace"
        }
      },
      "settings": {
        "ty": {
          "diagnosticMode": "workspace"
        }
      }
    }
  }
```

### Version

_No response_

---

_Label `server` added by @MichaReiser on 2025-07-29 12:12_

---

_Label `fatal` added by @MichaReiser on 2025-07-29 12:12_

---

_Comment by @MichaReiser on 2025-07-30 08:28_

I believe the issue here is that `SessionSnapshot` has its own `Arc<Index>`

---

_Closed by @MichaReiser on 2025-07-30 15:40_

---
