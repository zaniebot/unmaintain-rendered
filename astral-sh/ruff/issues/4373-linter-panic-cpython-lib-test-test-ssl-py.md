```yaml
number: 4373
title: "[Linter panic] cpython\\Lib\\test\\test_ssl.py"
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2023-05-11T13:10:38Z
updated_at: 2023-05-11T17:03:21Z
url: https://github.com/astral-sh/ruff/issues/4373
synced_at: 2026-01-12T15:54:44Z
```

# [Linter panic] cpython\Lib\test\test_ssl.py

---

_@FishAlchemist_

## Ruff Version
https://github.com/charliermarsh/ruff/tree/be6e00ef6e5efea68a8a1a0093611b26142b753d
## Command(Powershell)
```powershell
cargo run -p ruff_cli -- check --select=ALL --fix --no-cache --isolated .\test_ssl.py
```
## Terminal Content
```powershell
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
warning: Linting panicked test_ssl.py: This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/charliermarsh/ruff/issues/new?title=%5BLinter%20panic%5D

with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'assertion failed: start.raw <= end.raw', C:\Users\fish\.cargo\git\checkouts\parser-aad50aa3362d3a85\2af9805\ruff_text_size\src\range.rs:48:9
Backtrace:    0: std::backtrace_rs::backtrace::dbghelp::trace
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\..\..\backtrace\src\backtrace\dbghelp.rs:98
   1: std::backtrace_rs::backtrace::trace_unsynchronized
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\..\..\backtrace\src\backtrace\mod.rs:66
   2: std::backtrace::Backtrace::create
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\backtrace.rs:332
   3: std::backtrace::Backtrace::force_capture
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\backtrace.rs:314
   4: ruff_cli::panic::catch_unwind::closure$0<ruff_cli::commands::run::lint_path::closure_env$0,enum2$<core::result::Result<ruff_cli::diagnostics::Diagnostics,anyhow::Error> > >
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\panic.rs:31
   5: alloc::boxed::impl$47::call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\alloc\src\boxed.rs:2001
   6: std::panicking::rust_panic_with_hook
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\panicking.rs:696
   7: std::panicking::begin_panic_handler::closure$0
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\panicking.rs:581
   8: std::sys_common::backtrace::__rust_end_short_backtrace<std::panicking::begin_panic_handler::closure_env$0,never$>
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\sys_common\backtrace.rs:150
   9: std::panicking::begin_panic_handler
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\panicking.rs:579
  10: core::panicking::panic_fmt
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\core\src\panicking.rs:64
  11: core::panicking::panic
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\core\src\panicking.rs:114
  12: ruff_text_size::range::TextRange::new
             at C:\Users\fish\.cargo\git\checkouts\parser-aad50aa3362d3a85\2af9805\ruff_text_size\src\range.rs:48
  13: ruff::autofix::apply_fixes<core::iter::adapters::peekable::Peekable<core::iter::adapters::filter::Filter<core::slice::iter::Iter<ruff_diagnostics::diagnostic::Diagnostic>,ruff::autofix::fix_file::closure_env$0> > >
             at C:\Users\fish\source\ruff\crates\ruff\src\autofix\mod.rs:66
  14: ruff::autofix::fix_file
             at C:\Users\fish\source\ruff\crates\ruff\src\autofix\mod.rs:25
  15: ruff::linter::lint_fix
             at C:\Users\fish\source\ruff\crates\ruff\src\linter.rs:456
  16: ruff_cli::diagnostics::lint_path
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\diagnostics.rs:155
  17: ruff_cli::commands::run::lint_path::closure$0
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\commands\run.rs:164
  18: std::panicking::try::do_call<ruff_cli::commands::run::lint_path::closure_env$0,enum2$<core::result::Result<ruff_cli::diagnostics::Diagnostics,anyhow::Error> > >
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\std\src\panicking.rs:487
  19: std::panicking::try::do_catch<core::panic::unwind_safe::AssertUnwindSafe<rayon_core::job::impl$9::call::closure_env$0<tuple$<usize,usize>,rayon_core::registry::impl$6::in_worker_cold::closure$0::closure_env$0<rayon_core::join::join_context::closure_env$0<
  20: std::panicking::try<enum2$<core::result::Result<ruff_cli::diagnostics::Diagnostics,anyhow::Error> >,ruff_cli::commands::run::lint_path::closure_env$0>
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\std\src\panicking.rs:451
  21: std::panic::catch_unwind<ruff_cli::commands::run::lint_path::closure_env$0,enum2$<core::result::Result<ruff_cli::diagnostics::Diagnostics,anyhow::Error> > >
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\std\src\panic.rs:140
  22: ruff_cli::panic::catch_unwind<ruff_cli::commands::run::lint_path::closure_env$0,enum2$<core::result::Result<ruff_cli::diagnostics::Diagnostics,anyhow::Error> > >
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\panic.rs:40
  23: ruff_cli::commands::run::lint_path
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\commands\run.rs:163
  24: ruff_cli::commands::run::run::closure$0
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\commands\run.rs:91
  25: core::ops::function::impls::impl$1::call_mut<tuple$<ref$<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > > >,ruff_cli::commands::run::run::closure_env$0>
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\core\src\ops\function.rs:274
  26: core::iter::adapters::map::map_fold::closure$0<ref$<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,ruff_cli::diagnostics::Diagnostics,ruff_cli::diagnostics::Diagnostics,ref$<ruff_cli::commands::run::run::closure_env$0>,ref$
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\core\src\iter\adapters\map.rs:84
  27: core::iter::traits::iterator::Iterator::fold<core::slice::iter::Iter<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,ruff_cli::diagnostics::Diagnostics,core::iter::adapters::map::map_fold::closure_env$0<ref$<enum2$<core::res
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\core\src\iter\traits\iterator.rs:2477
  28: core::iter::adapters::map::impl$2::fold<ruff_cli::diagnostics::Diagnostics,core::slice::iter::Iter<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,ref$<ruff_cli::commands::run::run::closure_env$0>,ruff_cli::diagnostics::Diag
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\core\src\iter\adapters\map.rs:124
  29: rayon::iter::reduce::impl$5::consume_iter<ruff_cli::commands::run::run::closure_env$1,ruff_cli::diagnostics::Diagnostics,core::iter::adapters::map::Map<core::slice::iter::Iter<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\reduce.rs:105
  30: rayon::iter::map::impl$8::consume_iter<ref$<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,ruff_cli::diagnostics::Diagnostics,rayon::iter::reduce::ReduceFolder<ruff_cli::commands::run::run::closure_env$1,ruff_cli::diagnosti
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\map.rs:248
  31: rayon::iter::plumbing::Producer::fold_with<rayon::slice::IterProducer<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,rayon::iter::map::MapFolder<rayon::iter::reduce::ReduceFolder<ruff_cli::commands::run::run::closure_env$1,
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\plumbing\mod.rs:110
  32: rayon::iter::plumbing::bridge_producer_consumer::helper<rayon::slice::IterProducer<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,rayon::iter::map::MapConsumer<rayon::iter::reduce::ReduceConsumer<ruff_cli::commands::run::ru
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\plumbing\mod.rs:438
  33: rayon::iter::plumbing::bridge_producer_consumer<rayon::slice::IterProducer<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,rayon::iter::map::MapConsumer<rayon::iter::reduce::ReduceConsumer<ruff_cli::commands::run::run::closu
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\plumbing\mod.rs:397
  34: rayon::iter::plumbing::bridge::impl$0::callback<rayon::iter::map::MapConsumer<rayon::iter::reduce::ReduceConsumer<ruff_cli::commands::run::run::closure_env$1,ruff_cli::diagnostics::Diagnostics (*)()>,ruff_cli::commands::run::run::closure_env$0>,ref$<enum2
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\plumbing\mod.rs:373
  35: rayon::slice::impl$6::with_producer<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > >,rayon::iter::plumbing::bridge::Callback<rayon::iter::map::MapConsumer<rayon::iter::reduce::ReduceConsumer<ruff_cli::commands::run::run::closur
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\slice\mod.rs:732
  36: rayon::iter::plumbing::bridge<rayon::slice::Iter<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,rayon::iter::map::MapConsumer<rayon::iter::reduce::ReduceConsumer<ruff_cli::commands::run::run::closure_env$1,ruff_cli::diagnos
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\plumbing\mod.rs:357
  37: rayon::slice::impl$5::drive_unindexed<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > >,rayon::iter::map::MapConsumer<rayon::iter::reduce::ReduceConsumer<ruff_cli::commands::run::run::closure_env$1,ruff_cli::diagnostics::Diagnos
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\slice\mod.rs:708
  38: rayon::iter::map::impl$2::drive_unindexed<rayon::slice::Iter<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,ruff_cli::commands::run::run::closure_env$0,ruff_cli::diagnostics::Diagnostics,rayon::iter::reduce::ReduceConsumer<
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\map.rs:49
  39: rayon::iter::reduce::reduce<rayon::iter::map::Map<rayon::slice::Iter<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,ruff_cli::commands::run::run::closure_env$0>,ruff_cli::commands::run::run::closure_env$1,ruff_cli::diagnost
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\reduce.rs:15
  40: rayon::iter::ParallelIterator::reduce<rayon::iter::map::Map<rayon::slice::Iter<enum2$<core::result::Result<ignore::walk::DirEntry,enum2$<ignore::Error> > > >,ruff_cli::commands::run::run::closure_env$0>,ruff_cli::commands::run::run::closure_env$1,ruff_cli
             at C:\Users\fish\.cargo\registry\src\github.com-1ecc6299db9ec823\rayon-1.7.0\src\iter\mod.rs:991
  41: ruff_cli::commands::run::run
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\commands\run.rs:79
  42: ruff_cli::check
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\lib.rs:300
  43: ruff_cli::run
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\lib.rs:119
  44: ruff::main
             at C:\Users\fish\source\ruff\crates\ruff_cli\src\bin\ruff.rs:48
  45: core::ops::function::FnOnce::call_once<std::process::ExitCode (*)(),tuple$<> >
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\core\src\ops\function.rs:250
  46: std::sys_common::backtrace::__rust_begin_short_backtrace<std::process::ExitCode (*)(),std::process::ExitCode>
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\std\src\sys_common\backtrace.rs:134
  47: std::rt::lang_start::closure$0<std::process::ExitCode>
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\std\src\rt.rs:166
  48: core::ops::function::impls::impl$2::call_once
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\core\src\ops\function.rs:287
  49: std::panicking::try::do_call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\panicking.rs:487
  50: std::panicking::try
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\panicking.rs:451
  51: std::panic::catch_unwind
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\panic.rs:140
  52: std::rt::lang_start_internal::closure$2
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\rt.rs:148
  53: std::panicking::try::do_call
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\panicking.rs:487
  54: std::panicking::try
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\panicking.rs:451
  55: std::panic::catch_unwind
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\panic.rs:140
  56: std::rt::lang_start_internal
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc/library\std\src\rt.rs:148
  57: std::rt::lang_start<std::process::ExitCode>
             at /rustc/84c898d65adf2f39a5a98507f1fe0ce10a2b8dbc\library\std\src\rt.rs:165
  58: main
  59: invoke_main
             at D:\a\_work\1\s\src\vctools\crt\vcstartup\src\startup\exe_common.inl:78
  60: __scrt_common_main_seh
             at D:\a\_work\1\s\src\vctools\crt\vcstartup\src\startup\exe_common.inl:288
  61: BaseThreadInitThunk
  62: RtlUserThreadStart
```
## Python File
Source https://github.com/python/cpython/tree/167072938342981b96d06d739cd97185207b64dd
**Please change the file extension from txt to py**
[test_ssl.txt](https://github.com/charliermarsh/ruff/files/11452526/test_ssl.txt)
## Ruff settings 
Nothing

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-11 15:53_

---

_Label `bug` added by @charliermarsh on 2023-05-11 15:53_

---

_Comment by @charliermarsh on 2023-05-11 17:03_

Closing in favor of #4374, the root causes are the same.

---

_Closed by @charliermarsh on 2023-05-11 17:03_

---
