---
number: 19226
title: "Panic `assertion failed: start.raw <= end.raw` in `ruff_python_formatter/src/expression/binary_like.rs`"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - formatter
  - help wanted
  - fuzzer
assignees: []
created_at: 2025-07-09T07:07:35Z
updated_at: 2025-11-18T15:48:16Z
url: https://github.com/astral-sh/ruff/issues/19226
synced_at: 2026-01-10T01:23:00Z
---

# Panic `assertion failed: start.raw <= end.raw` in `ruff_python_formatter/src/expression/binary_like.rs`

---

_Issue opened by @qarmin on 2025-07-09 07:07_

### Summary

File content(at the bottom should be attached raw, not formatted file - github removes some non-printable characters, so copying from here may not work):
```
if''and(not#
0):t
```

command
```
timeout -v 300 ruff format TEST___FILE.py
```

cause this
```
warning: Detected debug build without --no-cache.
error: Panicked while formatting /opt/BROKEN_FILES_DIR/278_IDX_0_RAND_1418773717408230476762071_minimized_237.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFormatter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at crates/ruff_python_formatter/src/expression/binary_like.rs:843:37:
assertion failed: start.raw <= end.raw
Backtrace:    0: ruff_db::panic::install_hook::{{closure}}::{{closure}}
             at ./ruff-main/crates/ruff_db/src/panic.rs:91:34
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/alloc/src/boxed.rs:1985:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/panicking.rs:841:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/panicking.rs:699:13
   4: std::sys::backtrace::__rust_end_short_backtrace
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/sys/backtrace.rs:168:18
   5: __rustc::rust_begin_unwind
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/panicking.rs:697:5
   6: core::panicking::panic_fmt
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/core/src/panicking.rs:75:14
   7: core::panicking::panic
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/core/src/panicking.rs:145:5
   8: ruff_text_size::range::TextRange::new
             at ./ruff-main/crates/ruff_text_size/src/range.rs:50:9
   9: ruff_python_formatter::expression::binary_like::Operand::has_unparenthesized_leading_comments::{{closure}}
             at ./ruff-main/crates/ruff_python_formatter/src/expression/binary_like.rs:843:37
  10: <core::slice::iter::Iter<T> as core::iter::traits::iterator::Iterator>::any
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/slice/iter/macros.rs:308:24
  11: ruff_python_formatter::expression::binary_like::Operand::has_unparenthesized_leading_comments
             at ./ruff-main/crates/ruff_python_formatter/src/expression/binary_like.rs:838:36
  12: <ruff_python_formatter::expression::binary_like::FlatBinaryExpressionSlice as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/expression/binary_like.rs:719:46
  13: <&T as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:602:9
  14: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  15: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  16: <ruff_formatter::arguments::Arguments<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:81:19
  17: <ruff_python_formatter::expression::parentheses::FormatInParenthesesOnlyGroup as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/expression/parentheses.rs:333:40
  18: <ruff_python_formatter::expression::binary_like::BinaryLike as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/expression/binary_like.rs:289:62
  19: <ruff_python_formatter::expression::expr_bool_op::FormatExprBoolOp as ruff_python_formatter::FormatNodeRule<ruff_python_ast::generated::ExprBoolOp>>::fmt_fields
             at ./ruff-main/crates/ruff_python_formatter/src/expression/expr_bool_op.rs:15:32
  20: ruff_python_formatter::FormatNodeRule::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/lib.rs:78:18
  21: ruff_python_formatter::generated::<impl ruff_formatter::FormatRule<ruff_python_ast::generated::ExprBoolOp,ruff_python_formatter::context::PyFormatContext> for ruff_python_formatter::expression::expr_bool_op::FormatExprBoolOp>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/generated.rs:938:9
  22: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:746:19
  23: <ruff_python_formatter::expression::FormatExpr as ruff_formatter::FormatRule<ruff_python_ast::generated::Expr,ruff_python_formatter::context::PyFormatContext>>::fmt::{{closure}}
             at ./ruff-main/crates/ruff_python_formatter/src/expression/mod.rs:80:49
  24: <ruff_formatter::builders::FormatWith<Context,T> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/builders.rs:2259:9
  25: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  26: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  27: <ruff_python_formatter::expression::FormatExpr as ruff_formatter::FormatRule<ruff_python_ast::generated::Expr,ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/expression/mod.rs:146:13
  28: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:746:19
  29: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  30: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  31: <ruff_formatter::arguments::Arguments<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:81:19
  32: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  33: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  34: <ruff_formatter::arguments::Arguments<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:81:19
  35: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  36: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  37: <ruff_formatter::arguments::Arguments<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:81:19
  38: <ruff_formatter::builders::IndentIfGroupBreaks<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/builders.rs:2144:40
  39: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  40: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  41: <ruff_formatter::arguments::Arguments<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:81:19
  42: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  43: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  44: <ruff_formatter::arguments::Arguments<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:81:19
  45: <ruff_formatter::builders::Group<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/builders.rs:1438:40
  46: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  47: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  48: <ruff_python_formatter::expression::parentheses::FormatOptionalParentheses as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/expression/parentheses.rs:247:9
  49: <ruff_python_formatter::expression::MaybeParenthesizeExpression as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/expression/mod.rs:413:64
  50: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  51: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  52: <ruff_formatter::arguments::Arguments<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:81:19
  53: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  54: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  55: <ruff_formatter::arguments::Arguments<Context> as ruff_formatter::Format<Context>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:81:19
  56: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  57: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  58: <ruff_python_formatter::statement::clause::FormatClauseHeader as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/statement/clause.rs:378:13
  59: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  60: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  61: <ruff_python_formatter::statement::stmt_if::FormatStmtIf as ruff_python_formatter::FormatNodeRule<ruff_python_ast::generated::StmtIf>>::fmt_fields
             at ./ruff-main/crates/ruff_python_formatter/src/statement/stmt_if.rs:27:9
  62: ruff_python_formatter::FormatNodeRule::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/lib.rs:78:18
  63: ruff_python_formatter::generated::<impl ruff_formatter::FormatRule<ruff_python_ast::generated::StmtIf,ruff_python_formatter::context::PyFormatContext> for ruff_python_formatter::statement::stmt_if::FormatStmtIf>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/generated.rs:430:9
  64: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:746:19
  65: <ruff_python_formatter::statement::FormatStmt as ruff_formatter::FormatRule<ruff_python_ast::generated::Stmt,ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/statement/mod.rs:51:39
  66: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:746:19
  67: <ruff_python_formatter::statement::suite::SuiteChildStatement as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/statement/suite.rs:914:73
  68: <ruff_python_formatter::statement::suite::FormatSuite as ruff_formatter::FormatRule<alloc::vec::Vec<ruff_python_ast::generated::Stmt>,ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/statement/suite.rs:172:19
  69: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:746:19
  70: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  71: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  72: <ruff_python_formatter::module::mod_module::FormatModModule as ruff_python_formatter::FormatNodeRule<ruff_python_ast::generated::ModModule>>::fmt_fields
             at ./ruff-main/crates/ruff_python_formatter/src/module/mod_module.rs:30:13
  73: ruff_python_formatter::FormatNodeRule::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/lib.rs:78:18
  74: ruff_python_formatter::generated::<impl ruff_formatter::FormatRule<ruff_python_ast::generated::ModModule,ruff_python_formatter::context::PyFormatContext> for ruff_python_formatter::module::mod_module::FormatModModule>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/generated.rs:14:9
  75: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:746:19
  76: <ruff_python_formatter::module::FormatMod as ruff_formatter::FormatRule<ruff_python_ast::generated::Mod,ruff_python_formatter::context::PyFormatContext>>::fmt
             at ./ruff-main/crates/ruff_python_formatter/src/module/mod.rs:15:42
  77: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:746:19
  78: ruff_formatter::arguments::Argument<Context>::format
             at ./ruff-main/crates/ruff_formatter/src/arguments.rs:29:20
  79: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/formatter.rs:220:22
  80: ruff_formatter::write
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:860:7
  81: ruff_formatter::buffer::Buffer::write_fmt
             at ./ruff-main/crates/ruff_formatter/src/buffer.rs:59:9
  82: ruff_formatter::format
             at ./ruff-main/crates/ruff_formatter/src/lib.rs:909:12
  83: ruff_python_formatter::format_node
             at ./ruff-main/crates/ruff_python_formatter/src/lib.rs:154:21
  84: ruff_python_formatter::format_module_ast
             at ./ruff-main/crates/ruff_python_formatter/src/lib.rs:138:5
  85: ruff_python_formatter::format_module_source
             at ./ruff-main/crates/ruff_python_formatter/src/lib.rs:128:21
  86: ruff::commands::format::format_source
             at ./ruff-main/crates/ruff/src/commands/format.rs:366:17
  87: ruff::commands::format::format_path
             at ./ruff-main/crates/ruff/src/commands/format.rs:276:31
  88: ruff::commands::format::format::{{closure}}::{{closure}}
             at ./ruff-main/crates/ruff/src/commands/format.rs:148:29
  89: std::panicking::catch_unwind::do_call
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:589:40
  90: __rust_try
  91: std::panicking::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panicking.rs:552:19
  92: std::panic::catch_unwind
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/panic.rs:359:14
  93: ruff_db::panic::catch_unwind
             at ./ruff-main/crates/ruff_db/src/panic.rs:122:18
  94: ruff::commands::format::format::{{closure}}
             at ./ruff-main/crates/ruff/src/commands/format.rs:147:31
  95: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:123:36
  96: rayon::iter::plumbing::Folder::consume_iter
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:178:25
  97: rayon::iter::plumbing::Producer::fold_with
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:109:16
  98: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:437:22
  99: rayon::iter::plumbing::bridge_producer_consumer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:396:12
 100: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:372:13
 101: <rayon::slice::Iter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:826:18
 102: rayon::iter::plumbing::bridge
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/plumbing/mod.rs:356:21
 103: <rayon::slice::Iter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/slice/mod.rs:802:9
 104: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/filter_map.rs:46:19
 105: <rayon::iter::unzip::UnzipB<I,OP,CA> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/unzip.rs:271:32
 106: rayon::iter::extend::<impl rayon::iter::ParallelExtend<T> for alloc::vec::Vec<T>>::par_extend
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/extend.rs:588:37
 107: <rayon::iter::unzip::UnzipA<I,OP,FromB> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/unzip.rs:221:20
 108: rayon::iter::extend::<impl rayon::iter::ParallelExtend<T> for alloc::vec::Vec<T>>::par_extend
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/extend.rs:588:37
 109: rayon::iter::unzip::execute_into
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/unzip.rs:54:7
 110: rayon::iter::unzip::execute
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/unzip.rs:38:5
 111: rayon::iter::unzip::partition_map
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/unzip.rs:164:5
 112: rayon::iter::ParallelIterator::partition_map
             at /home/runner/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rayon-1.10.0/src/iter/mod.rs:2196:9
 113: ruff::commands::format::format
             at ./ruff-main/crates/ruff/src/commands/format.rs:171:10
 114: ruff::format
             at ./ruff-main/crates/ruff/src/lib.rs:211:9
 115: ruff::run
             at ./ruff-main/crates/ruff/src/lib.rs:199:34
 116: ruff::main
             at ./ruff-main/crates/ruff/src/main.rs:45:11
 117: core::ops::function::FnOnce::call_once
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/core/src/ops/function.rs:250:5
 118: std::sys::backtrace::__rust_begin_short_backtrace
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/sys/backtrace.rs:152:18
 119: std::rt::lang_start::{{closure}}
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:206:18
 120: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/core/src/ops/function.rs:284:21
 121: std::panicking::catch_unwind::do_call
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/panicking.rs:589:40
 122: std::panicking::catch_unwind
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/panicking.rs:552:19
 123: std::panic::catch_unwind
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/panic.rs:359:14
 124: std::rt::lang_start_internal::{{closure}}
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/rt.rs:175:24
 125: std::panicking::catch_unwind::do_call
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/panicking.rs:589:40
 126: std::panicking::catch_unwind
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/panicking.rs:552:19
 127: std::panic::catch_unwind
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/panic.rs:359:14
 128: std::rt::lang_start_internal
             at /rustc/a2d45f73c70d9dec57140c9412f83586eda895f8/library/std/src/rt.rs:171:5
 129: std::rt::lang_start
             at /home/runner/.rustup/toolchains/nightly-x86_64-unknown-linux-gnu/lib/rustlib/src/rust/library/std/src/rt.rs:205:5
 130: <unknown>
 131: __libc_start_main
 132: _start



##### Automatic Fuzzer note, output status "Some(2)", output signal "None"
```

