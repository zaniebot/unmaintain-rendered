```yaml
number: 18441
title: "[ty] Fix missing registration of identifiers during semantic index construction"
type: pull_request
state: closed
author: BurntSushi
labels:
  - server
  - ty
assignees: []
base: main
head: ag/fix-completion-panic
created_at: 2025-06-03T13:48:07Z
updated_at: 2026-01-05T14:24:04Z
url: https://github.com/astral-sh/ruff/pull/18441
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Fix missing registration of identifiers during semantic index construction

---

_@BurntSushi_

Fixes astral-sh/ty#572


---

_Review requested from @carljm by @BurntSushi on 2025-06-03 13:48_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-06-03 13:48_

---

_Review requested from @sharkdp by @BurntSushi on 2025-06-03 13:48_

---

_Review requested from @dcreager by @BurntSushi on 2025-06-03 13:48_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-06-03 13:48_

---

_Comment by @BurntSushi on 2025-06-03 13:48_

(This is kind of a wild guess on my part.)

---

_Comment by @github-actions[bot] on 2025-06-03 13:51_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-06-03 13:59_

---

_Comment by @AlexWaygood on 2025-06-03 14:07_

Could you say a bit more about what the cause of the panic is? Where are you using the `scopes_by_expression` map in the autocompletion logic?

---

_Comment by @BurntSushi on 2025-06-03 14:22_

It's this:

https://github.com/astral-sh/ruff/blob/f23d2c9b9e600cef08d6ed797cba4d335fe7772b/crates/ty_python_semantic/src/semantic_model.rs#L48-L67

Where the node given to that function is identified via:

https://github.com/astral-sh/ruff/blob/f23d2c9b9e600cef08d6ed797cba4d335fe7772b/crates/ty_ide/src/completion.rs#L31-L46

Basically, we're looking for the most constraining AST node, looking up that node's scope and then listing out the identifiers in that scope (and its parent scopes).

---

_Comment by @AlexWaygood on 2025-06-03 14:36_

You also need to recurse into `match` statements -- this also causes the playground to panic for me:

```py
match object():
    case fo<CURSOR>
```

---

_Comment by @AlexWaygood on 2025-06-03 14:41_

This still causes a panic on your branch if I run the playground locally:

```py
def f(x):<CURSOR>
```

---

_@AlexWaygood reviewed on 2025-06-03 14:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1046 on 2025-06-03 14:50_

It feels a bit weird to be inserting `Identifier`s into this map, since `Identifier`s aren't really expressions. I wondered if we should be using a separate map entirely for completions logic, but that would probably be overkill. It could be worth adding a comment about why we do this, though.

I'm also slightly concerned that it's easy to forget to insert into this map if we're just manually adding the `self.record_scope_for_identifier()` calls into various branches. But I don't necessarily have a better suggestion

---

_Comment by @BurntSushi on 2025-06-03 14:54_

> This still causes a panic on your branch if I run the playground locally:
> 
> ```python
> def f(x):<CURSOR>
> ```

Interesting! I can't reproduce this locally on this branch. It doesn't seem to panic on `main` either (when I run the LSP in vim).

> ```python
> match object():
>     case fo<CURSOR>
> ```

Ah yeah I see the problem. There are some identifiers I missed in the match cases.

---

_Comment by @MichaReiser on 2025-06-03 14:56_

I've to think this through a bit more. The initial design of `ScopedExpressionId` was that they're indeed only for expressions and not for any other node. I need to look back at why we allowed some identifiers (that was probably me) and if it makes sense to allow all identifiers or if we need a different approach here.

---

_@BurntSushi reviewed on 2025-06-03 14:56_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1046 on 2025-06-03 14:56_

Yeah I don't have a good sense of what is "right" to do here. I went this direction because it seems like the surrounding code already acknowledges an `Identifier` as something that can be addressed like an expression because of the `impl From<&ast::Identifier> for ExpressionNodeKey`.

---

_Comment by @AlexWaygood on 2025-06-03 15:00_

> Interesting! I can't reproduce this locally on this branch. It doesn't seem to panic on `main` either (when I run the LSP in vim).

This reproduces it very consistently for me on your branch:


https://github.com/user-attachments/assets/11fba3ac-ee3e-44dc-bd75-179e983236f3



---

_Comment by @AlexWaygood on 2025-06-03 15:17_

> This reproduces it very consistently for me on your branch

Do parameter nodes have identifier nodes inside them? It only repros for me if it's a function with parameters

---

_Comment by @MichaReiser on 2025-06-03 15:24_

Looking at the change history, enabling `scoped_expression_id` for `Identifier` was probably an unintentional change. It was made 2 months ago to allow recording uses on Identifiers. 

Recording the scopes for more nodes does seem reasonable to me if we have use cases beyond just expressions. But we should then rename `ExpressionNodeKey`, `ScopedExpressionId`, and the relevant methods. Although it's not clear ot me what a good name would be for this. 


The alternative is that we change the `completions` code to use a different mechanism to get the scope. E.g. to walk further up the tree until it finds an `ExprName` or `ClassDef` or `FunctionDef`, similar to what's done in hover. This is more work but has the advantage that we don't need to track more data (especially because most identifiers are redundant to `ExprName`). https://github.com/astral-sh/ruff/blob/67d94d9ec8f3d369af2e2acd8942978ea5c869d1/crates/ty_ide/src/goto.rs#L205-L251

Related: https://github.com/astral-sh/ty/issues/144

---

_Comment by @BurntSushi on 2025-06-03 15:44_

> The alternative is that we change the `completions` code to use a different mechanism to get the scope. E.g. to walk further up the tree until it finds an `ExprName` or `ClassDef` or `FunctionDef`, similar to what's done in hover. This is more work but has the advantage that we don't need to track more data (especially because most identifiers are redundant to `ExprName`).

All righty. I'll put in a pin in this PR for now until I have a chance to work on improving target detection. I guess if we can side-step this issue entirely, then it probably makes sense to avoid storing identifiers.

If we end up abdicating this PR, then I can submit another one beefing up the docs on the existing methods. Particularly their panic conditions. :-) When I used this method, I falsely assumed that _any_ expression in the `File` given to build the semantic index would be a valid key.

---

_Comment by @dhruvmanila on 2025-06-03 15:54_

> Looking at the change history, enabling `scoped_expression_id` for `Identifier` was probably an unintentional change. It was made 2 months ago to allow recording uses on Identifiers.

Yes, this was done by me (in https://github.com/astral-sh/ruff/pull/17366) to allow recording a use on identifiers which is how the overload implementation is being done. The main reason is that the function name is stored as `Identifier` and that is used to find the previous definition of the same function to create the overload "link".

---

_Comment by @MichaReiser on 2025-06-03 16:15_

Sounds good to me and thanks for jumping on this. It might be good to have a "fix" for now because it seems very easy to trigger this bug in the LSP (which results in a crash). 

A short term fix could be to add a `try_scoped_expression_id` method that returns `Option`. That gives us some time to explore the next steps without regressing the completion feature.

---

_Label `server` added by @AlexWaygood on 2025-06-03 22:55_

---

_Closed by @BurntSushi on 2026-01-05 14:24_

---
