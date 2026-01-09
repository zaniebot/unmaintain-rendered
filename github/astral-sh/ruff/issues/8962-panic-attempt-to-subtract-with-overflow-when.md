---
number: 8962
title: "Panic `attempt to subtract with overflow` when checking code"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-12-02T11:08:46Z
updated_at: 2023-12-02T13:56:58Z
url: https://github.com/astral-sh/ruff/issues/8962
synced_at: 2026-01-07T13:12:15-06:00
---

# Panic `attempt to subtract with overflow` when checking code

---

_Issue opened by @qarmin on 2023-12-02 11:08_

Ruff 0.1.6 nightly version - quite new regression probably from https://github.com/astral-sh/ruff/pull/8917
```
ruff . --select ALL
```

```
class ImportSetupActionBackend(
):
    def items_save(
    ):
        DownloadFile = apps.get_model(
        )
```

[F_NAME_7672771860829446668.py.zip](https://github.com/astral-sh/ruff/files/13536089/F_NAME_7672771860829446668.py.zip)

Panics with message
```
error: Panicked while linting F_NAME_7672771860829446668.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/pep8_naming/helpers.rs:108:59:
attempt to subtract with overflow
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:735:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:601:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:597:5
   6: core::panicking::panic_fmt
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/panicking.rs:72:14
   7: core::panicking::panic
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/panicking.rs:127:5
   8: ruff_linter::rules::pep8_naming::helpers::is_django_model_import::match_model_import
   9: ruff_linter::rules::pep8_naming::helpers::is_django_model_import
  10: ruff_linter::rules::pep8_naming::rules::non_lowercase_variable_in_function::non_lowercase_variable_in_function
             at /home/rafal/test/ruff/crates/ruff_linter/src/rules/pep8_naming/rules/non_lowercase_variable_in_function.rs:84:12
  11: ruff_linter::checkers::ast::analyze::expression::expression
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/analyze/expression.rs:208:29
  12: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1273:9
  13: ruff_python_ast::visitor::walk_stmt
             at /home/rafal/test/ruff/crates/ruff_python_ast/src/visitor.rs:193:17
  14: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:767:18
  15: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1410:13
  16: ruff_linter::checkers::ast::Checker::visit_deferred_functions
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1879:17
  17: ruff_linter::checkers::ast::check_ast
             at /home/rafal/test/ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2024:5
  18: ruff_linter::linter::check_path
             at /home/rafal/test/ruff/crates/ruff_linter/src/linter.rs:156:40
  19: ruff_linter::linter::lint_only
             at /home/rafal/test/ruff/crates/ruff_linter/src/linter.rs:391:18
  20: ruff_cli::diagnostics::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/diagnostics.rs:311:22
  21: ruff_cli::commands::check::lint_path::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:199:9
  22: std::panicking::try::do_call
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:504:40
  23: std::panicking::try
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panicking.rs:468:19
  24: std::panic::catch_unwind
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/panic.rs:142:14
  25: ruff_cli::panic::catch_unwind
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:40:18
  26: ruff_cli::commands::check::lint_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:198:18
  27: ruff_cli::commands::check::check::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:99:17
  28: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:123:36
  29: rayon::iter::plumbing::Folder::consume_iter
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:179:20
  30: rayon::iter::plumbing::Producer::fold_with
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:110:9
  31: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:438:13
  32: rayon::iter::plumbing::bridge_producer_consumer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:397:12
  33: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:373:13
  34: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:732:9
  35: rayon::iter::plumbing::bridge
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:357:12
  36: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/slice/mod.rs:708:9
  37: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:46:9
  38: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/fold.rs:59:9
  39: rayon::iter::reduce::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/reduce.rs:15:5
  40: rayon::iter::ParallelIterator::reduce
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/mod.rs:991:9
  41: ruff_cli::commands::check::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/check.rs:170:10
  42: ruff_cli::check
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:405:13
  43: ruff_cli::run
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:201:33
  44: ruff::main
             at /home/rafal/test/ruff/crates/ruff_cli/src/bin/ruff.rs:49:11
  45: core::ops::function::FnOnce::call_once
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/core/src/ops/function.rs:250:5
  46: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/79e9716c980570bfd1f666e3b16ac583f0168962/library/std/src/sys_common/backtrace.rs:154:18
  47: main
  48: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  49: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  50: _start
```

---

_Label `bug` added by @charliermarsh on 2023-12-02 13:09_

---

_Comment by @charliermarsh on 2023-12-02 13:09_

Thanks, good catch.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-02 13:51_

---

_Referenced in [astral-sh/ruff#8965](../../astral-sh/ruff/pulls/8965.md) on 2023-12-02 13:51_

---

_Closed by @charliermarsh on 2023-12-02 13:56_

---
