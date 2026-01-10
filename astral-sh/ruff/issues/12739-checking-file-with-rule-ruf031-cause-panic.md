---
number: 12739
title: Checking file with rule RUF031 cause panic (probably only in debug build)
type: issue
state: closed
author: qarmin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-08T05:10:35Z
updated_at: 2024-08-08T11:18:04Z
url: https://github.com/astral-sh/ruff/issues/12739
synced_at: 2026-01-10T01:22:52Z
---

# Checking file with rule RUF031 cause panic (probably only in debug build)

---

_Issue opened by @qarmin on 2024-08-08 05:10_

ruff 0.5.6+445 (a631d600a 2024-08-07)
```
ruff check *.py --select RUF031 --no-cache  --preview --output-format concise --isolated
```

file content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
r[()]
```

error
```
All checks passed!

error: Panicked while linting /tmp/tmp_folder/data/2980733090704608878.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_diagnostics/src/edit.rs:42:9:
Prefer `Fix::deletion`
Backtrace:    0: ruff::panic::catch_unwind::{{closure}}
             at ./ruff/crates/ruff/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/alloc/src/boxed.rs:2164:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panicking.rs:805:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panicking.rs:664:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/sys/backtrace.rs:170:18
   5: rust_begin_unwind
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panicking.rs:662:5
   6: core::panicking::panic_fmt
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/core/src/panicking.rs:74:14
   7: ruff_diagnostics::edit::Edit::range_replacement
             at ./ruff/crates/ruff_diagnostics/src/edit.rs:42:9
   8: ruff_linter::rules::ruff::rules::incorrectly_parenthesized_tuple_in_subscript::subscript_with_parenthesized_tuple
             at ./ruff/crates/ruff_linter/src/rules/ruff/rules/incorrectly_parenthesized_tuple_in_subscript.rs:74:16
   9: ruff_linter::checkers::ast::analyze::expression::expression
             at ./ruff/crates/ruff_linter/src/checkers/ast/analyze/expression.rs:150:17
  10: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1486:9
  11: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
  12: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:1608:13
  13: ruff_linter::checkers::ast::check_ast
             at ./ruff/crates/ruff_linter/src/checkers/ast/mod.rs:2410:5
  14: ruff_linter::linter::check_path
             at ./ruff/crates/ruff_linter/src/linter.rs:146:36
  15: ruff_linter::linter::lint_only
             at ./ruff/crates/ruff_linter/src/linter.rs:409:23
  16: ruff::diagnostics::lint_path
             at ./ruff/crates/ruff/src/diagnostics.rs:321:22
  17: ruff::commands::check::lint_path::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:192:9
  18: std::panicking::try::do_call
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panicking.rs:554:40
  19: __rust_try
  20: std::panicking::try
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panicking.rs:518:19
  21: std::panic::catch_unwind
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panic.rs:345:14
  22: ruff::panic::catch_unwind
             at ./ruff/crates/ruff/src/panic.rs:40:18
  23: ruff::commands::check::lint_path
             at ./ruff/crates/ruff/src/commands/check.rs:191:18
  24: ruff::commands::check::check::{{closure}}
             at ./ruff/crates/ruff/src/commands/check.rs:93:17
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
             at ./ruff/crates/ruff/src/commands/check.rs:163:10
  39: ruff::check
             at ./ruff/crates/ruff/src/lib.rs:404:13
  40: ruff::run
             at ./ruff/crates/ruff/src/lib.rs:186:33
  41: ruff::main
             at ./ruff/crates/ruff/src/main.rs:85:11
  42: core::ops::function::FnOnce::call_once
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/core/src/ops/function.rs:250:5
  43: std::sys::backtrace::__rust_begin_short_backtrace
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/sys/backtrace.rs:154:18
  44: std::rt::lang_start::{{closure}}
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/rt.rs:164:18
  45: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/core/src/ops/function.rs:284:13
  46: std::panicking::try::do_call
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panicking.rs:554:40
  47: std::panicking::try
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panicking.rs:518:19
  48: std::panic::catch_unwind
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panic.rs:345:14
  49: std::rt::lang_start_internal::{{closure}}
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/rt.rs:143:48
  50: std::panicking::try::do_call
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panicking.rs:554:40
  51: std::panicking::try
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panicking.rs:518:19
  52: std::panic::catch_unwind
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/panic.rs:345:14
  53: std::rt::lang_start_internal
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/rt.rs:143:20
  54: std::rt::lang_start
             at /rustc/60d146580c10036ce89e019422c6bc2fd9729b65/library/std/src/rt.rs:163:17
  55: <unknown>
  56: __libc_start_main
  57: _start

```

Ruff build, that was used to reproduce problem(compiled on Ubuntu 22.04 with relase mode + debug symbols + debug assertions + overflow checks) - https://github.com/qarmin/Automated-Fuzzer/releases/download/Nightly/ruff.7z

[python_compressed.zip](https://github.com/user-attachments/files/16537923/python_compressed.zip)

---

_Comment by @MichaReiser on 2024-08-08 05:51_

@dylwil3 are you interested in taking a look at this? The fix itself should be easy. The main question is if the rule should flag empty tuples or not.

---

_Label `bug` added by @MichaReiser on 2024-08-08 05:51_

---

_Label `preview` added by @MichaReiser on 2024-08-08 05:51_

---

_Comment by @dylwil3 on 2024-08-08 10:40_

Sure, I'd be happy to! I would vote for skipping over empty tuples, since it's not clear what a fix would be, but I defer to your judgment.

---

_Comment by @MichaReiser on 2024-08-08 10:49_

That makes sense to me

---

_Assigned to @dylwil3 by @MichaReiser on 2024-08-08 10:49_

---

_Referenced in [astral-sh/ruff#12749](../../astral-sh/ruff/pulls/12749.md) on 2024-08-08 11:08_

---

_Comment by @dylwil3 on 2024-08-08 11:11_

Apologies for missing this case originally, and thanks @qarmin for finding this bug!

---

_Closed by @MichaReiser on 2024-08-08 11:18_

---
