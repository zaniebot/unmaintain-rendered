---
number: 9732
title: Rule RET505 cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2024-01-31T07:19:37Z
updated_at: 2024-01-31T22:08:33Z
url: https://github.com/astral-sh/ruff/issues/9732
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule RET505 cause panic

---

_Issue opened by @qarmin on 2024-01-31 07:19_

Ruff 0.1.15 (latest changes from main branch)
```
ruff  *.py --select RET505 --no-cache --fix --unsafe-fixes --preview --isolated
```

file content:
```
class PSP(object):
    def sb(self):
        if self._sb is not None: return self._sb
        else: self._sb = '\033[01;%dm'; self._sa = '\033[0;0m';
```

error
```

error: Panicked while linting /opt/tmp_folder/F_NAME_5271991475256254489.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /home/runner/work/Automated-Fuzzer/Automated-Fuzzer/ruff/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/alloc/src/boxed.rs:2021:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:649:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/sys_common/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:72:14
   7: core::panicking::panic
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/panicking.rs:127:5
   8: ruff_text_size::range::TextRange::new
             at ./ruff/crates/ruff_text_size/src/range.rs:48:9
   9: ruff_linter::rules::flake8_return::rules::function::remove_else
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:853:13
  10: ruff_linter::rules::flake8_return::rules::function::superfluous_else_node::{{closure}}
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:642:25
  11: ruff_diagnostics::diagnostic::Diagnostic::try_set_fix
             at ./ruff/crates/ruff_diagnostics/src/diagnostic.rs:57:15
  12: ruff_linter::rules::flake8_return::rules::function::superfluous_else_node
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:641:21
  13: ruff_linter::rules::flake8_return::rules::function::superfluous_elif_else
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:721:9
  14: ruff_linter::rules::flake8_return::rules::function::function
             at ./ruff/crates/ruff_linter/src/rules/flake8_return/rules/function.rs:758:9
  15: ruff_linter::checkers::ast::analyze::statement::statement
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/statement.rs:227:17
  16: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:833:9
  17: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1434:13
  18: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:645:17
  19: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1434:13
  20: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2068:5
  21: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:153:40
  22: ruff_linter::linter::lint_only
             at ./ruff/crates/ruff_linter/src/linter.rs:397:18
  23: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:323:22
  24: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:194:9
  25: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  26: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  27: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  28: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  29: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:193:18
  30: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:94:17
  31: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/filter_map.rs:123:36
  32: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:179:20
  33: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:110:9
  34: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:438:13
  35: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:397:12
  36: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:373:13
  37: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/slice/mod.rs:732:9
  38: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/plumbing/mod.rs:357:12
  39: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/slice/mod.rs:708:9
  40: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/filter_map.rs:46:9
  41: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/fold.rs:59:9
  42: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/reduce.rs:15:5
  43: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.1/src/iter/mod.rs:991:9
  44: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:165:10
  45: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:405:13
  46: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:201:33
  47: ruff::main
             at ./ruff/crates/ruff/src/main.rs:49:11
  48: core::ops::function::FnOnce::call_once
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:250:5
  49: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/sys_common/backtrace.rs:154:18
  50: std::rt::lang_start::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:167:18
  51: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/core/src/ops/function.rs:284:13
  52: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  53: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  54: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  55: std::rt::lang_start_internal::{{closure}}
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:148:48
  56: std::panicking::try::do_call
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:552:40
  57: std::panicking::try
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panicking.rs:516:19
  58: std::panic::catch_unwind
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/panic.rs:142:14
  59: std::rt::lang_start_internal
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:148:20
  60: std::rt::lang_start
             at /rustc/82e1608dfa6e0b5569232559e3d385fea5a93112/library/std/src/rt.rs:166:17
  61: <unknown>
  62: __libc_start_main
  63: _start


```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/14108471/python_compressed.zip)



---

_Comment by @charliermarsh on 2024-01-31 17:22_

\cc @diceroll123 if interested.

---

_Label `bug` added by @charliermarsh on 2024-01-31 21:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-31 21:59_

---

_Comment by @charliermarsh on 2024-01-31 22:01_

I've got it actually.

---

_Referenced in [astral-sh/ruff#9748](../../astral-sh/ruff/pulls/9748.md) on 2024-01-31 22:02_

---

_Closed by @charliermarsh on 2024-01-31 22:08_

---
