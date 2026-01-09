---
number: 7735
title: "Panic when formatting file `xpected the keyword token Def but found the token SimpleToken { kind: At, range: 46..47 } instead`"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - good first issue
  - formatter
assignees: []
created_at: 2023-10-01T06:54:03Z
updated_at: 2023-10-04T07:55:03Z
url: https://github.com/astral-sh/ruff/issues/7735
synced_at: 2026-01-07T13:12:15-06:00
---

# Panic when formatting file `xpected the keyword token Def but found the token SimpleToken { kind: At, range: 46..47 } instead`

---

_Issue opened by @qarmin on 2023-10-01 06:54_

Ruff 0.0.291 (latest changes from main branch)
```
ruff format *.py
```

file content:
```
class DownloadBar(progress.bar.PixelBar):
    @property
    def push_url(self, url, dest=None, mode=0o755, tgz=False, extract_name=None):  # yapf: disable
        path = mirror_download(url,
                               logger=self.logger)
```

error
```


warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
error: Panicked while formatting /home/rafal/test/tmp_folder/F_NAME_140959584747323567431622.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFormatter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'Expected the keyword token Def but found the token SimpleToken { kind: At, range: 46..47 } instead.', crates/ruff_python_formatter/src/statement/clause.rs:422:13
Backtrace:    0: ruff_cli::panic::catch_unwind::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:31:25
   1: <alloc::boxed::Box<F,A> as core::ops::function::Fn<Args>>::call
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/alloc/src/boxed.rs:2007:9
   2: std::panicking::rust_panic_with_hook
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:709:13
   3: std::panicking::begin_panic_handler::{{closure}}
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:597:13
   4: std::sys_common::backtrace::__rust_end_short_backtrace
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/sys_common/backtrace.rs:151:18
   5: rust_begin_unwind
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:593:5
   6: core::panicking::panic_fmt
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/core/src/panicking.rs:67:14
   7: ruff_python_formatter::statement::clause::find_keyword
   8: ruff_python_formatter::statement::clause::ClauseHeader::first_keyword_range
   9: ruff_python_formatter::statement::clause::ClauseHeader::range
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/clause.rs:43:29
  10: ruff_python_formatter::verbatim::write_suppressed_clause_header
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/verbatim.rs:921:30
  11: <ruff_python_formatter::statement::clause::FormatClauseHeader as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/clause.rs:349:13
  12: ruff_formatter::arguments::Argument<Context>::new::formatter
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:43:13
  13: ruff_formatter::arguments::Argument<Context>::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:56:9
  14: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/formatter.rs:220:22
  15: <ruff_python_formatter::statement::stmt_function_def::FormatStmtFunctionDef as ruff_python_formatter::FormatNodeRule<ruff_python_ast::nodes::StmtFunctionDef>>::fmt_fields
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/stmt_function_def.rs:33:9
  16: ruff_python_formatter::FormatNodeRule::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/lib.rs:62:13
  17: ruff_python_formatter::generated::<impl ruff_formatter::FormatRule<ruff_python_ast::nodes::StmtFunctionDef,ruff_python_formatter::context::PyFormatContext> for ruff_python_formatter::statement::stmt_function_def::FormatStmtFunctionDef>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/generated.rs:80:9
  18: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:19
  19: <ruff_python_formatter::statement::FormatStmt as ruff_formatter::FormatRule<ruff_python_ast::nodes::Stmt,ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/mod.rs:40:48
  20: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:9
  21: <ruff_python_formatter::statement::suite::SuiteChildStatement as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/suite.rs:574:73
  22: <ruff_python_formatter::statement::suite::FormatSuite as ruff_formatter::FormatRule<alloc::vec::Vec<ruff_python_ast::nodes::Stmt>,ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/suite.rs:145:13
  23: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:9
  24: ruff_formatter::arguments::Argument<Context>::new::formatter
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:43:13
  25: ruff_formatter::arguments::Argument<Context>::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:56:9
  26: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/formatter.rs:220:22
  27: ruff_formatter::buffer::Recording<B>::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/buffer.rs:551:9
  28: <ruff_formatter::builders::BlockIndent<Context> as ruff_formatter::Format<Context>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/builders.rs:1213:23
  29: ruff_formatter::arguments::Argument<Context>::new::formatter
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:43:13
  30: ruff_formatter::arguments::Argument<Context>::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:56:9
  31: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/formatter.rs:220:22
  32: <ruff_python_formatter::statement::clause::FormatClauseBody as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/clause.rs:399:13
  33: ruff_formatter::arguments::Argument<Context>::new::formatter
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:43:13
  34: ruff_formatter::arguments::Argument<Context>::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:56:9
  35: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/formatter.rs:220:22
  36: <ruff_python_formatter::statement::stmt_class_def::FormatStmtClassDef as ruff_python_formatter::FormatNodeRule<ruff_python_ast::nodes::StmtClassDef>>::fmt_fields
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/stmt_class_def.rs:35:9
  37: ruff_python_formatter::FormatNodeRule::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/lib.rs:62:13
  38: ruff_python_formatter::generated::<impl ruff_formatter::FormatRule<ruff_python_ast::nodes::StmtClassDef,ruff_python_formatter::context::PyFormatContext> for ruff_python_formatter::statement::stmt_class_def::FormatStmtClassDef>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/generated.rs:116:9
  39: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:19
  40: <ruff_python_formatter::statement::FormatStmt as ruff_formatter::FormatRule<ruff_python_ast::nodes::Stmt,ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/mod.rs:41:45
  41: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:9
  42: <ruff_python_formatter::statement::suite::SuiteChildStatement as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/suite.rs:574:73
  43: <ruff_python_formatter::statement::suite::FormatSuite as ruff_formatter::FormatRule<alloc::vec::Vec<ruff_python_ast::nodes::Stmt>,ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/suite.rs:145:13
  44: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:9
  45: ruff_formatter::arguments::Argument<Context>::new::formatter
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:43:13
  46: ruff_formatter::arguments::Argument<Context>::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:56:9
  47: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/formatter.rs:220:22
  48: <ruff_python_formatter::module::mod_module::FormatModModule as ruff_python_formatter::FormatNodeRule<ruff_python_ast::nodes::ModModule>>::fmt_fields
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/module/mod_module.rs:27:13
  49: ruff_python_formatter::FormatNodeRule::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/lib.rs:62:13
  50: ruff_python_formatter::generated::<impl ruff_formatter::FormatRule<ruff_python_ast::nodes::ModModule,ruff_python_formatter::context::PyFormatContext> for ruff_python_formatter::module::mod_module::FormatModModule>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/generated.rs:14:9
  51: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:19
  52: <ruff_python_formatter::module::FormatMod as ruff_formatter::FormatRule<ruff_python_ast::nodes::Mod,ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/module/mod.rs:15:42
  53: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:9
  54: ruff_formatter::arguments::Argument<Context>::new::formatter
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:43:13
  55: ruff_formatter::arguments::Argument<Context>::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:56:9
  56: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/formatter.rs:220:22
  57: ruff_formatter::write
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:726:5
  58: ruff_formatter::buffer::Buffer::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/buffer.rs:58:9
  59: ruff_formatter::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:775:12
  60: ruff_python_formatter::format_module_ast
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/lib.rs:150:21
  61: ruff_python_formatter::format_module_source
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/lib.rs:136:21
  62: ruff_cli::commands::format::format_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/format.rs:154:21
  63: ruff_cli::commands::format::format::{{closure}}::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/format.rs:79:29
  64: std::panicking::try::do_call
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:500:40
  65: std::panicking::try
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:464:19
  66: std::panic::catch_unwind
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panic.rs:142:14
  67: ruff_cli::panic::catch_unwind
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:40:18
  68: ruff_cli::commands::format::format::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/format.rs:78:31
  69: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:123:36
  70: rayon::iter::plumbing::Folder::consume_iter
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:179:20
  71: rayon::iter::plumbing::Producer::fold_with
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:110:9
  72: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:438:13
  73: rayon::iter::plumbing::bridge_producer_consumer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:397:12
  74: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:373:13
  75: <rayon::vec::Drain<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/vec.rs:147:13
  76: <rayon::vec::IntoIter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/vec.rs:83:9
  77: rayon::iter::plumbing::bridge
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:357:12
  78: <rayon::vec::IntoIter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/vec.rs:58:9
  79: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:46:9
  80: <rayon::iter::unzip::UnzipB<I,OP,CA> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:271:22
  81: rayon::iter::extend::<impl rayon::iter::ParallelExtend<T> for alloc::vec::Vec<T>>::par_extend
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/extend.rs:586:28
  82: <rayon::iter::unzip::UnzipA<I,OP,FromB> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:221:13
  83: rayon::iter::extend::<impl rayon::iter::ParallelExtend<T> for alloc::vec::Vec<T>>::par_extend
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/extend.rs:586:28
  84: rayon::iter::unzip::execute_into
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:54:5
  85: rayon::iter::unzip::execute
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:38:5
  86: rayon::iter::unzip::partition_map
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:164:5
  87: rayon::iter::ParallelIterator::partition_map
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/mod.rs:2183:9
  88: ruff_cli::commands::format::format
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/format.rs:91:10
  89: ruff_cli::format
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:179:9
  90: ruff_cli::run
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:164:34
  91: ruff::main
             at /home/rafal/test/ruff/crates/ruff_cli/src/bin/ruff.rs:49:11
  92: core::ops::function::FnOnce::call_once
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/core/src/ops/function.rs:250:5
  93: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/sys_common/backtrace.rs:135:18
  94: main
  95: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  96: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  97: _start


```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12776567/python_compressed.zip)



---

_Label `bug` added by @charliermarsh on 2023-10-01 15:34_

---

_Label `formatter` added by @charliermarsh on 2023-10-01 15:34_

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-10-01 15:34_

---

_Assigned to @konstin by @konstin on 2023-10-01 15:37_

---

_Unassigned @konstin by @konstin on 2023-10-01 15:40_

---

_Label `good first issue` added by @konstin on 2023-10-01 15:40_

---

_Closed by @dhruvmanila on 2023-10-04 07:55_

---
