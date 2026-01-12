```yaml
number: 15479
title: "Remove `AstNode` and `AnyNode`"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
assignees: []
merged: true
base: main
head: dcreager/simplify-ast-node
created_at: 2025-01-14T20:35:09Z
updated_at: 2025-01-17T22:11:02Z
url: https://github.com/astral-sh/ruff/pull/15479
synced_at: 2026-01-12T15:55:51Z
```

# Remove `AstNode` and `AnyNode`

---

_@dcreager_

While looking into potential AST optimizations, I noticed the `AstNode` trait and `AnyNode` type aren't used anywhere in Ruff or Red Knot.  It looks like they might be historical artifacts of previous ways of consuming AST nodes?

- `AstNode::cast`, `AstNode::cast_ref`, and `AstNode::can_cast` are not used anywhere.
- Since `cast_ref` isn't needed anymore, the `Ref` associated type isn't either.

This is a pure refactoring, with no intended behavior changes.

---

_Review requested from @MichaReiser by @MichaReiser on 2025-01-14 20:40_

---

_Comment by @github-actions[bot] on 2025-01-14 20:44_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_Label `internal` added by @AlexWaygood on 2025-01-14 21:16_

---

_Comment by @MichaReiser on 2025-01-15 07:59_

> It looks like they might be historical artifacts of previous ways of consuming AST nodes?

They're mainly experimentations on my end but I never really had time to fully persuade them. The overall design with `AstNode` is inspired by rust analyzers/BiomeJS AST [facade over a generic syntax node](https://github.com/biomejs/biome/blob/main/crates/biome_js_syntax/src/generated/nodes.rs#L16418). But I don't think the design works as well for our AST. That's why removing `cast`, `cast_ref`, and `Ref` is probably a good idea. I'm also open to removing the `AstNode`. 

I do think it might be valuable to keep `StatementRef` around:

* We have and use `ExpressionRef`, `AnyNodeRef`. `StatementRef` is needed for a "complete" API
* The main motivation for `StatementRef` (and `ExpressionRef`) is that we have methods that should accept any statement node. Historically, we used `&Stmt` for those. The problem with using `&Stmt` is that you're screwed if you only have a `&StmtIf` because there's no way to go from `&StmtIf` to `&Stmt::If`. Our previous solution was to change the call-site to get access to the `&Stmt` node. Unfortunately, that method now accepts redundant arguments or we just weakened its signature (with a very likely `unwrap` call in the method itself). A possible solution for this problem is to use `StatementRef` over `&Stmt` because you can convert a `&Stmt` and a specific statement node to `StatementRef`. I've never had the time to refactor some code to use `StatementRef` because it's hard to identify the problematic methods now where they all use `&Stmt` instead of a specific node. It's also something that needs to be done at a larger scale because mixing `StatementRef` and `&Stmt` is problematic again whenever you have to call from a function taking a `StatementRef` into a function that takes a `&Stmt`.









---

_Comment by @dcreager on 2025-01-15 15:19_

> I do think it might be valuable to keep `StatementRef` around:
> 
> * We have and use `ExpressionRef`, `AnyNodeRef`. `StatementRef` is needed for a "complete" API
> 
> * The main motivation for `StatementRef` (and `ExpressionRef`) is that we have methods that should accept any statement node. Historically, we used `&Stmt` for those. The problem with using `&Stmt` is that you're screwed if you only have a `&StmtIf` because there's no way to go from `&StmtIf` to `&Stmt::If`. Our previous solution was to change the call-site to get access to the `&Stmt` node. Unfortunately, that method now accepts redundant arguments or we just weakened its signature (with a very likely `unwrap` call in the method itself). A possible solution for this problem is to use `StatementRef` over `&Stmt` because you can convert a `&Stmt` and a specific statement node to `StatementRef`. I've never had the time to refactor some code to use `StatementRef` because it's hard to identify the problematic methods now where they all use `&Stmt` instead of a specific node. It's also something that needs to be done at a larger scale because mixing `StatementRef` and `&Stmt` is problematic again whenever you have to call from a function taking a `StatementRef` into a function that takes a `&Stmt`.

What I'm eventually aiming for (WIP branch [here](https://github.com/astral-sh/ruff/tree/dcreager/ast-builder)) would be to use `IndexVec` to store all of the AST variants, and `Stmt`/`Expr`/etc would be enums wrapping the _id_, not the actual data.  I think that would make `Expr` work in places where we're currently using `ExpressionRef`.  And a method that only operates on `StmtIf` would take in `StmtIfId` instead of the enum wrapper.

---

_Converted to draft by @dcreager on 2025-01-16 02:05_

---

_Comment by @dcreager on 2025-01-16 02:06_

Putting this back to draft while I iterate on some follow-on patches

---

_Renamed from "Remove dead code from `AstNode` trait" to "Remove `AstNode` and `AnyNode`" by @dcreager on 2025-01-17 19:38_

---

_Comment by @dcreager on 2025-01-17 19:43_

This PR diff is _much_ nicer with #15544 merged!  This is ready for review again.

I've also taken the opportunity to remove `AnyNode`.  In general, we operate on references to syntax nodes, and not owned copies of them.  It was only being used in one test, which can be updated to use `AnyNodeRef` instead.

> I do think it might be valuable to keep `StatementRef` around:

I've kept `StatementRef` (and `ExpressionRef`), though with #15544 they're now auto-generated.

---

_Marked ready for review by @dcreager on 2025-01-17 19:43_

---

_@MichaReiser approved on 2025-01-17 20:30_

It's now also much easier to review. Thank you

---

_Merged by @dcreager on 2025-01-17 22:11_

---

_Closed by @dcreager on 2025-01-17 22:11_

---

_Branch deleted on 2025-01-17 22:11_

---
