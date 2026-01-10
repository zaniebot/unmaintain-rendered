---
number: 10546
title: Rule Q000-Q002 started to cause panic
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2024-03-24T12:37:47Z
updated_at: 2024-03-25T18:53:00Z
url: https://github.com/astral-sh/ruff/issues/10546
synced_at: 2026-01-10T01:22:50Z
---

# Rule Q000-Q002 started to cause panic

---

_Issue opened by @qarmin on 2024-03-24 12:37_

ruff 0.3.4
 (latest changes from main branch)
```
ruff  *.py --select Q000 --no-cache --fix --unsafe-fixes --preview --output-format concise --isolated
```

file content:
```
NEWLINE_SEQUENCE: "te.Literal['\\n', '\\r\\n', '\\r']" = "\n"
```

error
```
All checks passed!

error: Panicked while linting /opt/tmp_folder/2097975619179584230.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs:331:47:
begin <= end (1 <= 0) when slicing `C`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/alloc/src/boxed.rs:2029:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:783:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:657:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/sys_common/backtrace.rs:171:18
   5: rust_begin_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:645:5
   6: core::panicking::panic_fmt
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/panicking.rs:72:14
   7: core::str::slice_error_fail_rt
   8: core::str::slice_error_fail
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/str/mod.rs:88:9
   9: core::str::traits::<impl core::slice::index::SliceIndex<str> for core::ops::range::Range<usize>>::index
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/str/traits.rs:231:21
  10: core::str::traits::<impl core::ops::index::Index<I> for str>::index
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/str/traits.rs:61:15
  11: ruff_linter::rules::flake8_quotes::rules::check_string_quotes::strings::{{closure}}
             at ./ruff/crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs:331:47
  12: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::any
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/slice/iter/macros.rs:285:24
  13: ruff_linter::rules::flake8_quotes::rules::check_string_quotes::strings
             at ./ruff/crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs:322:23
  14: ruff_linter::rules::flake8_quotes::rules::check_string_quotes::check_string_quotes
             at ./ruff/crates/ruff_linter/src/rules/flake8_quotes/rules/check_string_quotes.rs:456:9
  15: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  16: ruff_python_ast::visitor::walk_expr
             at ./ruff/crates/ruff_python_ast/src/visitor.rs:546:17
  17: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  18: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
  19: ruff_linter::checkers::ast::Checker::visit_deferred_string_type_definitions
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2020:21
  20: ruff_linter::checkers::ast::Checker::visit_deferred
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2094:13
  21: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2206:5
  22: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:156:40
  23: ruff_linter::linter::lint_fix
             at ./ruff/crates/ruff_linter/src/linter.rs:544:22
  24: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:280:14
  25: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:194:9
  26: std::panicking::try::do_call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:552:40
  27: std::panicking::try
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:516:19
  28: std::panic::catch_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panic.rs:142:14
  29: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  30: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:193:18
  31: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:94:17
  32: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/filter_map.rs:123:36
  33: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/plumbing/mod.rs:179:20
  34: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/plumbing/mod.rs:110:9
  35: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/plumbing/mod.rs:438:13
  36: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/plumbing/mod.rs:397:12
  37: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/plumbing/mod.rs:373:13
  38: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/slice/mod.rs:777:9
  39: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/plumbing/mod.rs:357:12
  40: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/slice/mod.rs:753:9
  41: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/filter_map.rs:46:9
  42: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/fold.rs:59:9
  43: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/reduce.rs:15:5
  44: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.9.0/src/iter/mod.rs:998:9
  45: ruff::commands::check::check
             at ./ruff/crates/ruff/src/commands/check.rs:165:10
  46: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:415:13
  47: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:192:33
  48: ruff::main
             at ./ruff/crates/ruff/src/main.rs:65:11
  49: core::ops::function::FnOnce::call_once
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/ops/function.rs:250:5
  50: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/sys_common/backtrace.rs:155:18
  51: std::rt::lang_start::{{closure}}
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/rt.rs:166:18
  52: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/core/src/ops/function.rs:284:13
  53: std::panicking::try::do_call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:552:40
  54: std::panicking::try
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:516:19
  55: std::panic::catch_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panic.rs:142:14
  56: std::rt::lang_start_internal::{{closure}}
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/rt.rs:148:48
  57: std::panicking::try::do_call
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:552:40
  58: std::panicking::try
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panicking.rs:516:19
  59: std::panic::catch_unwind
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/panic.rs:142:14
  60: std::rt::lang_start_internal
             at /rustc/07dca489ac2d933c78d3c5158e3f43beefeb02ce/library/std/src/rt.rs:148:20
  61: main
  62: <unknown>
  63: __libc_start_main
  64: _start

```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/14735467/python_compressed.zip)


Probably caused by #10312

CC @dhruvmanila 


---

_Label `bug` added by @dhruvmanila on 2024-03-24 15:19_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-03-24 15:19_

---

_Comment by @dhruvmanila on 2024-03-24 15:19_

Thanks! Let me look at it tonight.

---

_Comment by @dhruvmanila on 2024-03-24 17:58_

This is happening because the checker is analyzing the string _inside_ the annotation (three strings inside `Literal`). The ranges aren't correct in that case. We need to find a holistic solution to this because it's happening to _all_ rules working on strings.

For example, here `UP025` is triggered for the `u'\\n'` which is inside the string type annotation.

```
/Users/dhruv/playground/ruff/src/UP025.py:1:12: UP025 [*] Remove unicode literals from strings
  |
1 | NEWLINE_SEQUENCE: "te.Literal[u'\\n', '\\r\\n', '\\r']" = "\n"
  |            ^^^^^ UP025
  |
  = help: Remove unicode prefix

ℹ Safe fix
1   |-NEWLINE_SEQUENCE: "te.Literal[u'\\n', '\\r\\n', '\\r']" = "\n"
  1 |+NEWLINE_SEQENCE: "te.Literal[u'\\n', '\\r\\n', '\\r']" = "\n"

Found 1 error.
[*] 1 fixable with the --fix option.
```

We could potentially fix the range for the rules to be useful in the context of type annotation or ignore them completely. Nevertheless, we should completely avoid rules which might update the quotes because otherwise we need the context of the quote which contains the actual type annotation.

---

_Referenced in [astral-sh/ruff#10574](../../astral-sh/ruff/issues/10574.md) on 2024-03-25 17:10_

---

_Referenced in [astral-sh/ruff#10585](../../astral-sh/ruff/pulls/10585.md) on 2024-03-25 18:32_

---

_Referenced in [astral-sh/ruff#10586](../../astral-sh/ruff/issues/10586.md) on 2024-03-25 18:33_

---

_Closed by @dhruvmanila on 2024-03-25 18:53_

---
