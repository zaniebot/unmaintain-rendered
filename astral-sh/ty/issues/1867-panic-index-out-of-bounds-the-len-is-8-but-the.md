```yaml
number: 1867
title: "panic: `index out of bounds: the len is 8 but the index is 16479`"
type: issue
state: open
author: correctmost
labels:
  - bug
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-12T14:52:25Z
updated_at: 2025-12-13T18:49:02Z
url: https://github.com/astral-sh/ty/issues/1867
synced_at: 2026-01-10T01:55:00Z
```

# panic: `index out of bounds: the len is 8 but the index is 16479`

---

_Issue opened by @correctmost on 2025-12-12 14:52_

### Summary

ty crashes when checking this fuzzed code:

```python
__next__ = next
@staticmethod
def __next__
```

```
error[panic]: Panicked at crates/ruff_db/src/parsed.rs:210:13 when checking `/home/user/a.py`: `index out of bounds: the len is 8 but the index is 16479`
```

### Version

5e31bc9 w/ astral-sh/ruff@bc8efa2fd86

---

_Label `bug` added by @AlexWaygood on 2025-12-12 14:54_

---

_Label `fatal` added by @AlexWaygood on 2025-12-12 14:54_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-12 14:54_

---

_Comment by @AlexWaygood on 2025-12-12 15:00_

If you use a debug build, we instead panic with an assertion failure, which makes it easier to see what the problem is:

```
error[panic]: Panicked at crates/ty_python_semantic/src/types/function.rs:341:18 when checking `/Users/alexw/dev/ruff/foo.py`: `assertion `left == right` failed
  left: File(System("/Users/alexw/dev/ruff/foo.py"))
 right: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi")))`
```

---

_Comment by @AlexWaygood on 2025-12-12 15:02_

full debug-build stacktrace:

<details>

```
error[panic]: Panicked at crates/ty_python_semantic/src/types/function.rs:341:18 when checking `/Users/alexw/dev/ruff/foo.py`: `assertion `left == right` failed
  left: File(System("/Users/alexw/dev/ruff/foo.py"))
 right: File(Vendored(VendoredPathBuf("stdlib/builtins.pyi")))`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: macos aarch64
