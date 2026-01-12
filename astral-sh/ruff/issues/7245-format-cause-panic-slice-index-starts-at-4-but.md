```yaml
number: 7245
title: "Format cause panic 'slice index starts at 4 but ends at 3'"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-09-08T16:25:01Z
updated_at: 2023-09-12T07:49:53Z
url: https://github.com/astral-sh/ruff/issues/7245
synced_at: 2026-01-12T15:54:46Z
```

# Format cause panic 'slice index starts at 4 but ends at 3'

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)
```
ruff format *.py
```

file content(at least simple cpython script shows that this is valid python file):
```
class EC2REPATH:
        f.write ("Pathway name" + "\t" "Database Identifier" + "\t" "Source database" + "\n")
```

error
```

warning: `ruff format` is a work-in-progress, subject to change at any time, and intended only for experimentation.

error: `ruff` crashed. This indicates a bug in `ruff`. If you could open an issue at:

https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at 'slice index starts at 4 but ends at 3', crates/ruff_python_formatter/src/expression/binary_like.rs:472:27
stack backtrace:
   0: rust_begin_unwind
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/std/src/panicking.rs:593:5
   1: core::panicking::panic_fmt
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/panicking.rs:67:14
   2: core::slice::index::slice_index_order_fail_rt
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/slice/index.rs:98:5
   3: core::slice::index::slice_index_order_fail
             at /rustc/5680fa18feaa87f3ff04063800aec256c3d4b4be/library/core/src/slice/index.rs:91:14
   4: <ruff_python_formatter::expression::binary_like::BinaryLike as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
   5: <ruff_python_formatter::expression::FormatExpr as ruff_formatter::FormatRule<ruff_python_ast::nodes::Expr,ruff_python_formatter::context::PyFormatContext>>::fmt::{{closure}}
   6: <ruff_python_formatter::expression::FormatExpr as ruff_formatter::FormatRule<ruff_python_ast::nodes::Expr,ruff_python_formatter::context::PyFormatContext>>::fmt
   7: ruff_formatter::arguments::Argument<Context>::new::formatter
   8: ruff_formatter::arguments::Argument<Context>::new::formatter
   9: ruff_formatter::arguments::Argument<Context>::new::formatter
  10: ruff_formatter::arguments::Argument<Context>::new::formatter
  11: ruff_formatter::arguments::Argument<Context>::new::formatter
  12: <ruff_python_formatter::expression::parentheses::FormatParenthesized as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
  13: ruff_python_formatter::FormatNodeRule::fmt
  14: ruff_python_formatter::FormatNodeRule::fmt
  15: <ruff_python_formatter::expression::FormatExpr as ruff_formatter::FormatRule<ruff_python_ast::nodes::Expr,ruff_python_formatter::context::PyFormatContext>>::fmt
  16: <ruff_python_formatter::statement::FormatStmt as ruff_formatter::FormatRule<ruff_python_ast::nodes::Stmt,ruff_python_formatter::context::PyFormatContext>>::fmt
  17: <ruff_python_formatter::statement::suite::FormatSuite as ruff_formatter::FormatRule<alloc::vec::Vec<ruff_python_ast::nodes::Stmt>,ruff_python_formatter::context::PyFormatContext>>::fmt
  18: ruff_formatter::arguments::Argument<Context>::new::formatter
  19: ruff_formatter::arguments::Argument<Context>::new::formatter
  20: <ruff_python_formatter::statement::clause::FormatClauseBody as ruff_formatter::Format<ruff_python_formatter::context::PyFormatContext>>::fmt
  21: <ruff_python_formatter::statement::stmt_class_def::FormatStmtClassDef as ruff_python_formatter::FormatNodeRule<ruff_python_ast::nodes::StmtClassDef>>::fmt_fields
  22: <ruff_python_formatter::statement::FormatStmt as ruff_formatter::FormatRule<ruff_python_ast::nodes::Stmt,ruff_python_formatter::context::PyFormatContext>>::fmt
  23: <ruff_python_formatter::statement::suite::FormatSuite as ruff_formatter::FormatRule<alloc::vec::Vec<ruff_python_ast::nodes::Stmt>,ruff_python_formatter::context::PyFormatContext>>::fmt
  24: ruff_python_formatter::format_module
  25: rayon::iter::plumbing::bridge_producer_consumer::helper
  26: ruff_cli::run
  27: ruff::main
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

[python_compressed.zip](https://github.com/astral-sh/ruff/files/12561912/python_compressed.zip)



---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 17:22_

---

_Label `bug` added by @MichaReiser on 2023-09-08 17:22_

---

_Label `formatter` added by @MichaReiser on 2023-09-08 17:22_

---

_Label `help wanted` added by @MichaReiser on 2023-09-08 17:22_

---

_Comment by @charliermarsh on 2023-09-12 01:54_

I looked into this a bit but haven't solved it yet. The `BinaryLike` format doesn't account for cases in which we have consecutive runs of implicit concatenations -- a minimal repo is `"a" "b" + "c" "d"`.

---

_Comment by @MichaReiser on 2023-09-12 05:12_

I can take a look at it

---

_Assigned to @MichaReiser by @MichaReiser on 2023-09-12 05:12_

---

_Label `help wanted` removed by @MichaReiser on 2023-09-12 05:48_

---

_Closed by @MichaReiser on 2023-09-12 07:49_

---
