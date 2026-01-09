---
number: 11736
title: Rule F632 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2024-06-04T14:00:17Z
updated_at: 2024-06-05T07:50:35Z
url: https://github.com/astral-sh/ruff/issues/11736
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule F632 cause panic

---

_Issue opened by @qarmin on 2024-06-04 14:00_

ruff 0.4.7 (latest changes from main branch)
```
ruff  *.py --select F632 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
relese_version :"0.0is 64"
```

error
```
All checks passed!

error: Panicked while linting /opt/tmp_folder/11388314438954777037.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_python_parser/src/lib.rs:484:25:
Offset 17 is inside a token range 16..26
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/alloc/src/boxed.rs:2034:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:657:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/panicking.rs:72:14
   7: ruff_python_parser::Tokens::after
             at ./ruff/crates/ruff_python_parser/src/lib.rs:484:25
   8: ruff_python_parser::Tokens::in_range
             at ./ruff/crates/ruff_python_parser/src/lib.rs:436:34
   9: ruff_linter::rules::pyflakes::rules::invalid_literal_comparisons::locate_cmp_ops
             at ./ruff/crates/ruff_linter/src/rules/pyflakes/rules/invalid_literal_comparisons.rs:146:24
  10: ruff_linter::rules::pyflakes::rules::invalid_literal_comparisons::invalid_literal_comparison
             at ./ruff/crates/ruff_linter/src/rules/pyflakes/rules/invalid_literal_comparisons.rs:99:37
  11: ruff_linter::checkers::ast::analyze::expression::expression
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/expression.rs:1280:17
  12: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1465:9
  13: ruff_linter::checkers::ast::Checker::visit_deferred_string_type_definitions
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2186:21
  14: ruff_linter::checkers::ast::Checker::visit_deferred
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2268:13
  15: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2396:5
  16: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:161:40
  17: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:560:22
  18: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:274:14
  19: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:192:9
  20: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  21: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  22: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  23: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  24: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:191:18
  25: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:93:17
  26: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  27: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:20
  28: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:9
  29: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:13
  30: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  31: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  32: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:826:9
  33: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:12
  34: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/slice/mod.rs:802:9
  35: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/filter_map.rs:46:9
  36: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/fold.rs:59:9
  37: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/reduce.rs:15:5
  38: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.10.0/src/iter/mod.rs:998:9
  39: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:163:10
  40: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:429:13
  41: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:202:33
  42: ruff::main
             at ./ruff/crates/ruff/src/main.rs:65:11
  43: core::ops::function::FnOnce::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:250:5
  44: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/sys_common/backtrace.rs:155:18
  45: std::rt::lang_start::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:166:18
  46: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/core/src/ops/function.rs:284:13
  47: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  48: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  49: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  50: std::rt::lang_start_internal::{{closure}}
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:48
  51: std::panicking::try::do_call
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:552:40
  52: std::panicking::try
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panicking.rs:516:19
  53: std::panic::catch_unwind
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/panic.rs:146:14
  54: std::rt::lang_start_internal
             at /rustc/9b00956e56009bab2aa15d7bff10916599e3d6d6/library/std/src/rt.rs:148:20
  55: main
  56: <unknown>
  57: __libc_start_main
  58: _start

```
[python_compressed.zip](https://github.com/user-attachments/files/15552656/python_compressed.zip)




---

_Label `bug` added by @dhruvmanila on 2024-06-04 14:16_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-06-04 14:16_

---

_Comment by @dhruvmanila on 2024-06-04 14:17_

Thanks for the issue! I'll look at it in a bit.

---

_Comment by @dhruvmanila on 2024-06-04 14:54_

Looking into this... I see the problem.

---

_Referenced in [astral-sh/ruff#11740](../../astral-sh/ruff/pulls/11740.md) on 2024-06-04 16:44_

---

_Closed by @dhruvmanila on 2024-06-05 07:50_

---
