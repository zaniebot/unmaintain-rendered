```yaml
number: 8680
title: Rule UP025 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-11-14T17:46:46Z
updated_at: 2023-11-16T02:30:43Z
url: https://github.com/astral-sh/ruff/issues/8680
synced_at: 2026-01-12T15:54:48Z
```

# Rule UP025 cause panic

---

_@qarmin_


Ruff 0.1.5 (latest changes from main branch)
```
ruff  *.py --select UP025 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
def Lamb2(thoughts, eyes, eye, tongue):
  return f"""
 """+f"{tongue}"U""" 7                         ;\`
       Y   L_    __..--':\`.   L
"""
```

error
```

error: Panicked while linting /home/rafal/test/tmp_folder/F_NAME_6501750325548934365.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/rafal/test/ruff/crates/ruff_source_file/src/locator.rs:396:23:
byte index 75 is not a char boundary; it is inside '\u{94}' (bytes 74..76) of `def Lamb2(thoughts, eyes, eye, tongue):
  return f"""
 """+f"{tongue}"U"""  L
"""`
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/std/src/panicking.rs:711:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/std/src/panicking.rs:599:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/std/src/panicking.rs:595:5
   6: core::panicking::panic_fmt
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/core/src/panicking.rs:67:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/core/src/str/mod.rs:87:9
   9: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::RangeFrom<usize>>::index
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/core/src/str/traits.rs:432:21
  10: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/core/src/str/traits.rs:61:15
  11: ruff_source_file::locator::Locator::after
             at /home/rafal/test/ruff/crates/ruff_source_file/src/locator.rs:396:23
  12: ruff_linter::fix::apply_fixes
             at /home/rafal/test/ruff/crates/ruff_linter/src/fix/mod.rs:120:17
  13: ruff_linter::fix::fix_file
             at /home/rafal/test/ruff/crates/ruff_linter/src/fix/mod.rs:48:14
  14: ruff_linter::linter::lint_fix
             at /home/rafal/test/ruff/crates/ruff_linter/src/linter.rs:519:14
  15: ruff_cli::diagnostics::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/diagnostics.rs:281:14
  16: ruff_cli::commands::check::lint_path::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:199:9
  17: std::panicking::try::do_call
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/std/src/panicking.rs:502:40
  18: std::panicking::try
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/std/src/panicking.rs:466:19
  19: std::panic::catch_unwind
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/std/src/panic.rs:142:14
  20: ruff_cli::panic::catch_unwind
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:40:18
  21: ruff_cli::commands::check::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:198:18
  22: ruff_cli::commands::check::check::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:99:17
  23: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:123:36
  24: rayon::iter::plumbing::Folder::consume_iter
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:179:20
  25: rayon::iter::plumbing::Producer::fold_with
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:110:9
  26: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:438:13
  27: rayon::iter::plumbing::bridge_producer_consumer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:397:12
  28: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:373:13
  29: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:732:9
  30: rayon::iter::plumbing::bridge
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:357:12
  31: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:708:9
  32: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:46:9
  33: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/fold.rs:59:9
  34: rayon::iter::reduce::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/reduce.rs:15:5
  35: rayon::iter::ParallelIterator::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/mod.rs:991:9
  36: ruff_cli::commands::check::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:170:10
  37: ruff_cli::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:397:13
  38: ruff_cli::run
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:188:33
  39: ruff::main
             at /home/rafal/test/ruff/crates/ruff_cli/src/bin/ruff.rs:49:11
  40: core::ops::function::FnOnce::call_once
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/core/src/ops/function.rs:250:5
  41: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/cc66ad468955717ab92600c770da8c1601a4ff33/library/std/src/sys_common/backtrace.rs:154:18
  42: main
  43: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  44: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  45: _start


```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/13353508/python_compressed.zip)



---

_Label `bug` added by @zanieb on 2023-11-14 17:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-16 01:12_

---

_Closed by @charliermarsh on 2023-11-16 02:30_

---
