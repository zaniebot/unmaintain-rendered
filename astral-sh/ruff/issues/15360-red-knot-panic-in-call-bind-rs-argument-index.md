```yaml
number: 15360
title: "[red-knot] Panic in `call/bind.rs` (argument index should not be out of range)"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - ty
assignees: []
created_at: 2025-01-08T21:08:53Z
updated_at: 2025-01-09T08:36:50Z
url: https://github.com/astral-sh/ruff/issues/15360
synced_at: 2026-01-10T11:09:56Z
```

# [red-knot] Panic in `call/bind.rs` (argument index should not be out of range)

---

_Issue opened by @sharkdp on 2025-01-08 21:08_

We currently panic when running `red_knot` on the `black` codebase:

<details>

<summary>Click for full backtrace

```
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/types/call/bind.rs:384:22:
argument index should not be out of range
```

</summary>

```
thread '<unnamed>' panicked at crates/red_knot_python_semantic/src/types/call/bind.rs:384:22:
argument index should not be out of range
stack backtrace:
   0: rust_begin_unwind
             at /rustc/90b35a6239c3d8bdabc530a6a0816f7ff89a0aaf/library/std/src/panicking.rs:665:5
   1: core::panicking::panic_fmt
             at /rustc/90b35a6239c3d8bdabc530a6a0816f7ff89a0aaf/library/core/src/panicking.rs:74:14
   2: core::panicking::panic_display
             at /rustc/90b35a6239c3d8bdabc530a6a0816f7ff89a0aaf/library/core/src/panicking.rs:264:5
   3: core::option::expect_failed
             at /rustc/90b35a6239c3d8bdabc530a6a0816f7ff89a0aaf/library/core/src/option.rs:2021:5
   4: core::option::Option<T>::expect
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:933:21
   5: red_knot_python_semantic::types::call::bind::CallBindingError::get_node
             at ./crates/red_knot_python_semantic/src/types/call/bind.rs:380:23
   6: red_knot_python_semantic::types::call::bind::CallBindingError::report_diagnostic
             at ./crates/red_knot_python_semantic/src/types/call/bind.rs:361:21
   7: red_knot_python_semantic::types::call::bind::CallBinding::report_diagnostics
             at ./crates/red_knot_python_semantic/src/types/call/bind.rs:182:13
   8: red_knot_python_semantic::types::call::CallOutcome::return_ty_result
             at ./crates/red_knot_python_semantic/src/types/call.rs:191:17
   9: red_knot_python_semantic::types::call::CallOutcome::unwrap_with_diagnostic
             at ./crates/red_knot_python_semantic/src/types/call.rs:115:15
  10: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_call_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:3088:9
  11: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression_impl
             at ./crates/red_knot_python_semantic/src/types/infer.rs:2624:49
  12: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_expression
             at ./crates/red_knot_python_semantic/src/types/infer.rs:2584:9
  13: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
             at ./crates/red_knot_python_semantic/src/types/infer.rs:978:17
  14: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_body
             at ./crates/red_knot_python_semantic/src/types/infer.rs:969:13
  15: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_with_statement
             at ./crates/red_knot_python_semantic/src/types/infer.rs:1450:9
  16: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_statement
             at ./crates/red_knot_python_semantic/src/types/infer.rs:982:48
  17: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_body
             at ./crates/red_knot_python_semantic/src/types/infer.rs:969:13
  18: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_function_body
             at ./crates/red_knot_python_semantic/src/types/infer.rs:964:9
  19: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region_scope
             at ./crates/red_knot_python_semantic/src/types/infer.rs:492:54
  20: red_knot_python_semantic::types::infer::TypeInferenceBuilder::infer_region
             at ./crates/red_knot_python_semantic/src/types/infer.rs:478:46
  21: red_knot_python_semantic::types::infer::TypeInferenceBuilder::finish
             at ./crates/red_knot_python_semantic/src/types/infer.rs:4609:9
  22: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types/infer.rs:101:5
  23: <red_knot_python_semantic::types::infer::infer_scope_types::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/components/salsa-macro-rules/src/setup_tracked_fn.rs:177:21
  24: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/execute.rs:51:43
  25: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272:9
  26: std::panicking::try::do_call
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:557:40
  27: __rust_try
  28: std::panicking::try
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:520:19
  29: std::panic::catch_unwind
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  30: salsa::cycle::Cycle::catch
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/cycle.rs:43:15
  31: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/execute.rs:51:27
  32: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/fetch.rs:91:14
  33: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/fetch.rs:41:67
  34: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1546:21
  35: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/fetch.rs:41:33
  36: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/fetch.rs:13:20
  37: red_knot_python_semantic::types::infer::infer_scope_types::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/components/salsa-macro-rules/src/setup_tracked_fn.rs:292:25
  38: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/attach.rs:71:9
  39: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/attach.rs:91:23
  40: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:283:12
  41: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:260:9
  42: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/attach.rs:91:5
  43: red_knot_python_semantic::types::infer::infer_scope_types
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/components/salsa-macro-rules/src/setup_tracked_fn.rs:284:13
  44: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/red_knot_python_semantic/src/types.rs:71:22
  45: <red_knot_python_semantic::types::check_types::Configuration_ as salsa::function::Configuration>::execute
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/components/salsa-macro-rules/src/setup_tracked_fn.rs:177:21
  46: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/execute.rs:51:43
  47: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272:9
  48: std::panicking::try::do_call
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:557:40
  49: __rust_try
  50: std::panicking::try
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:520:19
  51: std::panic::catch_unwind
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  52: salsa::cycle::Cycle::catch
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/cycle.rs:43:15
  53: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/execute.rs:51:27
  54: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/fetch.rs:91:14
  55: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/fetch.rs:41:67
  56: core::option::Option<T>::or_else
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:1546:21
  57: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/fetch.rs:41:33
  58: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/function/fetch.rs:13:20
  59: red_knot_python_semantic::types::check_types::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/components/salsa-macro-rules/src/setup_tracked_fn.rs:292:25
  60: salsa::attach::Attached::attach
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/attach.rs:71:9
  61: salsa::attach::attach::{{closure}}
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/attach.rs:91:23
  62: std::thread::local::LocalKey<T>::try_with
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:283:12
  63: std::thread::local::LocalKey<T>::with
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/thread/local.rs:260:9
  64: salsa::attach::attach
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/src/attach.rs:91:5
  65: red_knot_python_semantic::types::check_types
             at /home/shark/.cargo/git/checkouts/salsa-61760caba2b17ca5/88a1d77/components/salsa-macro-rules/src/setup_tracked_fn.rs:284:13
  66: red_knot_workspace::workspace::check_file
             at ./crates/red_knot_workspace/src/workspace.rs:398:24
  67: red_knot_workspace::workspace::Workspace::check::{{closure}}::{{closure}}
             at ./crates/red_knot_workspace/src/workspace.rs:211:44
  68: rayon_core::scope::Scope::spawn::{{closure}}::{{closure}}
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:526:57
  69: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:272:9
  70: std::panicking::try::do_call
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:557:40
  71: __rust_try
  72: std::panicking::try
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:520:19
  73: std::panic::catch_unwind
             at /home/shark/.rustup/toolchains/1.83-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  74: rayon_core::unwind::halt_unwinding
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/unwind.rs:17:5
  75: rayon_core::scope::ScopeBase::execute_job_closure
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:689:28
  76: rayon_core::scope::ScopeBase::execute_job
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:679:29
  77: rayon_core::scope::Scope::spawn::{{closure}}
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/scope/mod.rs:526:13
  78: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/job.rs:169:9
  79: rayon_core::job::JobRef::execute
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/job.rs:64:9
  80: rayon_core::registry::WorkerThread::execute
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:860:9
  81: rayon_core::registry::WorkerThread::wait_until_cold
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:794:21
  82: rayon_core::registry::WorkerThread::wait_until
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:769:13
  83: rayon_core::registry::WorkerThread::wait_until_out_of_work
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:818:9
  84: rayon_core::registry::main_loop
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:923:5
  85: rayon_core::registry::ThreadBuilder::run
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:53:18
  86: <rayon_core::registry::DefaultSpawn as rayon_core::registry::ThreadSpawn>::spawn::{{closure}}
             at /home/shark/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.12.1/src/registry.rs:98:20
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
Rayon: detected unexpected panic; aborting
[1]    4125360 IOT instruction (core dumped)  RUST_BACKTRACE=1 cargo run --bin red_knot -- --project ~/black
```


</details>

---

_Label `bug` added by @sharkdp on 2025-01-08 21:08_

---

_Label `red-knot` added by @sharkdp on 2025-01-08 21:08_

---

_Assigned to @carljm by @carljm on 2025-01-08 21:13_

---

_Closed by @carljm on 2025-01-09 08:36_

---
