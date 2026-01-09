---
number: 14969
title: Checking file with rule PERF401 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-12-14T08:48:49Z
updated_at: 2024-12-15T15:22:05Z
url: https://github.com/astral-sh/ruff/issues/14969
synced_at: 2026-01-07T13:12:16-06:00
---

# Checking file with rule PERF401 cause panic

---

_Issue opened by @qarmin on 2024-12-14 08:48_

ruff 0.8.3+1608 (224c8438b 2024-12-13)

probably regression from https://github.com/astral-sh/ruff/pull/14369 
CC @w0nder1ng

```
ruff check *.py --select PERF401 --no-cache  --preview --output-format concise --isolated
```

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
def n():
 for x in c:l.append(())
l=[]
```

error
```
All checks passed!

error: Panicked while linting /tmp/tmp_folder/data/7775264315287514819.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/alloc/src/boxed.rs:1984:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/panicking.rs:825:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/panicking.rs:683:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/sys/backtrace.rs:168:18
   5: rust_begin_unwind
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/panicking.rs:681:5
   6: core::panicking::panic_fmt
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/core/src/panicking.rs:75:14
   7: core::panicking::panic
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/core/src/panicking.rs:145:5
   8: ruff_text_size::range::TextRange::new
             at ./ruff/crates/ruff_text_size/src/range.rs:48:9
   9: ruff_linter::rules::perflint::rules::manual_list_comprehension::manual_list_comprehension::{{closure}}
             at ./ruff/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs:309:35
  10: core::option::Option<T>::is_some_and
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/option.rs:631:24
  11: ruff_linter::rules::perflint::rules::manual_list_comprehension::manual_list_comprehension
             at ./ruff/crates/ruff_linter/src/rules/perflint/rules/manual_list_comprehension.rs:308:52
  12: ruff_linter::checkers::ast::analyze::deferred_for_loops::deferred_for_loops
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/deferred_for_loops.rs:39:17
  13: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2618:5
  14: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:148:36
  15: ruff_linter::linter::lint_only
             at ./ruff/crates/ruff_linter/src/linter.rs:411:23
  16: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:322:22
  17: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:195:9
  18: std::panicking::try::do_call
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:573:40
  19: __rust_try
  20: std::panicking::try
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:536:19
  21: std::panic::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:358:14
  22: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  23: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:194:18
  24: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:96:17
  25: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  26: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  27: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  28: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  29: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  30: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  31: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  32: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  33: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  34: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  35: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  36: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  37: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  38: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:166:10
  39: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:424:13
  40: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:196:33
  41: ruff::main
             at ./ruff/crates/ruff/src/main.rs:44:11
  42: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  43: std::sys::backtrace::__rust_begin_short_backtrace
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152:18
  44: std::rt::lang_start::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:195:18
  45: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/core/src/ops/function.rs:284:13
  46: std::panicking::try::do_call
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/panicking.rs:573:40
  47: std::panicking::try
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/panicking.rs:536:19
  48: std::panic::catch_unwind
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/panic.rs:358:14
  49: std::rt::lang_start_internal::{{closure}}
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/rt.rs:174:48
  50: std::panicking::try::do_call
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/panicking.rs:573:40
  51: std::panicking::try
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/panicking.rs:536:19
  52: std::panic::catch_unwind
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/panic.rs:358:14
  53: std::rt::lang_start_internal
             at /rustc/327c7ee4367ea587a49eff1d4715f462ab6db5f0/library/std/src/rt.rs:174:20
  54: std::rt::lang_start
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:194:17
  55: <unknown>
  56: __libc_start_main
  57: _start

```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with relase mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/18135159/python_compressed.zip)


---

_Label `bug` added by @AlexWaygood on 2024-12-14 13:28_

---

_Label `help wanted` added by @AlexWaygood on 2024-12-14 13:29_

---

_Referenced in [astral-sh/ruff#14971](../../astral-sh/ruff/pulls/14971.md) on 2024-12-14 15:57_

---

_Closed by @MichaReiser on 2024-12-15 15:22_

---
