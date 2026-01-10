---
number: 4194
title: "ruff panics in debug_assert 'Prefer `Fix::deletion`' when fixing bokeh with `--select ALL`"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-05-02T16:31:18Z
updated_at: 2023-05-04T18:42:36Z
url: https://github.com/astral-sh/ruff/issues/4194
synced_at: 2026-01-10T01:22:43Z
---

# ruff panics in debug_assert 'Prefer `Fix::deletion`' when fixing bokeh with `--select ALL`

---

_Issue opened by @konstin on 2023-05-02 16:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```shell
git clone https://github.com/bokeh/bokeh # prints errors, ignore
cd bokeh
git checkout -f 3.2.0.dev2 # more errors, ignore, sorry again
```

In the ruff dir, checkout ac600bb3da05b7259a8b646c36e1dfe21ef8c450 and  `cargo build` in debug mode.

In the bokeh dir:

```
../ruff/target/debug/ruff check --no-cache --select ALL --fix .
```

I haven't gotten around to minimizing this, but the relevant snippet is https://github.com/charliermarsh/ruff/blob/cab65b25da8aff6fb9cb539b55345aae64a835f0/crates/ruff_diagnostics/src/edit.rs#L33-L47

```
panicked at 'Prefer `Fix::deletion`', crates/ruff_diagnostics/src/edit.rs:41:9
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
             at /home/konsti/ruff/crates/ruff_cli/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/alloc/src/boxed.rs:2001:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:696:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:581:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/sys_common/backtrace.rs:150:18
   5: rust_begin_unwind
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:579:5
   6: core::panicking::panic_fmt
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/panicking.rs:64:14
   7: ruff_diagnostics::edit::Edit::range_replacement
             at /home/konsti/ruff/crates/ruff_diagnostics/src/edit.rs:41:9
   8: ruff::rules::pydocstyle::rules::sections::common_section
             at /home/konsti/ruff/crates/ruff/src/rules/pydocstyle/rules/sections.rs:622:36
   9: ruff::rules::pydocstyle::rules::sections::google_section
             at /home/konsti/ruff/crates/ruff/src/rules/pydocstyle/rules/sections.rs:915:5
  10: ruff::rules::pydocstyle::rules::sections::parse_google_sections
             at /home/konsti/ruff/crates/ruff/src/rules/pydocstyle/rules/sections.rs:957:9
  11: ruff::rules::pydocstyle::rules::sections::sections
             at /home/konsti/ruff/crates/ruff/src/rules/pydocstyle/rules/sections.rs:321:17
  12: ruff::checkers::ast::Checker::check_definitions
             at /home/konsti/ruff/crates/ruff/src/checkers/ast/mod.rs:5579:25
  13: ruff::checkers::ast::check_ast
             at /home/konsti/ruff/crates/ruff/src/checkers/ast/mod.rs:5677:5
  14: ruff::linter::check_path
             at /home/konsti/ruff/crates/ruff/src/linter.rs:143:40
  15: ruff::linter::lint_fix
             at /home/konsti/ruff/crates/ruff/src/linter.rs:436:22
  16: ruff_cli::diagnostics::lint_path
             at /home/konsti/ruff/crates/ruff_cli/src/diagnostics.rs:157:14
  17: ruff_cli::commands::run::lint_path::{{closure}}
             at /home/konsti/ruff/crates/ruff_cli/src/commands/run.rs:164:9
  18: std::panicking::try::do_call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:487:40
  19: __rust_try
  20: std::panicking::try
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:451:19
  21: std::panic::catch_unwind
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panic.rs:140:14
  22: ruff_cli::panic::catch_unwind
             at /home/konsti/ruff/crates/ruff_cli/src/panic.rs:40:18
  23: ruff_cli::commands::run::lint_path
             at /home/konsti/ruff/crates/ruff_cli/src/commands/run.rs:163:18
  24: ruff_cli::commands::run::run::{{closure}}
             at /home/konsti/ruff/crates/ruff_cli/src/commands/run.rs:91:21
  25: core::ops::function::impls::<impl core::ops::function::FnMut<A> for &F>::call_mut
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/ops/function.rs:274:13
  26: core::iter::adapters::map::map_fold::{{closure}}
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/iter/adapters/map.rs:84:28
  27: core::iter::traits::iterator::Iterator::fold
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/iter/traits/iterator.rs:2477:21
  28: <core::iter::adapters::map::Map<I,F> as core::iter::traits::iterator::Iterator>::fold
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/iter/adapters/map.rs:124:9
  29: <rayon::iter::reduce::ReduceFolder<R,T> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/reduce.rs:105:19
  30: <rayon::iter::map::MapFolder<C,F> as rayon::iter::plumbing::Folder<T>>::consume_iter
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/map.rs:248:21
  31: rayon::iter::plumbing::Producer::fold_with
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:110:9
  32: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:438:13
  33: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  34: rayon_core::join::join_context::call_b::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:129:25
  35: rayon_core::job::StackJob<L,F,R>::run_inline
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/job.rs:102:9
  36: rayon_core::join::join_context::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:159:36
  37: rayon_core::registry::in_worker
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:984:13
  38: rayon_core::join::join_context
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:132:5
  39: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  40: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:418:21
  41: rayon_core::join::join_context::call_a::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:124:17
  42: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/panic/unwind_safe.rs:271:9
  43: std::panicking::try::do_call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:487:40
  44: __rust_try
  45: std::panicking::try
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:451:19
  46: std::panic::catch_unwind
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panic.rs:140:14
  47: rayon_core::unwind::halt_unwinding
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/unwind.rs:17:5
  48: rayon_core::join::join_context::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:142:24
  49: rayon_core::registry::in_worker
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:984:13
  50: rayon_core::join::join_context
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:132:5
  51: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  52: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  53: rayon_core::join::join_context::call_b::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:129:25
  54: rayon_core::job::StackJob<L,F,R>::run_inline
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/job.rs:102:9
  55: rayon_core::join::join_context::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:159:36
  56: rayon_core::registry::in_worker
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:984:13
  57: rayon_core::join::join_context
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:132:5
  58: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  59: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:418:21
  60: rayon_core::join::join_context::call_a::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:124:17
  61: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/panic/unwind_safe.rs:271:9
  62: std::panicking::try::do_call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:487:40
  63: __rust_try
  64: std::panicking::try
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:451:19
  65: std::panic::catch_unwind
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panic.rs:140:14
  66: rayon_core::unwind::halt_unwinding
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/unwind.rs:17:5
  67: rayon_core::join::join_context::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:142:24
  68: rayon_core::registry::in_worker
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:984:13
  69: rayon_core::join::join_context
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:132:5
  70: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  71: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:418:21
  72: rayon_core::join::join_context::call_a::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:124:17
  73: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/panic/unwind_safe.rs:271:9
  74: std::panicking::try::do_call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:487:40
  75: __rust_try
  76: std::panicking::try
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:451:19
  77: std::panic::catch_unwind
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panic.rs:140:14
  78: rayon_core::unwind::halt_unwinding
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/unwind.rs:17:5
  79: rayon_core::join::join_context::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:142:24
  80: rayon_core::registry::in_worker
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:984:13
  81: rayon_core::join::join_context
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:132:5
  82: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:416:47
  83: rayon::iter::plumbing::bridge_producer_consumer::helper::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-1.7.0/src/iter/plumbing/mod.rs:427:21
  84: rayon_core::join::join_context::call_b::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/join/mod.rs:129:25
  85: rayon_core::job::JobResult<T>::call::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/job.rs:218:41
  86: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/panic/unwind_safe.rs:271:9
  87: std::panicking::try::do_call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:487:40
  88: __rust_try
  89: std::panicking::try
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:451:19
  90: std::panic::catch_unwind
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panic.rs:140:14
  91: rayon_core::unwind::halt_unwinding
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/unwind.rs:17:5
  92: rayon_core::job::JobResult<T>::call
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/job.rs:218:15
  93: <rayon_core::job::StackJob<L,F,R> as rayon_core::job::Job>::execute
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/job.rs:120:32
  94: rayon_core::job::JobRef::execute
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/job.rs:64:9
  95: rayon_core::registry::WorkerThread::execute
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:874:9
  96: rayon_core::registry::WorkerThread::wait_until_cold
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:820:17
  97: rayon_core::registry::WorkerThread::wait_until
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:803:13
  98: rayon_core::registry::main_loop
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:948:5
  99: rayon_core::registry::ThreadBuilder::run
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:54:18
 100: <rayon_core::registry::DefaultSpawn as rayon_core::registry::ThreadSpawn>::spawn::{{closure}}
             at /home/konsti/.cargo/registry/src/github.com-1ecc6299db9ec823/rayon-core-1.11.0/src/registry.rs:99:20
 101: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/sys_common/backtrace.rs:134:18
 102: std::thread::Builder::spawn_unchecked_::{{closure}}::{{closure}}
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/thread/mod.rs:560:17
 103: <core::panic::unwind_safe::AssertUnwindSafe<F> as core::ops::function::FnOnce<()>>::call_once
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/panic/unwind_safe.rs:271:9
 104: std::panicking::try::do_call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:487:40
 105: __rust_try
 106: std::panicking::try
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panicking.rs:451:19
 107: std::panic::catch_unwind
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/panic.rs:140:14
 108: std::thread::Builder::spawn_unchecked_::{{closure}}
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/thread/mod.rs:559:30
 109: core::ops::function::FnOnce::call_once{{vtable.shim}}
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/core/src/ops/function.rs:250:5
 110: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/alloc/src/boxed.rs:1987:9
 111: <alloc::boxed::Box<F,A> as core::ops::function::FnOnce<Args>>::call_once
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/alloc/src/boxed.rs:1987:9
 112: std::sys::unix::thread::Thread::new::thread_start
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library/std/src/sys/unix/thread.rs:108:17
 113: start_thread
             at /build/glibc-SzIz7B/glibc-2.31/nptl/pthread_create.c:477:8
 114: clone
             at /build/glibc-SzIz7B/glibc-2.31/misc/../sysdeps/unix/sysv/linux/x86_64/clone.S:95
```





