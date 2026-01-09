---
number: 19751
title: "Panic `f-string: unexpected token TStringMiddle`"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - parser
assignees: []
created_at: 2025-08-05T05:50:24Z
updated_at: 2025-10-15T07:50:58Z
url: https://github.com/astral-sh/ruff/issues/19751
synced_at: 2026-01-07T13:12:16-06:00
---

# Panic `f-string: unexpected token TStringMiddle`

---

_Issue opened by @qarmin on 2025-08-05 05:50_

Quite similar to https://github.com/astral-sh/ruff/issues/18860

File content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
_='''{s''T'{: '''
```

command
```
timeout -v 300 ruff check TEST___FILE.py --select ALL --preview --output-format concise --no-cache --fix --unsafe-fixes --isolated
```

cause this
```
All checks passed!

warning: `incorrect-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `incorrect-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
error: Panicked while linting /opt/BROKEN_FILES_DIR/917_IDX_0_RAND_244014174919259028981136_minimized_366.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_python_parser/src/parser/expression.rs:1695:25:
internal error: entered unreachable code: f-string: unexpected token `TStringMiddle` at 12..13
Backtrace:    0: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at ./ruff-main/crates/ruff_db/src/panic.rs:91:34
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/alloc/src/boxed.rs:1985:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/panicking.rs:841:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/panicking.rs:706:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/sys/backtrace.rs:174:18
   5: __rustc::rust_begin_unwind
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/panicking.rs:697:5
   6: core::panicking::panic_fmt
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/core/src/panicking.rs:75:14
   7: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_interpolated_string_elements::{{closure}}
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1695:25
   8: ruff_python_parser::parser::Parser::parse_list
             at ./ruff-main/crates/ruff_python_parser/src/parser/mod.rs:508:17
   9: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_interpolated_string_elements
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1652:14
  10: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_interpolated_element
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1841:33
  11: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_interpolated_string_elements::{{closure}}
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1657:32
  12: ruff_python_parser::parser::Parser::parse_list
             at ./ruff-main/crates/ruff_python_parser/src/parser/mod.rs:508:17
  13: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_interpolated_string_elements
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1652:14
  14: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_interpolated_string
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1532:29
  15: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_strings
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1248:26
  16: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_atom
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:612:22
  17: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_lhs_expression
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:413:24
  18: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_binary_expression_or_higher
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:248:24
  19: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_simple_expression
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:236:14
  20: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_conditional_expression_or_higher_impl
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:213:36
  21: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_expression_list
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:151:32
  22: ruff_python_parser::parser::Parser::parse_single_expression
             at ./ruff-main/crates/ruff_python_parser/src/parser/mod.rs:109:32
  23: ruff_python_parser::parser::Parser::parse
             at ./ruff-main/crates/ruff_python_parser/src/parser/mod.rs:91:38
  24: ruff_python_parser::parse_expression
             at ./ruff-main/crates/ruff_python_parser/src/lib.rs:141:10
  25: ruff_linter::rules::ruff::rules::missing_fstring_syntax::should_be_fstring
             at ./ruff-main/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs:188:22
  26: ruff_linter::rules::ruff::rules::missing_fstring_syntax::missing_fstring_syntax
             at ./ruff-main/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs:118:8
  27: ruff_linter::checkers::ast::analyze::expression::expression
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/analyze/expression.rs:1646:21
  28: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:2017:9
  29: ruff_python_ast::visitor::walk_stmt
             at ./ruff-main/crates/ruff_python_ast/src/visitor.rs:207:21
  30: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:1381:18
  31: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:2131:18
  32: ruff_linter::checkers::ast::check_ast
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:3103:13
  33: ruff_linter::linter::check_path
             at ./ruff-main/crates/ruff_linter/src/linter.rs:228:39
  34: ruff_linter::linter::lint_fix
             at ./ruff-main/crates/ruff_linter/src/linter.rs:614:27
  35: ruff::diagnostics::lint_path
             at ./ruff-main/crates/ruff/src/diagnostics.rs:267:18
  36: ruff::commands::check::lint_path::{{closure}}
             at ./ruff-main/crates/ruff/src/commands/check.rs:190:9
  37: std::panicking::catch_unwind::do_call
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  38: __rust_try
  39: std::panicking::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  40: std::panic::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  41: ruff_db::panic::catch_unwind
             at ./ruff-main/crates/ruff_db/src/panic.rs:122:18
  42: ruff::commands::check::lint_path
             at ./ruff-main/crates/ruff/src/commands/check.rs:189:18
  43: ruff::commands::check::check::{{closure}}
             at ./ruff-main/crates/ruff/src/commands/check.rs:94:17
  44: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  45: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:25
  46: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:16
  47: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:22
  48: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  49: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  50: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:826:18
  51: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:21
  52: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:802:9
  53: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:46:19
  54: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/fold.rs:59:19
  55: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/reduce.rs:15:8
  56: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/mod.rs:998:9
  57: ruff::commands::check::check
             at ./ruff-main/crates/ruff/src/commands/check.rs:160:10
  58: ruff::check
             at ./ruff-main/crates/ruff/src/lib.rs:421:13
  59: ruff::run
             at ./ruff-main/crates/ruff/src/lib.rs:199:33
  60: ruff::main
             at ./ruff-main/crates/ruff/src/main.rs:45:11
  61: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:253:5
  62: std::sys::backtrace::__rust_begin_short_backtrace
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:158:18
  63: std::rt::lang_start::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:206:18
  64: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/core/src/ops/function.rs:290:21
  65: std::panicking::catch_unwind::do_call
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/panicking.rs:589:40
  66: std::panicking::catch_unwind
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/panicking.rs:552:19
  67: std::panic::catch_unwind
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/panic.rs:359:14
  68: std::rt::lang_start_internal::{{closure}}
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/rt.rs:175:24
  69: std::panicking::catch_unwind::do_call
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/panicking.rs:589:40
  70: std::panicking::catch_unwind
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/panicking.rs:552:19
  71: std::panic::catch_unwind
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/panic.rs:359:14
  72: std::rt::lang_start_internal
             at /rustc/f34ba774c78ea32b7c40598b8ad23e75cdac42a6/library/std/src/rt.rs:171:5
  73: std::rt::lang_start
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:205:5
  74: <unknown>
  75: __libc_start_main
  76: _start



##### Automatic Fuzzer note, output status "Some(0)", output signal "None"
```

[compressed.zip](https://github.com/user-attachments/files/21590664/compressed.zip)

---

_Comment by @MichaReiser on 2025-08-05 07:43_

CC: @dylwil3 

---

_Label `bug` added by @MichaReiser on 2025-08-05 07:43_

---

_Label `parser` added by @MichaReiser on 2025-08-05 07:43_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-08-05 15:01_

---

_Referenced in [astral-sh/ruff#20848](../../astral-sh/ruff/pulls/20848.md) on 2025-10-13 17:54_

---

_Closed by @MichaReiser on 2025-10-15 07:50_

---