info: Version: ruff/0.14.6+274 (ff0ed4e75 2025-12-12)
info: Args: ["target/debug/ty", "check", "foo.py"]
info: Backtrace:
   0: std::backtrace_rs::backtrace::libunwind::trace
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/std/src/../../backtrace/src/backtrace/libunwind.rs:117:9
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/std/src/../../backtrace/src/backtrace/mod.rs:66:14
   2: std::backtrace::Backtrace::create
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/std/src/backtrace.rs:331:13
   3: std::backtrace::Backtrace::capture
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/std/src/backtrace.rs:296:9
   4: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at ./crates/ruff_db/src/panic.rs:114:34
   5: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/alloc/src/boxed.rs:1999:9
   6: std::panicking::panic_with_hook
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/std/src/panicking.rs:842:13
   7: std::panicking::panic_handler::{{closure}}
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/std/src/panicking.rs:707:13
   8: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/std/src/sys/backtrace.rs:174:18
   9: __rustc::rust_begin_unwind
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/std/src/panicking.rs:698:5
  10: core::panicking::panic_fmt
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/core/src/panicking.rs:75:14
  11: core::panicking::assert_failed_inner
  12: core::panicking::assert_failed
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/panicking.rs:394:5
  13: ty_python_semantic::ast_node_ref::AstNodeRef<T>::node
             at ./crates/ty_python_semantic/src/ast_node_ref.rs:93:9
  14: ty_python_semantic::types::function::OverloadLiteral::focus_range
             at ./crates/ty_python_semantic/src/types/function.rs:341:18
  15: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::check_overloaded_functions
             at ./crates/ty_python_semantic/src/types/infer/builder.rs:1168:53
  16: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_scope
             at ./crates/ty_python_semantic/src/types/infer/builder.rs:577:18
  17: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region
             at ./crates/ty_python_semantic/src/types/infer/builder.rs:519:51
  18: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::finish_scope
             at ./crates/ty_python_semantic/src/types/infer/builder.rs:12584:14
  19: <ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/ty_python_semantic/src/types/infer.rs:80:82
  20: <ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_ as salsa::function::Configuration>::execute
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/components/salsa-macro-rules/src/setup_tracked_fn.rs:302:21
  21: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/execute.rs:556:25
  22: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_maybe_iterate
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/execute.rs:218:53
  23: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/execute.rs:112:53
  24: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/fetch.rs:179:14
  25: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/fetch.rs:65:34
  26: core::option::Option<T>::or_else
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1650:21
  27: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/fetch.rs:65:18
  28: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/fetch.rs:31:25
  29: ty_python_semantic::types::infer::infer_scope_types::{{closure}}
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/components/salsa-macro-rules/src/setup_tracked_fn.rs:479:72
  30: salsa::attach::Attached::attach
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/attach.rs:79:9
  31: salsa::attach::attach::{{closure}}
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/attach.rs:135:15
  32: std::thread::local::LocalKey<T>::try_with
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:315:12
  33: std::thread::local::LocalKey<T>::with
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:279:20
  34: salsa::attach::attach
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/attach.rs:133:14
  35: ty_python_semantic::types::infer::infer_scope_types
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/components/salsa-macro-rules/src/setup_tracked_fn.rs:469:13
  36: ty_python_semantic::types::check_types
             at ./crates/ty_python_semantic/src/types.rs:133:22
  37: <ty_project::check_file_impl::check_file_impl_Configuration_ as salsa::function::Configuration>::execute::inner_::{{closure}}
             at ./crates/ty_project/src/lib.rs:568:37
  38: std::panicking::catch_unwind::do_call
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:590:40
  39: ___rust_try
  40: std::panicking::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:553:19
  41: std::panic::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  42: salsa::cancelled::Cancelled::catch
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/cancelled.rs:35:15
  43: ty_project::catch::{{closure}}
             at ./crates/ty_project/src/lib.rs:678:9
  44: std::panicking::catch_unwind::do_call
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:590:40
  45: ___rust_try
  46: std::panicking::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:553:19
  47: std::panic::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  48: ruff_db::panic::catch_unwind
             at ./crates/ruff_db/src/panic.rs:145:18
  49: ty_project::catch
             at ./crates/ty_project/src/lib.rs:676:11
  50: <ty_project::check_file_impl::check_file_impl_Configuration_ as salsa::function::Configuration>::execute::inner_
             at ./crates/ty_project/src/lib.rs:568:15
  51: <ty_project::check_file_impl::check_file_impl_Configuration_ as salsa::function::Configuration>::execute
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/components/salsa-macro-rules/src/setup_tracked_fn.rs:302:21
  52: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute_query
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/execute.rs:556:25
  53: salsa::function::execute::<impl salsa::function::IngredientImpl<C>>::execute
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/execute.rs:60:49
  54: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch_cold
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/fetch.rs:179:14
  55: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo::{{closure}}
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/fetch.rs:65:34
  56: core::option::Option<T>::or_else
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/option.rs:1650:21
  57: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::refresh_memo
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/fetch.rs:65:18
  58: salsa::function::fetch::<impl salsa::function::IngredientImpl<C>>::fetch
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/function/fetch.rs:31:25
  59: ty_project::check_file_impl::{{closure}}
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/components/salsa-macro-rules/src/setup_tracked_fn.rs:479:72
  60: salsa::attach::Attached::attach
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/attach.rs:79:9
  61: salsa::attach::attach::{{closure}}
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/attach.rs:135:15
  62: std::thread::local::LocalKey<T>::try_with
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:315:12
  63: std::thread::local::LocalKey<T>::with
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/local.rs:279:20
  64: salsa::attach::attach
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/attach.rs:133:14
  65: ty_project::check_file_impl
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/components/salsa-macro-rules/src/setup_tracked_fn.rs:469:13
  66: ty_project::Project::check::{{closure}}::{{closure}}
             at ./crates/ty_project/src/lib.rs:297:31
  67: rayon_core::scope::Scope::spawn::{{closure}}::{{closure}}
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/scope/mod.rs:531:57
  68: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:274:9
  69: std::panicking::catch_unwind::do_call
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:590:40
  70: ___rust_try
  71: std::panicking::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:553:19
  72: std::panic::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  73: rayon_core::unwind::halt_unwinding
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/unwind.rs:17:5
  74: rayon_core::scope::ScopeBase::execute_job_closure
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/scope/mod.rs:693:28
  75: rayon_core::scope::ScopeBase::execute_job
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/scope/mod.rs:683:29
  76: rayon_core::scope::Scope::spawn::{{closure}}
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/scope/mod.rs:531:13
  77: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/job.rs:169:9
  78: rayon_core::job::JobRef::execute
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/job.rs:64:9
  79: rayon_core::registry::WorkerThread::execute
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:866:13
  80: rayon_core::registry::WorkerThread::wait_until_cold
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:792:22
  81: rayon_core::registry::WorkerThread::wait_until
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:775:18
  82: rayon_core::latch::CountLatch::wait
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/latch.rs:396:23
  83: rayon_core::scope::ScopeBase::complete
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/scope/mod.rs:672:34
  84: rayon_core::scope::scope::{{closure}}
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/scope/mod.rs:284:20
  85: rayon_core::registry::in_worker
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:957:13
  86: rayon_core::scope::scope
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/scope/mod.rs:282:5
  87: ty_project::Project::check
             at ./crates/ty_project/src/lib.rs:288:13
  88: ty_project::db::ProjectDatabase::check_with_reporter
             at ./crates/ty_project/src/db.rs:103:24
  89: ty::MainLoop::main_loop::{{closure}}::{{closure}}
             at ./crates/ty/src/lib.rs:291:32
  90: std::panicking::catch_unwind::do_call
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:590:40
  91: ___rust_try
  92: std::panicking::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:553:19
  93: std::panic::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  94: salsa::cancelled::Cancelled::catch
             at /Users/alexw/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/55e5e7d/src/cancelled.rs:35:15
  95: ty::MainLoop::main_loop::{{closure}}
             at ./crates/ty/src/lib.rs:290:31
  96: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:274:9
  97: std::panicking::catch_unwind::do_call
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:590:40
  98: ___rust_try
  99: std::panicking::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:553:19
 100: std::panic::catch_unwind
             at /Users/alexw/.rustup/toolchains/1.91-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
 101: rayon_core::unwind::halt_unwinding
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/unwind.rs:17:5
 102: rayon_core::registry::Registry::catch_unwind
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:376:27
 103: rayon_core::spawn::spawn_job::{{closure}}
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/spawn/mod.rs:95:22
 104: <rayon_core::job::HeapJob<BODY> as rayon_core::job::Job>::execute
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/job.rs:169:9
 105: rayon_core::job::JobRef::execute
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/job.rs:64:9
 106: rayon_core::registry::WorkerThread::execute
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:866:13
 107: rayon_core::registry::WorkerThread::wait_until_cold
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:800:26
 108: rayon_core::registry::WorkerThread::wait_until
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:775:18
 109: rayon_core::registry::WorkerThread::wait_until_out_of_work
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:824:14
 110: rayon_core::registry::main_loop
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:929:19
 111: rayon_core::registry::ThreadBuilder::run
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:50:18
 112: <rayon_core::registry::DefaultSpawn as rayon_core::registry::ThreadSpawn>::spawn::{{closure}}
             at /Users/alexw/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-core-1.13.0/src/registry.rs:95:27
 113: std::sys::backtrace::__rust_begin_short_backtrace
             at /Users/alexw/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:158:18
 114: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
             at /Users/alexw/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/mod.rs:559:17
 115: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /Users/alexw/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/panic/unwind_safe.rs:274:9
 116: std::panicking::catch_unwind::do_call
             at /Users/alexw/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:590:40
 117: ___rust_try
 118: std::panicking::catch_unwind
             at /Users/alexw/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panicking.rs:553:19
 119: std::panic::catch_unwind
             at /Users/alexw/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
 120: std::thread::Builder::spawn_unchecked_::{{closure}}
             at /Users/alexw/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/src/rust/library/std/src/thread/mod.rs:557:30
 121: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /Users/alexw/.rustup/toolchains/stable-aarch64-apple-darwin/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
 122: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/alloc/src/boxed.rs:1985:9
 123: std::sys::thread::unix::Thread::new::thread_start
             at /rustc/f8297e351a40c1439a467bbbb6879088047f50b3/library/std/src/sys/thread/unix.rs:126:17
 124: __pthread_cond_wait