[compressed.zip](https://github.com/user-attachments/files/21136944/compressed.zip)

### Version

_No response_

---

_Label `bug` added by @MichaReiser on 2025-07-09 07:10_

---

_Label `formatter` added by @MichaReiser on 2025-07-09 07:10_

---

_Label `help wanted` added by @MichaReiser on 2025-07-09 07:10_

---

_Label `fuzzer` added by @MichaReiser on 2025-07-09 07:10_

---

_Comment by @MichaReiser on 2025-07-09 07:13_

A slightly less cryptic version of this:

```py
if '' and (not #
0):
    pass
```

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-19 13:37_

---

_Comment by @TaKO8Ki on 2025-09-21 15:44_

I will take this one.

---

_Referenced in [astral-sh/ruff#20494](../../astral-sh/ruff/pulls/20494.md) on 2025-09-21 17:08_

---

_Assigned to @TaKO8Ki by @ntBre on 2025-09-22 13:39_

---

_Referenced in [astral-sh/ruff#21410](../../astral-sh/ruff/pulls/21410.md) on 2025-11-12 20:37_

---

_Referenced in [astral-sh/ruff#21501](../../astral-sh/ruff/pulls/21501.md) on 2025-11-17 16:15_

---

_Closed by @ntBre on 2025-11-18 15:48_

---
