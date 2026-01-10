```yaml
number: 11102
title: Rule PGH004 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-04-23T09:48:17Z
updated_at: 2024-04-25T18:39:40Z
url: https://github.com/astral-sh/ruff/issues/11102
synced_at: 2026-01-10T11:09:53Z
```

# Rule PGH004 cause panic

---

_Issue opened by @qarmin on 2024-04-23 09:48_

ruff 0.4.1 (latest changes from main branch)
```
ruff  *.py --select PGH004 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
#noqa
```

error
```
All checks passed!

error: Panicked while linting /opt/tmp_folder/10704147344890583060.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_source_file/src/locator.rs:463:23:
byte index 6 is out of bounds of `#noqa`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/alloc/src/boxed.rs:2029:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:785:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:659:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:647:5
   6: core::panicking::panic_fmt
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/panicking.rs:72:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/str/mod.rs:91:9
   9: ruff_linter::rules::pygrep_hooks::rules::blanket_noqa::blanket_noqa
  10: ruff_linter::checkers::noqa::check_noqa
             at ./ruff/crates/ruff_linter/src/checkers/noqa.rs:208:9
  11: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:290:23
  12: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:550:22
  13: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:280:14
  14: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:194:9
  15: std::panicking::try::do_call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:554:40
  16: std::panicking::try
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:518:19
  17: std::panic::catch_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panic.rs:142:14
  18: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  19: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:193:18
  20: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:94:17
  21: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  22: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  23: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  24: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  25: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  26: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  27: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  28: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  29: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  30: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  31: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  32: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  33: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  34: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:165:10
  35: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:422:13
  36: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:199:33
  37: ruff::main
             at ./ruff/crates/ruff/src/main.rs:65:11
  38: core::ops::function::FnOnce::call_once
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/ops/function.rs:250:5
  39: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/sys_common/backtrace.rs:155:18
  40: std::rt::lang_start::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/rt.rs:166:18
  41: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/core/src/ops/function.rs:284:13
  42: std::panicking::try::do_call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:554:40
  43: std::panicking::try
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:518:19
  44: std::panic::catch_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panic.rs:142:14
  45: std::rt::lang_start_internal::{{closure}}
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/rt.rs:148:48
  46: std::panicking::try::do_call
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:554:40
  47: std::panicking::try
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panicking.rs:518:19
  48: std::panic::catch_unwind
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/panic.rs:142:14
  49: std::rt::lang_start_internal
             at /rustc/25ef9e3d85d934b27d9dada2f9dd52b1dc63bb04/library/std/src/rt.rs:148:20
  50: main
  51: <unknown>
  52: __libc_start_main
  53: _start

```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/15074839/python_compressed.zip)

Probably regression from #11053 CC @augustelalande


---

_Label `fuzzer` added by @AlexWaygood on 2024-04-23 10:09_

---

_Label `bug` added by @AlexWaygood on 2024-04-23 10:10_

---

_Comment by @augustelalande on 2024-04-23 11:58_

I will look into it. Thanks.

---

_Assigned to @augustelalande by @charliermarsh on 2024-04-23 13:02_

---

_Closed by @charliermarsh on 2024-04-25 18:39_

---
