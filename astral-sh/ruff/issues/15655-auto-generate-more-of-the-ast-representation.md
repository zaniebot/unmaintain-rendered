```yaml
number: 15655
title: Auto-generate more of the AST representation
type: issue
state: open
author: dcreager
labels:
  - internal
  - help wanted
assignees: []
created_at: 2025-01-21T21:41:51Z
updated_at: 2025-02-20T19:20:03Z
url: https://github.com/astral-sh/ruff/issues/15655
synced_at: 2026-01-12T15:54:54Z
```

# Auto-generate more of the AST representation

---

_@dcreager_

In #15544 we added a [script](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/generate.py) to [auto-generate](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/src/generated.rs) large parts of the Rust data model that we use to store the AST of parsed Python code.  That script consumes a [TOML file](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/ast.toml), which describes all of the possible syntax nodes (e.g., `StmtIf`, `ExprBinOp`) and any groups those nodes belong to (e.g. `Stmt`, `Expr`).  The details of each syntax node are still [defined manually](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/src/nodes.rs) in Rust.

We could go further with auto-generation, with existing art that we could build on.  rust-analyzer uses [ungrammar](https://rust-analyzer.github.io//blog/2020/10/24/introducing-ungrammar.html), while Python itself uses [ASDL](https://www.oilshell.org/blog/2016/12/11.html) ([asdl](https://github.com/python/cpython/blob/main/Parser/Python.asdl), [parser](https://github.com/python/cpython/blob/main/Parser/asdl.py), [codegen](https://github.com/python/cpython/blob/main/Parser/asdl_c.py)).  This would eliminate even more tedious hand-written Rust code — not just the struct/enum definitions themselves, but even things like the [`visit_source_order`](https://github.com/astral-sh/ruff/blob/ef85c682bdd76ed63457bdfbec72e09424ac20ab/crates/ruff_python_ast/src/node.rs#L18-L24) methods for each syntax node.  It would also allow us to experiment more easily with other internal representations for the parsed AST — such as using `IndexVec` to store the syntax node content (as alluded to in https://github.com/astral-sh/ruff/pull/12419#issuecomment-2262727929).

---

_Label `internal` added by @dcreager on 2025-01-21 21:41_

---

_Label `help wanted` added by @dcreager on 2025-01-21 21:41_

---

_Comment by @Glyphack on 2025-02-13 20:49_

Hey, I'd like to help with this and the linked issue.
First I went through the original PR and tried resolving one of the comments to see how the code generation works(https://github.com/astral-sh/ruff/pull/16144).

I'll look into other files and see what other stuff can we auto generate with the current information.

I want to continue with the implementation, I appreciate your help with a few questions:

1. Is the goal use ASDL for generating the AST node structs and enums? If I'm not wrong by using ASDL we don't need the `ast.toml` anymore. So should we still keep `ast.toml`?
2. Is the plan to use ungrammar only for generating [visit_source_order](https://github.com/astral-sh/ruff/blob/ef85c682bdd76ed63457bdfbec72e09424ac20ab/crates/ruff_python_ast/src/node.rs#L18-L24) for nodes? Or the mention of ungrammar is for something else?

Update: I decided to extend the toml file you created. I just borrowed some names from ASDL to make it possible to generate AST nodes.

---