---

_Label `bug` added by @konstin on 2023-05-02 16:31_

---

_Comment by @MichaReiser on 2023-05-02 17:24_

I believe Ruff should show you the relevant file name in the warning printed to the console. That should help you to narrow it down to at least a specific file.

Also, we should fix this in the call-site rather than removing the diagnostic:

```
   8: ruff::rules::pydocstyle::rules::sections::common_section
             at /home/konsti/ruff/crates/ruff/src/rules/pydocstyle/rules/sections.rs:622:36
```

---

_Comment by @zanieb on 2023-05-03 02:45_

The failing file is https://github.com/bokeh/bokeh/blob/3.2.0.dev2/src/bokeh/tile_providers.py

I tried naively fixing this by using `deletion` instead of `range_replacement` when the content is empty

```diff
diff --git a/crates/ruff/src/rules/pydocstyle/rules/sections.rs b/crates/ruff/src/rules/pydocstyle/rules/sections.rs
index 5a5b55b8..ca658843 100644
--- a/crates/ruff/src/rules/pydocstyle/rules/sections.rs
+++ b/crates/ruff/src/rules/pydocstyle/rules/sections.rs
@@ -619,10 +619,16 @@ fn common_section(
             );
             if checker.patch(diagnostic.kind.rule()) {
                 // Replace the existing indentation with whitespace of the appropriate length.
-                diagnostic.set_fix(Edit::range_replacement(
-                    whitespace::clean(docstring.indentation),
-                    TextRange::at(context.range().start(), leading_space.text_len()),
-                ));
+                let content = whitespace::clean(docstring.indentation);
+                if content.is_empty() {
+                    let start = context.range().start();
+                    diagnostic.set_fix(Edit::deletion(start, start + leading_space.text_len()));
+                } else {
+                    diagnostic.set_fix(Edit::range_replacement(
+                        content,
+                        TextRange::at(context.range().start(), leading_space.text_len()),
+                    ));
+                }
             };
             checker.diagnostics.push(diagnostic);
         }
```

