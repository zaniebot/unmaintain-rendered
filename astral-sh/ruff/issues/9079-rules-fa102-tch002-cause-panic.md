```yaml
number: 9079
title: Rules FA102, TCH002 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-12-10T12:01:25Z
updated_at: 2023-12-20T17:07:43Z
url: https://github.com/astral-sh/ruff/issues/9079
synced_at: 2026-01-10T11:09:51Z
```

# Rules FA102, TCH002 cause panic

---

_Issue opened by @qarmin on 2023-12-10 12:01_


Ruff 0.1.7 (latest changes from main branch)
```
ruff  *.py --select FA102,TCH002 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
import typing_extensions
class FunctionSubscriber:
    _functions:list[Callable]
    def __iadd__(self,fn:Callable) -> typing_extensions.Self:
            self._functions.append(fn)
```

error
```

error: Panicked while linting /home/rafal/test/tmp_folder/F_NAME_18340810135824902508.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/rafal/test/ruff/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:735:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:601:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:597:5
   6: core::panicking::panic_fmt
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/panicking.rs:72:14
   7: core::panicking::panic
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/panicking.rs:127:5
   8: ruff_linter::fix::apply_fixes
   9: ruff_linter::fix::fix_file
             at /home/rafal/test/ruff/crates/ruff_linter/src/fix/mod.rs:48:14
  10: ruff_linter::linter::lint_fix
             at /home/rafal/test/ruff/crates/ruff_linter/src/linter.rs:523:14
  11: ruff_cli::diagnostics::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/diagnostics.rs:281:14
  12: ruff_cli::commands::check::lint_path::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:199:9
  13: std::panicking::try::do_call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:504:40
  14: std::panicking::try
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:468:19
  15: std::panic::catch_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panic.rs:142:14
  16: ruff_cli::panic::catch_unwind
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:40:18
  17: ruff_cli::commands::check::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:198:18
  18: ruff_cli::commands::check::check::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:99:17
  19: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:123:36
  20: rayon::iter::plumbing::Folder::consume_iter
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:179:20
  21: rayon::iter::plumbing::Producer::fold_with
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:110:9
  22: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:438:13
  23: rayon::iter::plumbing::bridge_producer_consumer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:397:12
  24: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:373:13
  25: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:732:9
  26: rayon::iter::plumbing::bridge
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:357:12
  27: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:708:9
  28: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:46:9
  29: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/fold.rs:59:9
  30: rayon::iter::reduce::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/reduce.rs:15:5
  31: rayon::iter::ParallelIterator::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/mod.rs:991:9
  32: ruff_cli::commands::check::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:170:10
  33: ruff_cli::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:405:13
  34: ruff_cli::run
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:201:33
  35: ruff::main
             at /home/rafal/test/ruff/crates/ruff_cli/src/bin/ruff.rs:49:11
  36: core::ops::function::FnOnce::call_once
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/ops/function.rs:250:5
  37: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/sys_common/backtrace.rs:154:18
  38: std::rt::lang_start::{{closure}}
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/rt.rs:166:18
  39: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/ops/function.rs:284:13
  40: std::panicking::try::do_call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:504:40
  41: std::panicking::try
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:468:19
  42: std::panic::catch_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panic.rs:142:14
  43: std::rt::lang_start_internal::{{closure}}
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/rt.rs:148:48
  44: std::panicking::try::do_call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:504:40
  45: std::panicking::try
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:468:19
  46: std::panic::catch_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panic.rs:142:14
  47: std::rt::lang_start_internal
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/rt.rs:148:20
  48: main
  49: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  50: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  51: _start


```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13627577/python_compressed.zip)


---

_Label `bug` added by @dhruvmanila on 2023-12-11 15:40_

---

_Label `fuzzer` added by @dhruvmanila on 2023-12-11 15:40_

---

_Comment by @charliermarsh on 2023-12-20 17:07_

This appears to be fixed on main (but is reproducible on v0.1.7, so assume it was fixed).

---

_Closed by @charliermarsh on 2023-12-20 17:07_

---
