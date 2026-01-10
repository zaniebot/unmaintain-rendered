```yaml
number: 6181
title: Formatter panic with empty attribute inner comment
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
  - help wanted
assignees: []
created_at: 2023-07-30T14:27:25Z
updated_at: 2023-08-04T09:59:57Z
url: https://github.com/astral-sh/ruff/issues/6181
synced_at: 2026-01-10T11:09:48Z
```

# Formatter panic with empty attribute inner comment

---

_Issue opened by @konstin on 2023-07-30 14:27_

The following snippet causes the formatter to panic:

```python
(#
()).a
```

There is not preceding node nor is the comment after the value range, since the comment is ahead of the value range, while the code was writing in the assumption that comments are either leading comments of the entire attribute expression or are after the value, not considering this odd parentheses case.

```
thread 'main' panicked at 'The enclosing node an attribute expression, expected to be at least after the name', crates/ruff_python_formatter/src/comments/placement.rs:934:5
stack backtrace:
   0: rust_begin_unwind
             at /rustc/90c541806f23a127002de5b4038be731ba1458ca/library/std/src/panicking.rs:578:5
   1: core::panicking::panic_fmt
             at /rustc/90c541806f23a127002de5b4038be731ba1458ca/library/core/src/panicking.rs:67:14
   2: ruff_python_formatter::comments::placement::handle_attribute_comment
             at ./crates/ruff_python_formatter/src/comments/placement.rs:934:5
   3: ruff_python_formatter::comments::placement::place_comment
             at ./crates/ruff_python_formatter/src/comments/placement.rs:53:49
   4: ruff_python_formatter::comments::visitor::CommentsVisitor::start_node_impl
             at ./crates/ruff_python_formatter/src/comments/visitor.rs:80:38
   5: ruff_python_formatter::comments::visitor::CommentsVisitor::start_node
             at ./crates/ruff_python_formatter/src/comments/visitor.rs:54:9
   6: <ruff_python_formatter::comments::visitor::CommentsVisitor as ruff_python_ast::visitor::preorder::PreorderVisitor>::visit_expr
             at ./crates/ruff_python_formatter/src/comments/visitor.rs:205:12
   7: ruff_python_ast::visitor::preorder::walk_expr
             at ./crates/ruff_python_ast/src/visitor/preorder.rs:665:13
   8: <ruff_python_formatter::comments::visitor::CommentsVisitor as ruff_python_ast::visitor::preorder::PreorderVisitor>::visit_expr
             at ./crates/ruff_python_formatter/src/comments/visitor.rs:206:13
   9: ruff_python_ast::visitor::preorder::walk_stmt
             at ./crates/ruff_python_ast/src/visitor/preorder.rs:156:15
  10: <ruff_python_formatter::comments::visitor::CommentsVisitor as ruff_python_ast::visitor::preorder::PreorderVisitor>::visit_stmt
             at ./crates/ruff_python_formatter/src/comments/visitor.rs:185:13
  11: <ruff_python_formatter::comments::visitor::CommentsVisitor as ruff_python_ast::visitor::preorder::PreorderVisitor>::visit_body
             at ./crates/ruff_python_formatter/src/comments/visitor.rs:168:23
  12: ruff_python_ast::visitor::preorder::walk_module
             at ./crates/ruff_python_ast/src/visitor/preorder.rs:118:13
  13: <ruff_python_formatter::comments::visitor::CommentsVisitor as ruff_python_ast::visitor::preorder::PreorderVisitor>::visit_mod
             at ./crates/ruff_python_formatter/src/comments/visitor.rs:158:13
  14: ruff_python_formatter::comments::visitor::CommentsVisitor::visit
             at ./crates/ruff_python_formatter/src/comments/visitor.rs:45:9
  15: ruff_python_formatter::comments::Comments::from_ast
             at ./crates/ruff_python_formatter/src/comments/mod.rs:265:13
  16: ruff_python_formatter::format_node
             at ./crates/ruff_python_formatter/src/lib.rs:148:20
  17: ruff_python_formatter::cli::format_and_debug_print
             at ./crates/ruff_python_formatter/src/cli.rs:60:21
  18: ruff_python_formatter::main
             at ./crates/ruff_python_formatter/src/main.rs:40:29
  19: core::ops::function::FnOnce::call_once
             at /rustc/90c541806f23a127002de5b4038be731ba1458ca/library/core/src/ops/function.rs:250:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
```

---

_Label `bug` added by @konstin on 2023-07-30 14:27_

---

_Label `formatter` added by @konstin on 2023-07-30 14:27_

---

_Label `help wanted` added by @MichaReiser on 2023-07-31 08:49_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-07-31 15:49_

---

_Closed by @konstin on 2023-08-04 09:59_

---
