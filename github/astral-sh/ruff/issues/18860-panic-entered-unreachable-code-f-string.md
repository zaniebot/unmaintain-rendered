---
number: 18860
title: "Panic `entered unreachable code f-string: unexpected token 'TStringMiddle' at 6..7` in `ruff_python_parser/src/parser/expression.rs`"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2025-06-22T13:31:18Z
updated_at: 2025-07-20T22:04:15Z
url: https://github.com/astral-sh/ruff/issues/18860
synced_at: 2026-01-07T13:12:16-06:00
---

# Panic `entered unreachable code f-string: unexpected token 'TStringMiddle' at 6..7` in `ruff_python_parser/src/parser/expression.rs`

---

_Issue opened by @qarmin on 2025-06-22 13:31_

### Summary

File content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
c('{\t"i}')
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
error: Panicked while linting /opt/BROKEN_FILES_DIR/205_IDX_0_RAND_1805299505270516981735147_minimized_717.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_python_parser/src/parser/expression.rs:1691:25:
internal error: entered unreachable code: f-string: unexpected token `TStringMiddle` at 6..7
Backtrace:    0: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at ./ruff-main/crates/ruff_db/src/panic.rs:91:34
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/alloc/src/boxed.rs:1980:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:841:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:706:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/sys/backtrace.rs:168:18
   5: __rustc::rust_begin_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:697:5
   6: core::panicking::panic_fmt
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/core/src/panicking.rs:75:14
   7: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_interpolated_string_elements::{{closure}}
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1691:25
   8: ruff_python_parser::parser::Parser::parse_list
             at ./ruff-main/crates/ruff_python_parser/src/parser/mod.rs:507:17
   9: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_interpolated_string_elements
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1648:14
  10: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_interpolated_string
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1528:29
  11: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_strings
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:1249:26
  12: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_atom
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:613:22
  13: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_lhs_expression
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:414:24
  14: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_binary_expression_or_higher
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:249:24
  15: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_simple_expression
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:237:14
  16: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_conditional_expression_or_higher_impl
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:214:36
  17: ruff_python_parser::parser::expression::<impl ruff_python_parser::parser::Parser>::parse_expression_list
             at ./ruff-main/crates/ruff_python_parser/src/parser/expression.rs:152:32
  18: ruff_python_parser::parser::Parser::parse_single_expression
             at ./ruff-main/crates/ruff_python_parser/src/parser/mod.rs:108:32
  19: ruff_python_parser::parser::Parser::parse
             at ./ruff-main/crates/ruff_python_parser/src/parser/mod.rs:90:38
  20: ruff_python_parser::parse_expression
             at ./ruff-main/crates/ruff_python_parser/src/lib.rs:141:10
  21: ruff_linter::rules::ruff::rules::missing_fstring_syntax::should_be_fstring
             at ./ruff-main/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs:188:22
  22: ruff_linter::rules::ruff::rules::missing_fstring_syntax::missing_fstring_syntax
             at ./ruff-main/crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs:118:8
  23: ruff_linter::checkers::ast::analyze::expression::expression
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/analyze/expression.rs:1582:21
  24: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:1998:9
  25: ruff_linter::checkers::ast::Checker::visit_non_type_definition
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:2356:14
  26: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_expr
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:1840:34
  27: ruff_python_ast::visitor::walk_stmt
  28: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_stmt
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:1362:18
  29: <ruff_linter::checkers::ast::Checker as ruff_python_ast::visitor::Visitor>::visit_body
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:2112:18
  30: ruff_linter::checkers::ast::check_ast
             at ./ruff-main/crates/ruff_linter/src/checkers/ast/mod.rs:3083:13
  31: ruff_linter::linter::check_path
             at ./ruff-main/crates/ruff_linter/src/linter.rs:229:39
  32: ruff_linter::linter::lint_fix
             at ./ruff-main/crates/ruff_linter/src/linter.rs:639:27
  33: ruff::diagnostics::lint_path
             at ./ruff-main/crates/ruff/src/diagnostics.rs:268:18
  34: ruff::commands::check::lint_path::{{closure}}
             at ./ruff-main/crates/ruff/src/commands/check.rs:192:9
  35: std::panicking::catch_unwind::do_call
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  36: __rust_try
  37: std::panicking::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  38: std::panic::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  39: ruff_db::panic::catch_unwind
             at ./ruff-main/crates/ruff_db/src/panic.rs:122:18
  40: ruff::commands::check::lint_path
             at ./ruff-main/crates/ruff/src/commands/check.rs:191:18
  41: ruff::commands::check::check::{{closure}}
             at ./ruff-main/crates/ruff/src/commands/check.rs:94:17
  42: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  43: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:25
  44: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:16
  45: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:22
  46: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
  47: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
  48: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:826:18
  49: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:21
  50: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:802:9
  51: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:46:19
  52: <rayon::iter::fold::Fold<I,ID,F> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/fold.rs:59:19
  53: rayon::iter::reduce::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/reduce.rs:15:8
  54: rayon::iter::ParallelIterator::reduce
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/mod.rs:998:9
  55: ruff::commands::check::check
             at ./ruff-main/crates/ruff/src/commands/check.rs:164:10
  56: ruff::check
             at ./ruff-main/crates/ruff/src/lib.rs:420:13
  57: ruff::run
             at ./ruff-main/crates/ruff/src/lib.rs:198:33
  58: ruff::main
             at ./ruff-main/crates/ruff/src/main.rs:45:11
  59: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
  60: std::sys::backtrace::__rust_begin_short_backtrace
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152:18
  61: std::rt::lang_start::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:206:18
  62: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/core/src/ops/function.rs:284:21
  63: std::panicking::catch_unwind::do_call
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:589:40
  64: std::panicking::catch_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:552:19
  65: std::panic::catch_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panic.rs:359:14
  66: std::rt::lang_start_internal::{{closure}}
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/rt.rs:175:24
  67: std::panicking::catch_unwind::do_call
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:589:40
  68: std::panicking::catch_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panicking.rs:552:19
  69: std::panic::catch_unwind
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/panic.rs:359:14
  70: std::rt::lang_start_internal
             at /rustc/d4e1159b8c97478778b09a4cc1c7adce5653b8bf/library/std/src/rt.rs:171:5
  71: std::rt::lang_start
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:205:5
  72: <unknown>
  73: __libc_start_main
  74: _start