```

</details>

---

_Added to milestone `Stable` by @carljm on 2025-12-12 15:53_

---

_Comment by @AlexWaygood on 2025-12-13 17:46_

This panic is sort-of wild.

After inferring all types for a given scope, we try to produce a list of all overloaded functions in that scope, and then we iterate over that list to ensure various correctness invariants are upheld ([ref](https://github.com/astral-sh/ruff/blob/82a7598aa8dc5cd2dde18e4da6e0e24b48dc7392/crates/ty_python_semantic/src/types/infer/builder.rs#L997-L1005)).

In this snippet, we correctly recognize the unfinished `__next__` function definition as an unfinished function definition... but when we try to answer the question "and what is the _type_ of the `__next__` function?" [here](https://github.com/astral-sh/ruff/blob/82a7598aa8dc5cd2dde18e4da6e0e24b48dc7392/crates/ty_python_semantic/src/types/infer/builder.rs#L1018), we return a `Type::FunctionLiteral` variant that refers to the definition of `builtins.next`! That means that we:
1. Incorrectly consider this unfinished `__next__` definition to be an overloaded function, so we start barelling down into the `check_overloaded_functions` method, and
2. From inside `check_overloaded_functions`, we call APIs in ways that are only safe if we can guarantee that the function we're looking at is defined in the file we're currently inferring types for ([ref](https://github.com/astral-sh/ruff/blob/82a7598aa8dc5cd2dde18e4da6e0e24b48dc7392/crates/ty_python_semantic/src/types/infer/builder.rs#L1167-L1171)). That "should" be safe because we "should" only ever check a function's overloads if that function was defined in the same file as the one we're currently inferring types for. But here, the `__next__` symbol incorrectly has a type associated with it that points to the `builtins.next` overloads, meaning that this API call is unsafe, and leading to the panic.

---

_Comment by @AlexWaygood on 2025-12-13 18:33_

> we return a `Type::FunctionLiteral` variant that refers to the definition of `builtins.next`!

To be more specific here: the `Type::FunctionLiteral` type returned iterates over both the `__next__` definition in the file we're checking _and_ the `next` overloads in `builtins.pyi` when we call `.iter_overloads_and_implementation()` on it. Applying this diff:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 1b78a02c32..7cb511d64d 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -1015,7 +1015,10 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                 if !matches!(definition.kind(self.db()), DefinitionKind::Function(_)) {
                     return None;
                 }
-                let function = ty.inner_type().as_function_literal()?;
+                let function = dbg!(ty.inner_type().as_function_literal()?);
+                for overload in function.iter_overloads_and_implementation(self.db()) {
+                    dbg!(overload);
+                }
                 if function.has_known_decorator(self.db(), FunctionDecorators::OVERLOAD) {
                     Some(definition.place(self.db()))
                 } else {
```

