```yaml
number: 9080
title: Rule D208 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-12-10T12:02:08Z
updated_at: 2023-12-15T17:02:16Z
url: https://github.com/astral-sh/ruff/issues/9080
synced_at: 2026-01-12T15:54:48Z
```

# Rule D208 cause panic

---

_@qarmin_

Ruff 0.1.7 (latest changes from main branch)
```
ruff  *.py --select D208 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
class Sensor(object):
        """ setting a sensor id
           Returns:
        """
```

error
```

error: Panicked while linting /home/rafal/test/tmp_folder/F_NAME_17390143158114535042.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/rafal/test/ruff/crates/ruff_source_file/src/locator.rs:396:23:
byte index 62 is not a char boundary; it is inside '\u{a0}' (bytes 61..63) of `class Sensor(object):
        """ setting a sensor id
           Returns:
        """`
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:735:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:609:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:597:5
   6: core::panicking::panic_fmt
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/panicking.rs:72:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/str/mod.rs:87:9
   9: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::RangeFrom<usize>>::index
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/str/traits.rs:432:21
  10: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/str/traits.rs:61:15
  11: ruff_source_file::locator::Locator::after
             at /home/rafal/test/ruff/crates/ruff_source_file/src/locator.rs:396:23
  12: ruff_linter::rules::pydocstyle::rules::indent::indent
             at /home/rafal/test/ruff/crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs:257:34
  13: ruff_linter::checkers::ast::analyze::definitions::definitions
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/analyze/definitions.rs:224:17
  14: ruff_linter::checkers::ast::check_ast
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2039:5
  15: ruff_linter::linter::check_path
             at /home/rafal/test/ruff/crates/ruff_linter/src/linter.rs:156:40
  16: ruff_linter::linter::lint_fix
             at /home/rafal/test/ruff/crates/ruff_linter/src/linter.rs:487:22
  17: ruff_cli::diagnostics::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/diagnostics.rs:281:14
  18: ruff_cli::commands::check::lint_path::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:199:9
  19: std::panicking::try::do_call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:504:40
  20: std::panicking::try
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:468:19
  21: std::panic::catch_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panic.rs:142:14
  22: ruff_cli::panic::catch_unwind
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:40:18
  23: ruff_cli::commands::check::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:198:18
  24: ruff_cli::commands::check::check::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:99:17
  25: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:123:36
  26: rayon::iter::plumbing::Folder::consume_iter
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:179:20
  27: rayon::iter::plumbing::Producer::fold_with
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:110:9
  28: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:438:13
  29: rayon::iter::plumbing::bridge_producer_consumer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:397:12
  30: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:373:13
  31: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:732:9
  32: rayon::iter::plumbing::bridge
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:357:12
  33: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:708:9
  34: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:46:9
  35: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/fold.rs:59:9
  36: rayon::iter::reduce::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/reduce.rs:15:5
  37: rayon::iter::ParallelIterator::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/mod.rs:991:9
  38: ruff_cli::commands::check::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:170:10
  39: ruff_cli::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:405:13
  40: ruff_cli::run
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:201:33
  41: ruff::main
             at /home/rafal/test/ruff/crates/ruff_cli/src/bin/ruff.rs:49:11
  42: core::ops::function::FnOnce::call_once
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/ops/function.rs:250:5
  43: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/sys_common/backtrace.rs:154:18
  44: std::rt::lang_start::{{closure}}
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/rt.rs:166:18
  45: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/core/src/ops/function.rs:284:13
  46: std::panicking::try::do_call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:504:40
  47: std::panicking::try
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:468:19
  48: std::panic::catch_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panic.rs:142:14
  49: std::rt::lang_start_internal::{{closure}}
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/rt.rs:148:48
  50: std::panicking::try::do_call
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:504:40
  51: std::panicking::try
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panicking.rs:468:19
  52: std::panic::catch_unwind
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/panic.rs:142:14
  53: std::rt::lang_start_internal
             at /rustc/a28077b28a02b92985b3a3faecf92813155f1ea1/library/std/src/rt.rs:148:20
  54: main
  55: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  56: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  57: _start


```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/13627580/python_compressed.zip)


---

_Label `bug` added by @dhruvmanila on 2023-12-11 15:39_

---

_Label `fuzzer` added by @dhruvmanila on 2023-12-11 15:39_

---

_Closed by @charliermarsh on 2023-12-15 17:02_

---
