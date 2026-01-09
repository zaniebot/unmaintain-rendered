---
number: 7734
title: "Panic when formatting file `editor_main.main_ui' contains an unsupported '\\r' line terminator character`"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-01T06:47:26Z
updated_at: 2023-10-02T13:51:08Z
url: https://github.com/astral-sh/ruff/issues/7734
synced_at: 2026-01-07T13:12:15-06:00
---

# Panic when formatting file `editor_main.main_ui' contains an unsupported '\r' line terminator character`

---

_Issue opened by @qarmin on 2023-10-01 06:47_

Ruff 0.0.291 (latest changes from main branch)
```
ruff format *.py
```

file content:
```
from integration_testing_environment.integration_testing_environment_ui\
    .editor_main.main_ui import start_editor

__all__ = [
    "start_editor"
]
```

error
```


warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.
error: Panicked while formatting /home/rafal/test/tmp_folder/F_NAME_6733953291507120079.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFormatter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at 'The content 'integration_testing_environment.integration_testing_environment_ui\
    .editor_main.main_ui' contains an unsupported '\r' line terminator character but text must only use line feeds '\n' as line separator. Use '\n' instead of '\r' and '\r\n' to insert a line break in strings.', crates/ruff_formatter/src/builders.rs:406:5
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
   7: ruff_formatter::builders::debug_assert_no_newlines
             at /home/rafal/test/ruff/crates/ruff_formatter/src/builders.rs:406:5
   8: <ruff_formatter::builders::SourceTextSliceBuilder as ruff_formatter::Format<Context>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/builders.rs:392:9
   9: <ruff_python_formatter::other::identifier::FormatIdentifier as ruff_formatter::FormatRule<ruff_python_ast::nodes::Identifier,ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/other/identifier.rs:11:9
  10: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:9
  11: <core::option::Option<T> as ruff_formatter::Format<Context>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:488:28
  12: ruff_formatter::arguments::Argument<Context>::new::formatter
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:43:13
  13: ruff_formatter::arguments::Argument<Context>::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:56:9
  14: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/formatter.rs:220:22
  15: <ruff_python_formatter::statement::stmt_import_from::FormatStmtImportFrom as ruff_python_formatter::FormatNodeRule<ruff_python_ast::nodes::StmtImportFrom>>::fmt_fields
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/stmt_import_from.rs:27:9
  16: ruff_python_formatter::FormatNodeRule::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/lib.rs:62:13
  17: ruff_python_formatter::generated::<impl ruff_formatter::FormatRule<ruff_python_ast::nodes::StmtImportFrom,ruff_python_formatter::context::PyFormatContext> for ruff_python_formatter::statement::stmt_import_from::FormatStmtImportFrom>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/generated.rs:662:9
  18: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:19
  19: <ruff_python_formatter::statement::FormatStmt as ruff_formatter::FormatRule<ruff_python_ast::nodes::Stmt,ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/statement/mod.rs:56:47
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
  27: <ruff_python_formatter::module::mod_module::FormatModModule as ruff_python_formatter::FormatNodeRule<ruff_python_ast::nodes::ModModule>>::fmt_fields
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/module/mod_module.rs:27:13
  28: ruff_python_formatter::FormatNodeRule::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/lib.rs:62:13
  29: ruff_python_formatter::generated::<impl ruff_formatter::FormatRule<ruff_python_ast::nodes::ModModule,ruff_python_formatter::context::PyFormatContext> for ruff_python_formatter::module::mod_module::FormatModModule>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/generated.rs:14:9
  30: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:19
  31: <ruff_python_formatter::module::FormatMod as ruff_formatter::FormatRule<ruff_python_ast::nodes::Mod,ruff_python_formatter::context::PyFormatContext>>::fmt
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/module/mod.rs:15:42
  32: <ruff_formatter::FormatRefWithRule<T,R,C> as ruff_formatter::Format<C>>::fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:608:9
  33: ruff_formatter::arguments::Argument<Context>::new::formatter
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:43:13
  34: ruff_formatter::arguments::Argument<Context>::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/arguments.rs:56:9
  35: <ruff_formatter::formatter::Formatter<Context> as ruff_formatter::buffer::Buffer>::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/formatter.rs:220:22
  36: ruff_formatter::write
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:726:5
  37: ruff_formatter::buffer::Buffer::write_fmt
             at /home/rafal/test/ruff/crates/ruff_formatter/src/buffer.rs:58:9
  38: ruff_formatter::format
             at /home/rafal/test/ruff/crates/ruff_formatter/src/lib.rs:775:12
  39: ruff_python_formatter::format_module_ast
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/lib.rs:150:21
  40: ruff_python_formatter::format_module_source
             at /home/rafal/test/ruff/crates/ruff_python_formatter/src/lib.rs:136:21
  41: ruff_cli::commands::format::format_path
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/format.rs:154:21
  42: ruff_cli::commands::format::format::{{closure}}::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/format.rs:79:29
  43: std::panicking::try::do_call
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:500:40
  44: std::panicking::try
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panicking.rs:464:19
  45: std::panic::catch_unwind
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/panic.rs:142:14
  46: ruff_cli::panic::catch_unwind
             at /home/rafal/test/ruff/crates/ruff_cli/src/panic.rs:40:18
  47: ruff_cli::commands::format::format::{{closure}}
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/format.rs:78:31
  48: <rayon::iter::filter_map::FilterMapFolder<C,P> as rayon::iter::plumbing::Folder<T>>::consume
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:123:36
  49: rayon::iter::plumbing::Folder::consume_iter
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:179:20
  50: rayon::iter::plumbing::Producer::fold_with
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:110:9
  51: rayon::iter::plumbing::bridge_producer_consumer::helper
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:438:13
  52: rayon::iter::plumbing::bridge_producer_consumer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:397:12
  53: <rayon::iter::plumbing::bridge::Callback<C> as rayon::iter::plumbing::ProducerCallback<I>>::callback
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:373:13
  54: <rayon::vec::Drain<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/vec.rs:147:13
  55: <rayon::vec::IntoIter<T> as rayon::iter::IndexedParallelIterator>::with_producer
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/vec.rs:83:9
  56: rayon::iter::plumbing::bridge
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/plumbing/mod.rs:357:12
  57: <rayon::vec::IntoIter<T> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/vec.rs:58:9
  58: <rayon::iter::filter_map::FilterMap<I,P> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/filter_map.rs:46:9
  59: <rayon::iter::unzip::UnzipB<I,OP,CA> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:271:22
  60: rayon::iter::extend::<impl rayon::iter::ParallelExtend<T> for alloc::vec::Vec<T>>::par_extend
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/extend.rs:586:28
  61: <rayon::iter::unzip::UnzipA<I,OP,FromB> as rayon::iter::ParallelIterator>::drive_unindexed
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:221:13
  62: rayon::iter::extend::<impl rayon::iter::ParallelExtend<T> for alloc::vec::Vec<T>>::par_extend
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/extend.rs:586:28
  63: rayon::iter::unzip::execute_into
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:54:5
  64: rayon::iter::unzip::execute
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:38:5
  65: rayon::iter::unzip::partition_map
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/unzip.rs:164:5
  66: rayon::iter::ParallelIterator::partition_map
             at /home/rafal/.cargo/registry/src/index.crates.io-6f17d22bba15001f/rayon-1.8.0/src/iter/mod.rs:2183:9
  67: ruff_cli::commands::format::format
             at /home/rafal/test/ruff/crates/ruff_cli/src/commands/format.rs:91:10
  68: ruff_cli::format
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:179:9
  69: ruff_cli::run
             at /home/rafal/test/ruff/crates/ruff_cli/src/lib.rs:164:34
  70: ruff::main
             at /home/rafal/test/ruff/crates/ruff_cli/src/bin/ruff.rs:49:11
  71: core::ops::function::FnOnce::call_once
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/core/src/ops/function.rs:250:5
  72: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/d5c2e9c342b358556da91d61ed4133f6f50fc0c3/library/std/src/sys_common/backtrace.rs:135:18
  73: main
  74: __libc_start_call_main
             at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
  75: __libc_start_main_impl
             at ./csu/../csu/libc-start.c:360:3
  76: _start


```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12776558/python_compressed.zip)



---

_Renamed from "Panic when formatting file" to "Panic when formatting file `editor_main.main_ui' contains an unsupported '\r' line terminator character`" by @qarmin on 2023-10-01 06:47_

---

_Comment by @charliermarsh on 2023-10-01 15:36_

I need to look into why `\r` is invalid there. It might be intentionally unsupported by Micha.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-01 18:04_

---

_Label `bug` added by @charliermarsh on 2023-10-01 18:12_

---

_Label `formatter` added by @charliermarsh on 2023-10-01 18:12_

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-10-01 18:12_

---

_Comment by @charliermarsh on 2023-10-01 18:12_

This is a bug.

---

_Referenced in [astral-sh/ruff#7744](../../astral-sh/ruff/pulls/7744.md) on 2023-10-01 18:22_

---

_Closed by @charliermarsh on 2023-10-02 13:51_

---
