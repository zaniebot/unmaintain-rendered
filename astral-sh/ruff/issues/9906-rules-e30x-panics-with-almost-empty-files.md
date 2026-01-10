```yaml
number: 9906
title: Rules E30X panics with almost empty files
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2024-02-09T06:48:32Z
updated_at: 2024-02-09T14:26:25Z
url: https://github.com/astral-sh/ruff/issues/9906
synced_at: 2026-01-10T11:09:52Z
```

# Rules E30X panics with almost empty files

---

_Issue opened by @qarmin on 2024-02-09 06:48_

Ruff ruff 0.2.1 (latest changes from main branch)
```
ruff  *.py --select E301 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```

	
```

error
```
error: Panicked while linting /opt/tmp_folder/F_NAME_774356372734619814.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs:504:17:
assertion `left == right` failed
  left: 1
 right: 2
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:657:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:72:14
   7: core::panicking::assert_failed_inner
   8: core::panicking::assert_failed
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:279:5
   9: ruff_linter::rules::pycodestyle::rules::blank_lines::BlankLines::add
             at ./ruff/crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs:504:17
  10: <ruff_linter::rules::pycodestyle::rules::blank_lines::LinePreprocessor as core::iter::traits::iterator::Iterator>::next
             at ./ruff/crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs:398:21
  11: ruff_linter::rules::pycodestyle::rules::blank_lines::BlankLinesChecker::check_lines
             at ./ruff/crates/ruff_linter/src/rules/pycodestyle/rules/blank_lines.rs:632:29
  12: ruff_linter::checkers::tokens::check_tokens
             at ./ruff/crates/ruff_linter/src/checkers/tokens.rs:48:9
  13: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:107:28
  14: ruff_linter::linter::lint_only
             at ./ruff/crates/ruff_linter/src/linter.rs:447:18
  15: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:323:22
  16: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:194:9
  17: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  18: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  19: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  20: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  21: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:193:18
  22: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:94:17
  23: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/filter_map.rs:123:36
  24: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:179:20
  25: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:110:9
  26: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:438:13
  27: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:397:12
  28: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:373:13
  29: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/slice/mod.rs:732:9
  30: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:357:12
  31: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/slice/mod.rs:708:9
  32: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/filter_map.rs:46:9
  33: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/fold.rs:59:9
  34: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/reduce.rs:15:5
  35: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/mod.rs:991:9
  36: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:165:10
  37: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:410:13
  38: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:201:33
  39: ruff::main
             at ./ruff/crates/ruff/src/main.rs:49:11
  40: core::ops::function::FnOnce::call_once
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:250:5
  41: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/sys_common/backtrace.rs:154:18
  42: std::rt::lang_start::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:167:18
  43: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:284:13
  44: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  45: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  46: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  47: std::rt::lang_start_internal::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:148:48
  48: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  49: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  50: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  51: std::rt::lang_start_internal
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:148:20
  52: std::rt::lang_start
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:166:17
  53: <unknown>
  54: __libc_start_main
  55: _start


```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/14218158/python_compressed.zip)

903 example files - [reports-ONLY_CHECK_1.zip](https://github.com/astral-sh/ruff/files/14218160/reports-ONLY_CHECK_1.zip)

CC @hoel-bagard 

---

_Label `bug` added by @zanieb on 2024-02-09 08:10_

---

_Comment by @zanieb on 2024-02-09 08:12_

ref https://github.com/astral-sh/ruff/pull/9266

---

_Comment by @hoel-bagard on 2024-02-09 09:29_

Thanks for providing files to reproduce the issue! Hopefully it will be fixed soon.

---

_Comment by @hoel-bagard on 2024-02-09 14:07_

The issue should have been fixed with https://github.com/astral-sh/ruff/pull/9907.

---

_Closed by @qarmin on 2024-02-09 14:26_

---
