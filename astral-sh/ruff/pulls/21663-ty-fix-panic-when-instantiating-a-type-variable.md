```yaml
number: 21663
title: "[ty] fix panic when instantiating a type variable with invalid constraints"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: unknown-typevar-constraints
created_at: 2025-11-27T15:53:10Z
updated_at: 2025-12-05T03:52:14Z
url: https://github.com/astral-sh/ruff/pull/21663
synced_at: 2026-01-12T15:57:30Z
```

# [ty] fix panic when instantiating a type variable with invalid constraints

---

_@mtshiba_

## Summary

This PR fixes a panic that occurs with code like this:

```python
class C[T: (A, B)]:
    def f(foo: T):
        try:
            pass
        except foo:
            pass
```

The panic occurs because the constraints of `T` are inferred to be `Unknown | Unknown`.
Calling `to_instance` on such a type results in `Unknown`, but the panic occurs because contexts expecting constraints on type variables only accept union types.
A fundamental solution to this problem would be to define a new struct `TypeVarConstraints` to hold each constraint individually, rather than trying to store everything in a `UnionType`. There may be additional cost for converting between this and a union type. I'll ask codspeed if this is actually costly.

<details>
<summary>Panic log</summary>

```
error[panic]: Panicked at crates\ty_python_semantic\src\types\infer\builder.rs:3127:44 when checking `189994.py`: ``Type::to_instance()` should always return `Some()` if called on a type assignable to `type[BaseException]``
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: windows x86_64
info: Version: ruff/0.14.6+68 (7c7f8d1a1 2025-11-27)
info: Args: ["GitHub\\ruff\\target\\debug\\ty.exe", "check", "189994.py"]
info: Backtrace:
   0: std::backtrace_rs::backtrace::win64::trace
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\std\src\..\..\backtrace\src\backtrace\win64.rs:85
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\std\src\..\..\backtrace\src\backtrace\mod.rs:66
   2: std::backtrace::Backtrace::create
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\std\src\backtrace.rs:331
   3: std::backtrace::Backtrace::capture
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\std\src\backtrace.rs:296
   4: ruff_db::panic::install_hook::closure$0::closure$0
             at GitHub\ruff\crates\ruff_db\src\panic.rs:114
   5: std::panicking::panic_with_hook
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\std\src\panicking.rs:842
   6: std::panicking::panic_handler::closure$0
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\std\src\panicking.rs:707
   7: std::sys::backtrace::__rust_end_short_backtrace<std::panicking::panic_handler::closure_env$0,never$>
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\std\src\sys\backtrace.rs:174
   8: std::panicking::panic_handler
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\std\src\panicking.rs:698
   9: core::panicking::panic_fmt
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\core\src\panicking.rs:75
  10: core::panicking::panic_display
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\core\src\panicking.rs:259
  11: core::option::expect_failed
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\core\src\option.rs:2178
  12: enum2$<core::option::Option<enum2$<ty_python_semantic::types::Type> > >::expect<enum2$<ty_python_semantic::types::Type> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:965
  13: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_exception
             at GitHub\ruff\crates\ty_python_semantic\src\types\infer\builder.rs:3127
  14: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_try_statement
             at GitHub\ruff\crates\ty_python_semantic\src\types\infer\builder.rs:2976
  15: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_statement
             at GitHub\ruff\crates\ty_python_semantic\src\types\infer\builder.rs:2180
  16: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_body
             at GitHub\ruff\crates\ty_python_semantic\src\types\infer\builder.rs:2162
  17: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_function_body
             at GitHub\ruff\crates\ty_python_semantic\src\types\infer\builder.rs:2035
  18: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region_scope
             at GitHub\ruff\crates\ty_python_semantic\src\types\infer\builder.rs:533
  19: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::infer_region
             at GitHub\ruff\crates\ty_python_semantic\src\types\infer\builder.rs:517
  20: ty_python_semantic::types::infer::builder::TypeInferenceBuilder::finish_scope
             at GitHub\ruff\crates\ty_python_semantic\src\types\infer\builder.rs:12031
  21: ty_python_semantic::types::infer::infer_scope_types::impl$2::execute::inner_
             at GitHub\ruff\crates\ty_python_semantic\src\types\infer.rs:79
  22: ty_python_semantic::types::infer::infer_scope_types::impl$2::execute
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\components\salsa-macro-rules\src\setup_tracked_fn.rs:302
  23: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::execute_query<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\execute.rs:556
  24: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::execute_maybe_iterate<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\execute.rs:218
  25: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::execute<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\execute.rs:112
  26: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::fetch_cold<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\fetch.rs:179
  27: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\fetch.rs:65
  28: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_> > > >::or_else<ref$<salsa::function::memo::Memo<ty_python_semantic::types::infer::infer_scope_types::infer_s
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1650
  29: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::refresh_memo
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\fetch.rs:65
  30: salsa::function::IngredientImpl<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>::fetch<ty_python_semantic::types::infer::infer_scope_types::infer_scope_types_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\fetch.rs:31
  31: ty_python_semantic::types::infer::infer_scope_types::closure$0
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\components\salsa-macro-rules\src\setup_tracked_fn.rs:479
  32: salsa::attach::Attached::attach<dyn$<ty_python_semantic::db::Db>,ref$<ty_python_semantic::types::infer::ScopeInference>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\attach.rs:79
  33: salsa::attach::attach::closure$0<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\attach.rs:135
  34: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_sco
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:315
  35: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_t
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:279
  36: salsa::attach::attach<ref$<ty_python_semantic::types::infer::ScopeInference>,dyn$<ty_python_semantic::db::Db>,ty_python_semantic::types::infer::infer_scope_types::closure_env$0>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\attach.rs:133
  37: ty_python_semantic::types::infer::infer_scope_types
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\components\salsa-macro-rules\src\setup_tracked_fn.rs:469
  38: ty_python_semantic::types::check_types
             at GitHub\ruff\crates\ty_python_semantic\src\types.rs:129
  39: ty_project::check_file_impl::impl$2::execute::inner_::closure$2
             at GitHub\ruff\crates\ty_project\src\lib.rs:568
  40: std::panicking::catch_unwind::do_call<ty_project::check_file_impl::impl$2::execute::inner_::closure_env$2,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:590
  41: indexmap::set::impl$12::default<ruff_db::system::path::SystemPathBuf,std::hash::random::RandomState>
  42: std::panicking::catch_unwind
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:553
  43: std::panic::catch_unwind
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
  44: salsa::cancelled::Cancelled::catch<ty_project::check_file_impl::impl$2::execute::inner_::closure_env$2,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\cancelled.rs:35
  45: ty_project::catch::closure$0<ty_project::check_file_impl::impl$2::execute::inner_::closure_env$2,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >
             at GitHub\ruff\crates\ty_project\src\lib.rs:678
  46: std::panicking::catch_unwind::do_call<ty_project::catch::closure_env$0<ty_project::check_file_impl::impl$2::execute::inner_::closure_env$2,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >,enum2$<core::option::Option<alloc::vec::Vec<r
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:590
  47: std::panic::catch_unwind<ty_project::catch::closure_env$0<ty_project::check_file_impl::impl$2::execute::inner_::closure_env$2,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >,enum2$<core::option::Option<alloc::vec::Vec<ruff_db::diagn
  48: std::panicking::catch_unwind
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:553
  49: std::panic::catch_unwind<ty_project::catch::closure_env$0<ty_project::check_file_impl::impl$2::execute::inner_::closure_env$2,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >,enum2$<core::option::Option<alloc::vec::Vec<ruff_db::diagn
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
  50: ruff_db::panic::catch_unwind<ty_project::catch::closure_env$0<ty_project::check_file_impl::impl$2::execute::inner_::closure_env$2,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >,enum2$<core::option::Option<alloc::vec::Vec<ruff_db::d
             at GitHub\ruff\crates\ruff_db\src\panic.rs:145
  51: ty_project::catch<ty_project::check_file_impl::impl$2::execute::inner_::closure_env$2,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >
             at GitHub\ruff\crates\ty_project\src\lib.rs:676
  52: ty_project::check_file_impl::impl$2::execute::inner_
             at GitHub\ruff\crates\ty_project\src\lib.rs:568
  53: ty_project::check_file_impl::impl$2::execute
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\components\salsa-macro-rules\src\setup_tracked_fn.rs:302
  54: salsa::function::IngredientImpl<ty_project::check_file_impl::check_file_impl_Configuration_>::execute_query<ty_project::check_file_impl::check_file_impl_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\execute.rs:556
  55: salsa::function::IngredientImpl<ty_project::check_file_impl::check_file_impl_Configuration_>::execute<ty_project::check_file_impl::check_file_impl_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\execute.rs:60
  56: salsa::function::IngredientImpl<ty_project::check_file_impl::check_file_impl_Configuration_>::fetch_cold<ty_project::check_file_impl::check_file_impl_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\fetch.rs:179
  57: salsa::function::fetch::impl$0::refresh_memo::closure$0<ty_project::check_file_impl::check_file_impl_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\fetch.rs:65
  58: enum2$<core::option::Option<ref$<salsa::function::memo::Memo<ty_project::check_file_impl::check_file_impl_Configuration_> > > >::or_else<ref$<salsa::function::memo::Memo<ty_project::check_file_impl::check_file_impl_Configuration_> >,salsa::function::fetch:
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\option.rs:1650
  59: salsa::function::IngredientImpl<ty_project::check_file_impl::check_file_impl_Configuration_>::refresh_memo
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\fetch.rs:65
  60: salsa::function::IngredientImpl<ty_project::check_file_impl::check_file_impl_Configuration_>::fetch<ty_project::check_file_impl::check_file_impl_Configuration_>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\function\fetch.rs:31
  61: ty_project::check_file_impl::closure$0
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\components\salsa-macro-rules\src\setup_tracked_fn.rs:479
  62: salsa::attach::Attached::attach<dyn$<ty_project::db::Db>,ref$<enum2$<core::result::Result<alloc::boxed::Box<slice2$<ruff_db::diagnostic::Diagnostic>,alloc::alloc::Global>,ruff_db::diagnostic::Diagnostic> > >,ty_project::check_file_impl::closure_env$0>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\attach.rs:79
  63: salsa::attach::attach::closure$0<ref$<enum2$<core::result::Result<alloc::boxed::Box<slice2$<ruff_db::diagnostic::Diagnostic>,alloc::alloc::Global>,ruff_db::diagnostic::Diagnostic> > >,dyn$<ty_project::db::Db>,ty_project::check_file_impl::closure_env$0>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\attach.rs:135
  64: std::thread::local::LocalKey<salsa::attach::Attached>::try_with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<enum2$<core::result::Result<alloc::boxed::Box<slice2$<ruff_db::diagnostic::Diagnostic>,alloc::alloc::Global>,ruff_db::diagnost
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:315
  65: std::thread::local::LocalKey<salsa::attach::Attached>::with<salsa::attach::Attached,salsa::attach::attach::closure_env$0<ref$<enum2$<core::result::Result<alloc::boxed::Box<slice2$<ruff_db::diagnostic::Diagnostic>,alloc::alloc::Global>,ruff_db::diagnostic::
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\local.rs:279
  66: salsa::attach::attach<ref$<enum2$<core::result::Result<alloc::boxed::Box<slice2$<ruff_db::diagnostic::Diagnostic>,alloc::alloc::Global>,ruff_db::diagnostic::Diagnostic> > >,dyn$<ty_project::db::Db>,ty_project::check_file_impl::closure_env$0>
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\attach.rs:133
  67: ty_project::check_file_impl
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\components\salsa-macro-rules\src\setup_tracked_fn.rs:469
  68: ty_project::impl$15::check::closure$0::closure$0
             at GitHub\ruff\crates\ty_project\src\lib.rs:297
  69: rayon_core::scope::impl$0::spawn::closure$0::closure$0<ty_project::impl$15::check::closure$0::closure_env$0>
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\scope\mod.rs:531
  70: core::panic::unwind_safe::impl$25::call_once<tuple$<>,rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$15::check::closure$0::closure_env$0> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\panic\unwind_safe.rs:274
  71: std::panicking::catch_unwind::do_call<core::panic::unwind_safe::AssertUnwindSafe<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$15::check::closure$0::closure_env$0> >,tuple$<> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:590
  72: std::panic::catch_unwind<ty_project::catch::closure_env$0<ty_project::check_file_impl::impl$2::execute::inner_::closure_env$2,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >,enum2$<core::option::Option<alloc::vec::Vec<ruff_db::diagn
  73: std::panicking::catch_unwind
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:553
  74: std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$15::check::closure$0::closure_env$0> >,tuple$<> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
  75: rayon_core::unwind::halt_unwinding<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$15::check::closure$0::closure_env$0>,tuple$<> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\unwind.rs:17
  76: rayon_core::scope::ScopeBase::execute_job_closure<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$15::check::closure$0::closure_env$0>,tuple$<> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\scope\mod.rs:693
  77: rayon_core::scope::ScopeBase::execute_job<rayon_core::scope::impl$0::spawn::closure$0::closure_env$0<ty_project::impl$15::check::closure$0::closure_env$0> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\scope\mod.rs:683
  78: rayon_core::scope::impl$0::spawn::closure$0<ty_project::impl$15::check::closure$0::closure_env$0>
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\scope\mod.rs:531
  79: rayon_core::job::impl$6::execute<rayon_core::scope::impl$0::spawn::closure_env$0<ty_project::impl$15::check::closure$0::closure_env$0> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\job.rs:169
  80: rayon_core::job::JobRef::execute
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\job.rs:64
  81: rayon_core::registry::WorkerThread::execute
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:866
  82: rayon_core::registry::WorkerThread::wait_until_cold
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:792
  83: rayon_core::registry::WorkerThread::wait_until<rayon_core::latch::CoreLatch>
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:775
  84: rayon_core::latch::CountLatch::wait
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\latch.rs:396
  85: rayon_core::scope::ScopeBase::complete<rayon_core::scope::scope::closure$0::closure_env$0<ty_project::impl$15::check::closure_env$0,tuple$<> >,tuple$<> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\scope\mod.rs:672
  86: rayon_core::scope::scope::closure$0<ty_project::impl$15::check::closure_env$0,tuple$<> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\scope\mod.rs:284
  87: rayon_core::registry::in_worker<rayon_core::scope::scope::closure_env$0<ty_project::impl$15::check::closure_env$0,tuple$<> >,tuple$<> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:957
  88: rayon_core::scope::scope<ty_project::impl$15::check::closure_env$0,tuple$<> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\scope\mod.rs:282
  89: ty_project::Project::check
             at GitHub\ruff\crates\ty_project\src\lib.rs:288
  90: ty_project::db::ProjectDatabase::check_with_reporter
             at GitHub\ruff\crates\ty_project\src\db.rs:102
  91: ty::impl$2::main_loop::closure$0::closure$0
             at GitHub\ruff\crates\ty\src\lib.rs:291
  92: std::panicking::catch_unwind::do_call<ty::impl$2::main_loop::closure$0::closure_env$0,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:590
  93: salsa::cancelled::Cancelled::catch<ty::impl$2::main_loop::closure$0::closure_env$0,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >
  94: std::panicking::catch_unwind
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:553
  95: std::panic::catch_unwind
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
  96: salsa::cancelled::Cancelled::catch<ty::impl$2::main_loop::closure$0::closure_env$0,alloc::vec::Vec<ruff_db::diagnostic::Diagnostic,alloc::alloc::Global> >
             at .cargo\git\checkouts\salsa-e6f3bb7c2a062968\17bc55d\src\cancelled.rs:35
  97: ty::impl$2::main_loop::closure$0
             at GitHub\ruff\crates\ty\src\lib.rs:290
  98: core::panic::unwind_safe::impl$25::call_once<tuple$<>,ty::impl$2::main_loop::closure_env$0>
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\panic\unwind_safe.rs:274
  99: std::panicking::catch_unwind::do_call<core::panic::unwind_safe::AssertUnwindSafe<ty::impl$2::main_loop::closure_env$0>,tuple$<> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:590
 100: std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<ty::impl$2::main_loop::closure_env$0>,tuple$<> >
 101: std::panicking::catch_unwind
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:553
 102: std::panic::catch_unwind<core::panic::unwind_safe::AssertUnwindSafe<ty::impl$2::main_loop::closure_env$0>,tuple$<> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
 103: rayon_core::unwind::halt_unwinding<ty::impl$2::main_loop::closure_env$0,tuple$<> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\unwind.rs:17
 104: rayon_core::registry::Registry::catch_unwind<ty::impl$2::main_loop::closure_env$0>
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:376
 105: rayon_core::spawn::spawn_job::closure$0<ty::impl$2::main_loop::closure_env$0>
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\spawn\mod.rs:95
 106: rayon_core::job::impl$6::execute<rayon_core::spawn::spawn_job::closure_env$0<ty::impl$2::main_loop::closure_env$0> >
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\job.rs:169
 107: rayon_core::job::JobRef::execute
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\job.rs:64
 108: rayon_core::registry::WorkerThread::execute
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:866
 109: rayon_core::registry::WorkerThread::wait_until_cold
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:800
 110: rayon_core::registry::WorkerThread::wait_until<rayon_core::latch::OnceLatch>
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:775
 111: rayon_core::registry::WorkerThread::wait_until_out_of_work
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:824
 112: rayon_core::registry::main_loop
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:929
 113: rayon_core::registry::ThreadBuilder::run
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:50
 114: rayon_core::registry::impl$2::spawn::closure$0
             at .cargo\registry\src\index.crates.io-1949cf8c6b5b557f\rayon-core-1.13.0\src\registry.rs:95
 115: core::hint::black_box
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\hint.rs:472
 116: std::sys::backtrace::__rust_begin_short_backtrace<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\sys\backtrace.rs:158
 117: std::thread::impl$0::spawn_unchecked_::closure$1::closure$0<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\mod.rs:559
 118: core::panic::unwind_safe::impl$25::call_once<tuple$<>,std::thread::impl$0::spawn_unchecked_::closure$1::closure_env$0<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> > >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\panic\unwind_safe.rs:274
 119: std::panicking::catch_unwind::do_call<core::panic::unwind_safe::AssertUnwindSafe<std::thread::impl$0::spawn_unchecked_::closure$1::closure_env$0<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> > >,tuple$<> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:590
 120: core::iter::range::impl$5::spec_next<i32>
 121: std::panicking::catch_unwind
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panicking.rs:553
 122: std::panic::catch_unwind
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\panic.rs:359
 123: std::thread::impl$0::spawn_unchecked_::closure$1<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\std\src\thread\mod.rs:557
 124: core::ops::function::FnOnce::call_once<std::thread::impl$0::spawn_unchecked_::closure_env$1<rayon_core::registry::impl$2::spawn::closure_env$0,tuple$<> >,tuple$<> >
             at .rustup\toolchains\1.91-x86_64-pc-windows-msvc\lib\rustlib\src\rust\library\core\src\ops\function.rs:250
 125: alloc::boxed::impl$29::call_once
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\alloc\src\boxed.rs:1985
 126: alloc::boxed::impl$29::call_once
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\alloc\src\boxed.rs:1985
 127: std::sys::thread::windows::impl$0::new::thread_start
             at /rustc/ed61e7d7e242494fb7057f2657300d9e77bb4fcb/library\std\src\sys\thread\windows.rs:60
 128: BaseThreadInitThunk
 129: RtlUserThreadStart

info: query stacktrace:
   0: infer_scope_types(Id(1003))
             at crates\ty_python_semantic\src\types\infer.rs:68
   1: check_file_impl(Id(c00))
             at crates\ty_project\src\lib.rs:535


Found 1 diagnostic
WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```

</details>

## Test Plan

New corpus test


---

_Comment by @astral-sh-bot[bot] on 2025-11-27 15:55_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-27 15:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 39 diagnostics
+ Found 38 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-11-27 15:58_

---

_Marked ready for review by @mtshiba on 2025-11-27 16:24_

---

_Review requested from @carljm by @mtshiba on 2025-11-27 16:24_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-11-27 16:24_

---

_Review requested from @sharkdp by @mtshiba on 2025-11-27 16:24_

---

_Review requested from @dcreager by @mtshiba on 2025-11-27 16:24_

---

_@carljm approved on 2025-12-05 02:16_

This looks good; sorry I didn't get to it sooner. Can you fix up the conflicts?

---

_Comment by @mtshiba on 2025-12-05 02:19_

> Can you fix up the conflicts?

Yes, but it would be better to merge #21802 first.



---

_Comment by @carljm on 2025-12-05 02:20_

> it would be better to merge #21802 first.

Done

---

_@carljm approved on 2025-12-05 02:48_

---

_Merged by @carljm on 2025-12-05 02:48_

---

_Closed by @carljm on 2025-12-05 02:48_

---

_Branch deleted on 2025-12-05 03:52_

---
