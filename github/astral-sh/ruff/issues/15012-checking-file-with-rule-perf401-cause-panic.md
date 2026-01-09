---
number: 15012
title: Checking file with rule PERF401 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - help wanted
  - fuzzer
assignees: []
created_at: 2024-12-16T07:46:57Z
updated_at: 2024-12-17T07:30:39Z
url: https://github.com/astral-sh/ruff/issues/15012
synced_at: 2026-01-07T13:12:16-06:00
---

# Checking file with rule PERF401 cause panic

---

_Issue opened by @qarmin on 2024-12-16 07:46_

ruff 0.8.3+1619 (d84818234 2024-12-15)
```
ruff check *.py --select PERF401 --no-cache  --preview --output-format concise --isolated
```

CC @w0nder1ng - looks that not all panics were fixed by recent PR

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
codons=[]
for nt1 in n:codons.append('')
del nt1
```

error
```
All checks passed!

error: Panicked while linting /tmp/tmp_folder/data/4267921401824228287.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs:243:10:
for loop target must exist
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/alloc/src/boxed.rs:1984:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/panicking.rs:825:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/panicking.rs:690:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/sys/backtrace.rs:168:18
   5: rust_begin_unwind
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/panicking.rs:681:5
   6: core::panicking::panic_fmt
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/core/src/panicking.rs:75:14
   7: core::panicking::panic_display
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/core/src/panicking.rs:261:5
   8: core::option::expect_failed
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/core/src/option.rs:2018:5
   9: core::option::Option<T>::expect
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:933:21
  10: ruff_linter::rules::perflint::rules::manual_list_comprehension::manual_list_comprehension
             at ./ruff/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs:243:10
  11: ruff_linter::checkers::ast::analyze::deferred_for_loops::deferred_for_loops
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/deferred_for_loops.rs:39:17
  12: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2618:5
  13: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:148:36
  14: ruff_linter::linter::lint_only
             at ./ruff/crates/ruff_linter/src/linter.rs:411:23
  15: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:322:22
  16: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:195:9
  17: std::panicking::try::do_call
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:573:40
  18: __rust_try
  19: std::panicking::try
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:536:19
  20: std::panic::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  21: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  22: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:194:18
  23: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:96:17
  24: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  25: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  26: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  27: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  28: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  29: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  30: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:826:9
  31: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  32: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:802:9
  33: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  34: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/fold.rs:59:9
  35: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/reduce.rs:15:5
  36: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/mod.rs:998:9
  37: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:166:10
  38: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:424:13
  39: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:196:33
  40: ruff::main
             at ./ruff/crates/ruff/src/main.rs:44:11
  41: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  42: std::sys::backtrace::__rust_begin_short_backtrace
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152:18
  43: std::rt::lang_start::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:195:18
  44: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/core/src/ops/function.rs:284:13
  45: std::panicking::try::do_call
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/panicking.rs:573:40
  46: std::panicking::try
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/panicking.rs:536:19
  47: std::panic::catch_unwind
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/panic.rs:358:14
  48: std::rt::lang_start_internal::{{closure}}
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/rt.rs:174:48
  49: std::panicking::try::do_call
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/panicking.rs:573:40
  50: std::panicking::try
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/panicking.rs:536:19
  51: std::panic::catch_unwind
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/panic.rs:358:14
  52: std::rt::lang_start_internal
             at /rustc/0aeaa5eb22180fdf12a8489e63c4daa18da6f236/library/std/src/rt.rs:174:20
  53: std::rt::lang_start
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:194:17
  54: <unknown>
  55: __libc_start_main
  56: _start

```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with relase mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/18146489/python_compressed.zip)


---

_Label `bug` added by @MichaReiser on 2024-12-16 07:49_

---

_Label `help wanted` added by @MichaReiser on 2024-12-16 07:49_

---

_Label `fuzzer` added by @dhruvmanila on 2024-12-16 08:04_

---

_Referenced in [astral-sh/ruff#15025](../../astral-sh/ruff/pulls/15025.md) on 2024-12-16 20:54_

---

_Closed by @MichaReiser on 2024-12-17 07:30_

---
