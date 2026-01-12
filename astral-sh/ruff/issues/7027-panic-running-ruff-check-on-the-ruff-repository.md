```yaml
number: 7027
title: "Panic running `ruff check` on the ruff repository"
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2023-08-31T16:04:27Z
updated_at: 2023-08-31T16:16:30Z
url: https://github.com/astral-sh/ruff/issues/7027
synced_at: 2026-01-12T15:54:46Z
```

# Panic running `ruff check` on the ruff repository

---

_@zanieb_

From `main` at 96a9717c1a77796bc23d15625b60e8c03851e54c

Piping standard output to another file lets us see the panics buried in the run which otherwise completes happily

```
./target/debug/ruff check . --no-cache --select CPY001 --isolated > /dev/null
```
```
warning: Linting panicked crates/ruff/resources/test/fixtures/pyupgrade/UP012.py: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'byte index 1024 is not a char boundary; it is inside '©' (bytes 1023..1025) of `# ASCII literals should be replaced by a bytes literal
"foo".encode("utf-8")  # b"foo"
"foo".encode("u8")  # b"foo"
"foo".encode()  # b"foo"
"foo".encode("UTF8")  # b"foo"
U"foo".encode("utf-8")  # b"foo"
"foo".encode(encoding="utf-8")  # b"foo"
"""
Lorem
`[...]', /Users/mz/eng/src/astral-sh/ruff/crates/ruff_source_file/src/locator.rs:382:10
Backtrace:    0: std::backtrace_rs::backtrace::libunwind::trace
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/../../backtrace/src/backtrace/libunwind.rs:93:5
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/../../backtrace/src/backtrace/mod.rs:66:5
   2: std::backtrace::Backtrace::create
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/backtrace.rs:332:13
   3: ruff_cli::panic::catch_unwind::{{closure}}
             at ./crates/ruff_cli/src/panic.rs:31:25
   4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/alloc/src/boxed.rs:2007:9
   5: std::panicking::rust_panic_with_hook
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:709:13
   6: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:597:13
   7: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/sys_common/backtrace.rs:151:18
   8: rust_begin_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:593:5
   9: core::panicking::panic_fmt
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panicking.rs:67:14
  10: core::str::slice_error_fail_rt
  11: core::str::slice_error_fail
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/str/mod.rs:87:9
  12: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::Range<usize>>::index
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/str/traits.rs:235:21
  13: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/str/traits.rs:61:9
  14: ruff_text_size::range::<impl core::ops::index::Index<ruff_text_size::range::TextRange> for str>::index
             at ./crates/ruff_text_size/src/range.rs:430:10
  15: ruff_source_file::locator::Locator::up_to
             at ./crates/ruff_source_file/src/locator.rs:382:10
  16: ruff::rules::flake8_copyright::rules::missing_copyright_notice::missing_copyright_notice
             at ./crates/ruff/src/rules/flake8_copyright/rules/missing_copyright_notice.rs:39:9
  17: ruff::checkers::physical_lines::check_physical_lines
             at ./crates/ruff/src/checkers/physical_lines.rs:91:35
  18: ruff::linter::check_path
             at ./crates/ruff/src/linter.rs:212:28
  19: ruff::linter::lint_only
             at ./crates/ruff/src/linter.rs:371:18
  20: ruff_cli::diagnostics::lint_path
             at ./crates/ruff_cli/src/diagnostics.rs:376:22
  21: ruff_cli::commands::check::lint_path::{{closure}}
             at ./crates/ruff_cli/src/commands/check.rs:206:9
  22: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
  23: ___rust_try
  24: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
  25: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
  26: ruff_cli::panic::catch_unwind
             at ./crates/ruff_cli/src/panic.rs:40:18
  27: ruff_cli::commands::check::lint_path
             at ./crates/ruff_cli/src/commands/check.rs:205:18
  28: ruff_cli::commands::check::check::{{closure}}
             at ./crates/ruff_cli/src/commands/check.rs:125:21
  29: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &F>::call_mut
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/ops/function.rs:272:13
  30: core::iter::adapters::map::map_fold::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/iter/adapters/map.rs:84:28
  31: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/slice/iter/macros.rs:215:27
  32: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/iter/adapters/map.rs:124:9
  33: <rayon::iter::reduce::ReduceFolder<R,T> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/reduce.rs:105:19
  34: <rayon::iter::map::MapFolder<C,F> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/map.rs:248:21
  35: rayon::iter::plumbing::Producer::fold_with
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:110:9
  36: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:438:13
  37: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  38: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:129:25
  39: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:102:9
  40: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:159:36
  41: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  42: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  43: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  44: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  45: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:129:25
  46: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:102:9
  47: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:159:36
  48: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  49: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  50: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  51: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  52: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:129:25
  53: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:102:9
  54: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:159:36
  55: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  56: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  57: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  58: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:418:21
  59: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:124:17
  60: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panic/unwind_safe.rs:271:9
  61: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
  62: ___rust_try
  63: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
  64: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
  65: rayon_core::unwind::halt_unwinding
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/unwind.rs:17:5
  66: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:142:24
  67: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  68: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  69: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  70: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:418:21
  71: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:124:17
  72: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panic/unwind_safe.rs:271:9
  73: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
  74: ___rust_try
  75: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
  76: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
  77: rayon_core::unwind::halt_unwinding
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/unwind.rs:17:5
  78: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:142:24
  79: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  80: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  81: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  82: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  83: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:129:25
  84: rayon_core::job::JobResult<T>::call::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:218:41
  85: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panic/unwind_safe.rs:271:9
  86: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
  87: ___rust_try
  88: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
  89: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
  90: rayon_core::unwind::halt_unwinding
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/unwind.rs:17:5
  91: rayon_core::job::JobResult<T>::call
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:218:15
  92: <rayon_core::job::StackJob<L,F,R> as rayon_core::job::Job>::execute
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:120:32
  93: rayon_core::job::JobRef::execute
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:64:9
  94: rayon_core::registry::WorkerThread::execute
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:874:9
  95: rayon_core::registry::WorkerThread::wait_until_cold
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:820:17
  96: rayon_core::registry::WorkerThread::wait_until
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:803:13
  97: rayon_core::registry::main_loop
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:948:5
  98: rayon_core::registry::ThreadBuilder::run
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:54:18
  99: <rayon_core::registry::DefaultSpawn as rayon_core::registry::ThreadSpawn>::spawn::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:99:20
 100: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/sys_common/backtrace.rs:135:18
 101: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/thread/mod.rs:529:17
 102: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panic/unwind_safe.rs:271:9
 103: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
 104: ___rust_try
 105: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
 106: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
 107: std::thread::Builder::spawn_unchecked_::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/thread/mod.rs:528:30
 108: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/ops/function.rs:250:5
 109: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/alloc/src/boxed.rs:1993:9
 110: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/alloc/src/boxed.rs:1993:9
 111: std::sys::unix::thread::Thread::new::thread_start
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/sys/unix/thread.rs:108:17
 112: __pthread_joiner_wake


warning: Linting panicked crates/ruff/resources/test/fixtures/flake8_simplify/SIM401.py: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'byte index 1024 is not a char boundary; it is inside '💣' (bytes 1023..1027) of `###
# Positive cases
###

# SIM401 (pattern-1)
if key in a_dict:
    var = a_dict[key]
else:
    var = "default1"

# SIM401 (pattern-2)
if key not in a_dict:
    var = "default2"
else:
    var = a_dict[key]

# OK (default contains effect)
if key in a_dict:`[...]', /Users/mz/eng/src/astral-sh/ruff/crates/ruff_source_file/src/locator.rs:382:10
Backtrace:    0: std::backtrace_rs::backtrace::libunwind::trace
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/../../backtrace/src/backtrace/libunwind.rs:93:5
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/../../backtrace/src/backtrace/mod.rs:66:5
   2: std::backtrace::Backtrace::create
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/backtrace.rs:332:13
   3: ruff_cli::panic::catch_unwind::{{closure}}
             at ./crates/ruff_cli/src/panic.rs:31:25
   4: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/alloc/src/boxed.rs:2007:9
   5: std::panicking::rust_panic_with_hook
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:709:13
   6: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:597:13
   7: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/sys_common/backtrace.rs:151:18
   8: rust_begin_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:593:5
   9: core::panicking::panic_fmt
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panicking.rs:67:14
  10: core::str::slice_error_fail_rt
  11: core::str::slice_error_fail
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/str/mod.rs:87:9
  12: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::Range<usize>>::index
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/str/traits.rs:235:21
  13: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/str/traits.rs:61:9
  14: ruff_text_size::range::<impl core::ops::index::Index<ruff_text_size::range::TextRange> for str>::index
             at ./crates/ruff_text_size/src/range.rs:430:10
  15: ruff_source_file::locator::Locator::up_to
             at ./crates/ruff_source_file/src/locator.rs:382:10
  16: ruff::rules::flake8_copyright::rules::missing_copyright_notice::missing_copyright_notice
             at ./crates/ruff/src/rules/flake8_copyright/rules/missing_copyright_notice.rs:39:9
  17: ruff::checkers::physical_lines::check_physical_lines
             at ./crates/ruff/src/checkers/physical_lines.rs:91:35
  18: ruff::linter::check_path
             at ./crates/ruff/src/linter.rs:212:28
  19: ruff::linter::lint_only
             at ./crates/ruff/src/linter.rs:371:18
  20: ruff_cli::diagnostics::lint_path
             at ./crates/ruff_cli/src/diagnostics.rs:376:22
  21: ruff_cli::commands::check::lint_path::{{closure}}
             at ./crates/ruff_cli/src/commands/check.rs:206:9
  22: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
  23: ___rust_try
  24: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
  25: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
  26: ruff_cli::panic::catch_unwind
             at ./crates/ruff_cli/src/panic.rs:40:18
  27: ruff_cli::commands::check::lint_path
             at ./crates/ruff_cli/src/commands/check.rs:205:18
  28: ruff_cli::commands::check::check::{{closure}}
             at ./crates/ruff_cli/src/commands/check.rs:125:21
  29: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &F>::call_mut
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/ops/function.rs:272:13
  30: core::iter::adapters::map::map_fold::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/iter/adapters/map.rs:84:28
  31: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/slice/iter/macros.rs:215:27
  32: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/iter/adapters/map.rs:124:9
  33: <rayon::iter::reduce::ReduceFolder<R,T> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/reduce.rs:105:19
  34: <rayon::iter::map::MapFolder<C,F> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/map.rs:248:21
  35: rayon::iter::plumbing::Producer::fold_with
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:110:9
  36: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:438:13
  37: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:418:21
  38: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:124:17
  39: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panic/unwind_safe.rs:271:9
  40: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
  41: ___rust_try
  42: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
  43: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
  44: rayon_core::unwind::halt_unwinding
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/unwind.rs:17:5
  45: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:142:24
  46: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  47: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  48: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  49: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  50: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:129:25
  51: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:102:9
  52: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:159:36
  53: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  54: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  55: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  56: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  57: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:129:25
  58: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:102:9
  59: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:159:36
  60: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  61: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  62: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  63: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  64: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:129:25
  65: rayon_core::job::StackJob<L,F,R>::run_inline
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:102:9
  66: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:159:36
  67: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  68: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  69: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  70: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:418:21
  71: rayon_core::join::join_context::call_a::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:124:17
  72: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panic/unwind_safe.rs:271:9
  73: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
  74: ___rust_try
  75: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
  76: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
  77: rayon_core::unwind::halt_unwinding
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/unwind.rs:17:5
  78: rayon_core::join::join_context::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:142:24
  79: rayon_core::registry::in_worker
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:984:13
  80: rayon_core::join::join_context
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:132:5
  81: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  82: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  83: rayon_core::join::join_context::call_b::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/join/mod.rs:129:25
  84: rayon_core::job::JobResult<T>::call::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:218:41
  85: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panic/unwind_safe.rs:271:9
  86: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
  87: ___rust_try
  88: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
  89: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
  90: rayon_core::unwind::halt_unwinding
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/unwind.rs:17:5
  91: rayon_core::job::JobResult<T>::call
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:218:15
  92: <rayon_core::job::StackJob<L,F,R> as rayon_core::job::Job>::execute
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:120:32
  93: rayon_core::job::JobRef::execute
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/job.rs:64:9
  94: rayon_core::registry::WorkerThread::execute
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:874:9
  95: rayon_core::registry::WorkerThread::wait_until_cold
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:820:17
  96: rayon_core::registry::WorkerThread::wait_until
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:803:13
  97: rayon_core::registry::main_loop
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:948:5
  98: rayon_core::registry::ThreadBuilder::run
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:54:18
  99: <rayon_core::registry::DefaultSpawn as rayon_core::registry::ThreadSpawn>::spawn::{{closure}}
             at /Users/mz/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-core-1.11.0/src/registry.rs:99:20
 100: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/sys_common/backtrace.rs:135:18
 101: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/thread/mod.rs:529:17
 102: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panic/unwind_safe.rs:271:9
 103: std::panicking::try::do_call
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:500:40
 104: ___rust_try
 105: std::panicking::try
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:464:19
 106: std::panic::catch_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panic.rs:142:14
 107: std::thread::Builder::spawn_unchecked_::{{closure}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/thread/mod.rs:528:30
 108: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/ops/function.rs:250:5
 109: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/alloc/src/boxed.rs:1993:9
 110: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/alloc/src/boxed.rs:1993:9
 111: std::sys::unix::thread::Thread::new::thread_start
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/sys/unix/thread.rs:108:17
 112: __pthread_joiner_wake


warning: Unexpected `# ruff: noqa` directive at crates/ruff/resources/test/fixtures/ruff/ruff_noqa_invalid.py:1. File-level suppression comments must appear on their own line.
```

---

_Comment by @MichaReiser on 2023-08-31 16:07_

This is probably caused by https://github.com/astral-sh/ruff/issues/7015. Is this issue about investigating why the panic gets swallowed otherwise? 

---

_Comment by @zanieb on 2023-08-31 16:07_

Duplicate of https://github.com/astral-sh/ruff/issues/7015 (thanks @MichaReiser)

Should this panic cause the linter to fail? It seems good that it continues but weird that it's not noted in the summary.

---

_Closed by @zanieb on 2023-08-31 16:16_

---
