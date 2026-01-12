```yaml
number: 11692
title: Rule UP032 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2024-06-02T12:50:55Z
updated_at: 2024-06-02T17:51:05Z
url: https://github.com/astral-sh/ruff/issues/11692
synced_at: 2026-01-12T15:54:51Z
```

# Rule UP032 cause panic

---

_@qarmin_

ruff 0.4.7 (latest changes from main branch)
```
ruff  *.py --select UP032 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
''.format(self.project)
```

error
```
All checks passed!

error: Panicked while linting /opt/tmp_folder/10231309488158600143.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_diagnostics/src/edit.rs:42:9:
Prefer `Fix::deletion`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/alloc/src/boxed.rs:2034:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:649:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/panicking.rs:72:14
   7: ruff_diagnostics::edit::Edit::range_replacement
             at ./ruff/crates/ruff_diagnostics/src/edit.rs:42:9
   8: ruff_linter::rules::pyupgrade::rules::f_strings::f_strings
             at ./ruff/crates/ruff_linter/src/rules/pyupgrade/rules/f_strings.rs:523:43
   9: ruff_linter::checkers::ast::analyze::expression::expression
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/expression.rs:439:41
  10: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1459:9
  11: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:945:18
  12: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1581:13
  13: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2381:5
  14: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:154:40
  15: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:541:22
  16: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:274:14
  17: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:192:9
  18: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  19: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  20: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  21: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  22: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:191:18
  23: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:93:17
  24: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  25: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  26: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  27: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  28: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  29: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  30: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  31: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  32: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  33: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  34: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  35: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  36: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  37: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:163:10
  38: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:429:13
  39: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:202:33
  40: ruff::main
             at ./ruff/crates/ruff/src/main.rs:65:11
  41: core::ops::function::FnOnce::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:250:5
  42: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/sys_common/backtrace.rs:155:18
  43: std::rt::lang_start::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:166:18
  44: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:284:13
  45: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  46: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  47: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  48: std::rt::lang_start_internal::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:48
  49: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  50: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  51: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  52: std::rt::lang_start_internal
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:20
  53: main
  54: <unknown>
  55: __libc_start_main
  56: _start

```
[python_compressed.zip](https://github.com/user-attachments/files/15525352/python_compressed.zip)




---

_Comment by @charliermarsh on 2024-06-02 17:35_

This is only in dev, thankfully, but I will fix.

---

_Label `bug` added by @charliermarsh on 2024-06-02 17:35_

---

_Label `fuzzer` added by @charliermarsh on 2024-06-02 17:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-02 17:42_

---

_Closed by @charliermarsh on 2024-06-02 17:51_

---
