```yaml
number: 16342
title: Unexpected server panic when source contain different newline characters
type: issue
state: open
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
created_at: 2025-02-24T08:46:07Z
updated_at: 2025-02-24T08:46:07Z
url: https://github.com/astral-sh/ruff/issues/16342
synced_at: 2026-01-12T15:54:55Z
```

# Unexpected server panic when source contain different newline characters

---

_@dhruvmanila_

### Description

Tested on `0.9.7` and on 5eaf225fc3baf738493ad052575d544e565d74d3

Noticed it when looking at https://github.com/astral-sh/ruff/issues/16318

Using the source code:
```console
$ printf '"\\r".strip("""\r\n\n\r\r""")' > test.py
```

I couldn't figure out how to reproduce this in VS Code because it normalizes all newline characters but am able to reproduce this consistently in Neovim where it retains the characters.

1. Open Neovim with `test.py` content
2. At the end, add a newline

The (2) above gives the below `didChange` request:
```
[DEBUG][2025-02-24 14:07:08] .../vim/lsp/rpc.lua:277	"rpc.send"	{
  jsonrpc = "2.0",
  method = "textDocument/didChange",
  params = {
    contentChanges = { {
        range = {
          ["end"] = {
            character = 0,
            line = 3
          },
          start = {
            character = 6,
            line = 2
          }
        },
        rangeLength = 1,
        text = "\n\n"
      } },
    textDocument = {
      uri = "file:///Users/dhruv/playground/ruff/parser/_.py",
      version = 3
    }
  }
}
```

<details><summary>Panic trace:</summary>
<p>