##### Automatic Fuzzer note, output status "Some(0)", output signal "None"
```

[compressed.zip](https://github.com/user-attachments/files/20852944/compressed.zip)

### Version

90894932634fe58bd26bb7a586a969383bc49b29

---

_Label `bug` added by @ntBre on 2025-06-22 14:41_

---

_Label `fuzzer` added by @ntBre on 2025-06-22 14:41_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-06-22 15:13_

---

_Comment by @MichaReiser on 2025-06-23 06:39_

Note, this is not necessarily a parser bug. It's more likely that the lint rule calls `parse_expression` at an invalid position.

Also funny that we think the `\t` is the start of a t-string.

---

_Comment by @dylwil3 on 2025-06-23 12:38_

We don't think `\t` is the start of a t-string in the expression above. What's happening is that `RUF027` generates a fix changing this into an f-string. In the process of determining whether or not we should generate that fix, we parse the expression

```python
f'{\t"i}'
```
Since `\t"i` is inside braces, it is being treated as an expression - which means it's actually a line-continuation character followed by an unterminated t-string. When you parse it as a module this error is being reported correctly, but as you point out we instead call `parse_expression` which is likely causing the issue.

---

_Referenced in [astral-sh/ruff#19183](../../astral-sh/ruff/pulls/19183.md) on 2025-07-07 14:54_

---

_Closed by @dylwil3 on 2025-07-20 22:04_

---

_Referenced in [astral-sh/ruff#19751](../../astral-sh/ruff/issues/19751.md) on 2025-08-05 05:50_

---
