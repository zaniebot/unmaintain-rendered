---
number: 6087
title: Add required changes for handling the new Jupyter AST nodes
type: issue
state: closed
author: dhruvmanila
labels:
  - core
assignees: []
created_at: 2023-07-26T07:44:02Z
updated_at: 2023-07-26T09:24:01Z
url: https://github.com/astral-sh/ruff/issues/6087
synced_at: 2026-01-07T13:12:15-06:00
---

# Add required changes for handling the new Jupyter AST nodes

---

_Issue opened by @dhruvmanila on 2023-07-26 07:44_

- [`Visitor`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/visitor.rs#L19) and [`PreorderVisitor`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/visitor/preorder.rs#L9) support
- [`ComparableExpr`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/comparable.rs#L656) and [`ComparableStmt`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/comparable.rs#L1159) support
- [`relocate_expr`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/relocate.rs#L10) support
- [`any_over_expr`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/helpers.rs#L126) and [`any_over_stmt`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/helpers.rs#L349) support
- [`AnyNode`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/node.rs#L109) and [`AnyNodeRef`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/node.rs#L3737) support
- Code [`Generator`](https://github.com/astral-sh/ruff/blob/62f821daaa5d3ec56d0ba5910b4982bb7fa2be35/crates/ruff_python_ast/src/source_code/generator.rs#L64) support

---

_Referenced in [astral-sh/ruff#5188](../../astral-sh/ruff/issues/5188.md) on 2023-07-26 07:44_

---

_Renamed from "Add required changes for handling the new  and  nodes" to "Add required changes for handling the new Jupyter AST nodes" by @dhruvmanila on 2023-07-26 07:45_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-07-26 07:45_

---

_Referenced in [astral-sh/ruff#6086](../../astral-sh/ruff/pulls/6086.md) on 2023-07-26 07:45_

---

_Closed by @dhruvmanila on 2023-07-26 08:20_

---

_Label `core` added by @dhruvmanila on 2023-07-26 09:24_

---