```
   1.314090834s ERROR     ruff:main ruff_server::server: panicked at crates/ruff_server/src/edit/range.rs:90:9:
assertion failed: start.raw <= end.raw
   0: std::backtrace_rs::backtrace::libunwind::trace
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/../../backtrace/src/backtrace/libunwind.rs:116:5
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/../../backtrace/src/backtrace/mod.rs:66:5
   2: std::backtrace::Backtrace::create
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/backtrace.rs:331:13
   3: ruff_server::server::Server::run::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server.rs:145:29
   4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/alloc/src/boxed.rs:1986:9
   5: std::panicking::rust_panic_with_hook
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:809:13
   6: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:667:13
   7: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/sys/backtrace.rs:170:18
   8: rust_begin_unwind
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:665:5
   9: core::panicking::panic_fmt
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/core/src/panicking.rs:76:14
  10: core::panicking::panic
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/core/src/panicking.rs:148:5
  11: ruff_text_size::range::TextRange::new
             at /Users/dhruv/work/astral/ruff/crates/ruff_text_size/src/range.rs:49:9
  12: <lsp_types::Range as ruff_server::edit::range::RangeExt>::to_text_range
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/edit/range.rs:90:9
  13: ruff_server::edit::text_document::TextDocument::apply_changes
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/edit/text_document.rs:107:29
  14: ruff_server::session::index::Index::update_text_document
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/session/index.rs:118:9
  15: ruff_server::session::Session::update_text_document
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/session.rs:96:9
  16: <ruff_server::server::api::notifications::did_change::DidChange as ruff_server::server::api::traits::SyncNotificationHandler>::run
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server/api/notifications/did_change.rs:32:9
  17: ruff_server::server::api::local_notification_task::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server/api.rs:143:27
  18: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  19: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/alloc/src/boxed.rs:1972:9
  20: ruff_server::server::schedule::Scheduler::dispatch
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server/schedule.rs:83:17
  21: ruff_server::server::Server::event_loop
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server.rs:195:13
  22: ruff_server::server::Server::run::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server.rs:163:13
  23: ruff_server::server::schedule::thread::Builder::spawn::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server/schedule/thread.rs:70:13
  24: std::sys::backtrace::__rust_begin_short_backtrace
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:154:18
  25: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/mod.rs:561:17
  26: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272:9
  27: std::panicking::try::do_call
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:557:40
  28: ___rust_try
  29: std::panicking::try
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:520:19
  30: std::panic::catch_unwind
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  31: std::thread::Builder::spawn_unchecked_::{{closure}}
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559:30
  32: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  33: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/alloc/src/boxed.rs:1972:9
  34: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/alloc/src/boxed.rs:1972:9
  35: std::sys::pal::unix::thread::Thread::new::thread_start
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/sys/pal/unix/thread.rs:105:17
  36: __pthread_deallocate

   1.363861417s ERROR          main ruff_server::server: panicked at /Users/dhruv/.cargo/registry/src/index.crates.io-6f17d22bba15001f/jod-thread-0.1.2/src/lib.rs:33:22:
called `Result::unwrap()` on an `Err` value: Any { .. }
   0: std::backtrace_rs::backtrace::libunwind::trace
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/../../backtrace/src/backtrace/libunwind.rs:116:5
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/../../backtrace/src/backtrace/mod.rs:66:5
   2: std::backtrace::Backtrace::create
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/backtrace.rs:331:13
   3: ruff_server::server::Server::run::{{closure}}
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server.rs:145:29
   4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/alloc/src/boxed.rs:1986:9
   5: std::panicking::rust_panic_with_hook
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:809:13
   6: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:674:13
   7: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/sys/backtrace.rs:170:18
   8: rust_begin_unwind
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:665:5
   9: core::panicking::panic_fmt
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/core/src/panicking.rs:76:14
  10: core::result::unwrap_failed
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/core/src/result.rs:1699:5
  11: core::result::Result<T,E>::unwrap
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/result.rs:1104:23
  12: jod_thread::JoinHandle<T>::join
             at /Users/dhruv/.cargo/registry/src/index.crates.io-6f17d22bba15001f/jod-thread-0.1.2/src/lib.rs:33:9
  13: ruff_server::server::schedule::thread::JoinHandle<T>::join
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server/schedule/thread.rs:89:9
  14: ruff_server::server::Server::run
             at /Users/dhruv/work/astral/ruff/crates/ruff_server/src/server.rs:162:9
  15: ruff::commands::server::run_server
             at /Users/dhruv/work/astral/ruff/crates/ruff/src/commands/server.rs:13:5
  16: ruff::server
             at /Users/dhruv/work/astral/ruff/crates/ruff/src/lib.rs:228:5
  17: ruff::run
             at /Users/dhruv/work/astral/ruff/crates/ruff/src/lib.rs:197:34
  18: ruff::main
             at /Users/dhruv/work/astral/ruff/crates/ruff/src/main.rs:44:11
  19: core::ops::function::FnOnce::call_once
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  20: std::sys::backtrace::__rust_begin_short_backtrace
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:154:18
  21: std::rt::lang_start::{{closure}}
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/rt.rs:195:18
  22: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/core/src/ops/function.rs:284:13
  23: std::panicking::try::do_call
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:557:40
  24: std::panicking::try
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:520:19
  25: std::panic::catch_unwind
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panic.rs:358:14
  26: std::rt::lang_start_internal::{{closure}}
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/rt.rs:174:48
  27: std::panicking::try::do_call
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:557:40
  28: std::panicking::try
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panicking.rs:520:19
  29: std::panic::catch_unwind
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/panic.rs:358:14
  30: std::rt::lang_start_internal
             at /rustc/e71f9a9a98b0faf423844bf0ba7438f29dc27d58/library/std/src/rt.rs:174:20
  31: std::rt::lang_start
             at /Users/dhruv/.rustup/toolchains/1.84-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/rt.rs:194:17
  32: _main
```

</p>
</details> 

---

_Label `bug` added by @dhruvmanila on 2025-02-24 08:46_

---

_Label `server` added by @dhruvmanila on 2025-02-24 08:46_

---
