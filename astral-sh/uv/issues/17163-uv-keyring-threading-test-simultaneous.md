```yaml
number: 17163
title: "`uv-keyring::threading::test_simultaneous_independent_create_set` flakes on Linux with \"can't get ascii password\""
type: issue
state: open
author: EliteTK
labels:
  - ci-flake
assignees: []
created_at: 2025-12-17T15:57:21Z
updated_at: 2025-12-18T10:46:33Z
url: https://github.com/astral-sh/uv/issues/17163
synced_at: 2026-01-12T16:02:44Z
```

# `uv-keyring::threading::test_simultaneous_independent_create_set` flakes on Linux with "can't get ascii password"

---

_@EliteTK_

Randomly failed here: https://github.com/astral-sh/uv/actions/runs/20305612646/job/58322152302?pr=17088

Seems unrelated to the changes.

Here's a follow up run with some more unrelated changes, the same test succeeded: https://github.com/astral-sh/uv/actions/runs/20308222303/job/58332149552?pr=17088

Haven't investigated further.

---

_Label `testing` added by @EliteTK on 2025-12-17 15:57_

---

_Label `testing` removed by @konstin on 2025-12-17 16:24_

---

_Label `ci-flake` added by @konstin on 2025-12-17 16:24_

---

_Renamed from "Flaky test: `uv-keyring::threading` `test_simultaneous_independent_create_set`" to "`uv-keyring::threading::test_simultaneous_independent_create_set` flakes on Linux with "can't get ascii password"" by @zanieb on 2025-12-18 04:17_

---

_Comment by @zanieb on 2025-12-18 04:17_

```
        FAIL [   0.137s] uv-keyring::threading test_simultaneous_independent_create_set
  stdout ───

    running 1 test
    test test_simultaneous_independent_create_set ... FAILED

    failures:

    failures:
        test_simultaneous_independent_create_set

    test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 4 filtered out; finished in 0.13s
    
  stderr ───

    thread 'test_simultaneous_independent_create_set' (129699) panicked at crates/uv-keyring/tests/threading.rs:185:18:
    Can't get ascii password: PlatformFailure(Zbus(MethodError(OwnedErrorName("org.freedesktop.DBus.Error.NoReply"), Some("Message recipient disconnected from message bus without replying"), Msg { type: Error, serial: 3, sender: UniqueName("org.freedesktop.DBus"), reply-serial: 116, body: Str, fds: [] })))
    stack backtrace:
       0: __rustc::rust_begin_unwind
       1: core::panicking::panic_fmt
       2: core::result::unwrap_failed
       3: tokio::runtime::task::core::Core<T,S>::poll
       4: tokio::runtime::task::harness::Harness<T,S>::poll
       5: tokio::runtime::scheduler::current_thread::Context::run_task
       6: tokio::runtime::context::scoped::Scoped<T>::set
       7: std::thread::local::LocalKey<T>::with
       8: tokio::runtime::scheduler::current_thread::CoreGuard::block_on
       9: tokio::runtime::context::runtime::enter_runtime
      10: tokio::runtime::runtime::Runtime::block_on
      11: core::ops::function::FnOnce::call_once
    note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.

    thread 'test_simultaneous_independent_create_set' (129699) panicked at crates/uv-keyring/tests/threading.rs:203:22:
    Task failed: JoinError::Panic(Id(1), "Can't get ascii password: PlatformFailure(Zbus(MethodError(OwnedErrorName(\"org.freedesktop.DBus.Error.NoReply\"), Some(\"Message recipient disconnected from message bus without replying\"), Msg { type: Error, serial: 3, sender: UniqueName(\"org.freedesktop.DBus\"), reply-serial: 116, body: Str, fds: [] })))", ...)
    stack backtrace:
       0: __rustc::rust_begin_unwind
       1: core::panicking::panic_fmt
       2: core::result::unwrap_failed
       3: threading::test_simultaneous_independent_create_set::{{closure}}
       4: tokio::runtime::scheduler::current_thread::Context::enter
       5: tokio::runtime::context::scoped::Scoped<T>::set
       6: std::thread::local::LocalKey<T>::with
       7: tokio::runtime::scheduler::current_thread::CoreGuard::block_on
       8: tokio::runtime::context::runtime::enter_runtime
       9: tokio::runtime::runtime::Runtime::block_on
      10: core::ops::function::FnOnce::call_once
    note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace
```

---

_Comment by @konstin on 2025-12-18 10:46_

Another one: https://github.com/astral-sh/uv/actions/runs/20333424890/job/58414030678?pr=17104

---