but this does some weird things to the whitespace that I imagine is not the intent

```diff
diff --git a/src/bokeh/tile_providers.py b/src/bokeh/tile_providers.py
index 682636530..4e23a5b85 100644
--- a/src/bokeh/tile_providers.py
+++ b/src/bokeh/tile_providers.py
@@ -4,7 +4,7 @@
 #
 # The full license is in the file LICENSE.txt, distributed with this software.
 #-----------------------------------------------------------------------------
-''' Pre-configured tile sources for common third party tile services.
+'''Pre-configured tile sources for common third party tile s
ervices.
 
 get_provider
     Use this function to retrieve an instance of a predefined tile provider.
@@ -13,7 +13,8 @@ get_provider
         get_provider is deprecated as of Bokeh 3.0.0 and will be removed in a future
         release. Use ``add_tile`` directly instead.
 
-    Args:
+Args:
+----
         provider_name (Union[str, Vendors, xyzservices.TileProvider])
             Name of the tile provider to supply.
 
@@ -21,14 +22,16 @@ get_provider
             name of one of the known providers. Use
             :class:`xyzservices.TileProvider` to pass custom tile providers.
 
-    Returns:
+Returns:
+-------
         WMTSTileProviderSource: The desired tile provider instance
 
-    Raises:
+Raises:
+------
         ValueError, if the specified provider can not be found
 
-    Example:
-
```