yields these debug prints when checking the MRE:

```
[crates/ty_python_semantic/src/types/infer/builder.rs:1018:32] ty.inner_type().as_function_literal()? = FunctionType {
    literal: FunctionLiteral {
        last_definition: OverloadLiteral {
            name: Name("__next__"),
            known: None,
            body_scope: ScopeId {
                [salsa id]: Id(1001),
                file: File {
                    path: System(
                        "/Users/alexw/dev/ruff/foo.py",
                    ),
                    status: Exists,
                    permissions: Some(
                        33188,
                    ),
                    revision: FileRevision(
                        32570502325268240380866310001,
                    ),
                },
                file_scope_id: FileScopeId(
                    1,
                ),
            },
            decorators: FunctionDecorators(
                STATICMETHOD,
            ),
            deprecated: None,
            dataclass_transformer_params: None,
        },
    },
    updated_signature: None,
    updated_last_definition_signature: None,
}
[crates/ty_python_semantic/src/types/infer/builder.rs:1020:21] overload = OverloadLiteral {
    name: Name("__next__"),
    known: None,
    body_scope: ScopeId {
        [salsa id]: Id(1001),
        file: File {
            path: System(
                "/Users/alexw/dev/ruff/foo.py",
            ),
            status: Exists,
            permissions: Some(
                33188,
            ),
            revision: FileRevision(
                32570502325268240380866310001,
            ),
        },
        file_scope_id: FileScopeId(
            1,
        ),
    },
    decorators: FunctionDecorators(
        STATICMETHOD,
    ),
    deprecated: None,
    dataclass_transformer_params: None,
}
[crates/ty_python_semantic/src/types/infer/builder.rs:1020:21] overload = OverloadLiteral {
    name: Name("next"),
    known: None,
    body_scope: ScopeId {
        [salsa id]: Id(5497),
        file: File {
            path: Vendored(
                VendoredPathBuf(
                    "stdlib/builtins.pyi",
                ),
            ),
            status: Exists,
            permissions: Some(
                292,
            ),
            revision: FileRevision(
                1782718066,
            ),
        },
        file_scope_id: FileScopeId(
            794,
        ),
    },
    decorators: FunctionDecorators(
        OVERLOAD,
    ),
    deprecated: None,
    dataclass_transformer_params: None,
}
[crates/ty_python_semantic/src/types/infer/builder.rs:1020:21] overload = OverloadLiteral {
    name: Name("next"),
    known: None,
    body_scope: ScopeId {
        [salsa id]: Id(5498),
        file: File {
            path: Vendored(
                VendoredPathBuf(
                    "stdlib/builtins.pyi",
                ),
            ),
            status: Exists,
            permissions: Some(
                292,
            ),
            revision: FileRevision(
                1782718066,
            ),
        },
        file_scope_id: FileScopeId(
            795,
        ),
    },
    decorators: FunctionDecorators(
        OVERLOAD,
    ),
    deprecated: None,
    dataclass_transformer_params: None,
}
```

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-12-13 18:49_

---