It seems like the issue is that `docstring.indentation` is an empty string?

If you run `pydocstyle --select=D` it recommends:

```
/Users/mz/eng/src/bokeh/bokeh/src/bokeh/tile_providers.py:7 at module level:
        D210: No whitespaces allowed surrounding docstring text
/Users/mz/eng/src/bokeh/bokeh/src/bokeh/tile_providers.py:7 at module level:
        D213: Multi-line docstring summary should start at the second line
/Users/mz/eng/src/bokeh/bokeh/src/bokeh/tile_providers.py:7 at module level:
        D300: Use """triple double quotes""" (found '''-quotes)
/Users/mz/eng/src/bokeh/bokeh/src/bokeh/tile_providers.py:7 at module level:
        D214: Section is over-indented ('Returns')
/Users/mz/eng/src/bokeh/bokeh/src/bokeh/tile_providers.py:7 at module level:
        D407: Missing dashed underline after section ('Returns')
/Users/mz/eng/src/bokeh/bokeh/src/bokeh/tile_providers.py:7 at module level:
        D406: Section name should end with a newline ('Returns', not 'Returns:')
/Users/mz/eng/src/bokeh/bokeh/src/bokeh/tile_providers.py:7 at module level:
        D214: Section is over-indented ('Raises')
/Users/mz/eng/src/bokeh/bokeh/src/bokeh/tile_providers.py:7 at module level:
        D407: Missing dashed underline after section ('Raises')
/Users/mz/eng/src/bokeh/bokeh/src/bokeh/tile_providers.py:7 at module level:
        D406: Section name should end with a newline ('Raises', not 'Raises:')
```

However in this example I think the indentation is correct and should not be auto-fixed. It's unclear to me why they're documenting the function at the module level — perhaps because of the peculiar `ModuleType` implementation.

---

_Comment by @charliermarsh on 2023-05-04 02:59_

I think the fix you have there seems correct to me. I don't mind if the diagnostics are a bit wonky as long as they're consistent with pydocstyle for now :)

---

_Referenced in [astral-sh/ruff#4216](../../astral-sh/ruff/pulls/4216.md) on 2023-05-04 05:42_

---

_Referenced in [astral-sh/ruff#4193](../../astral-sh/ruff/pulls/4193.md) on 2023-05-04 09:01_

---

_Closed by @charliermarsh on 2023-05-04 18:42_

---
