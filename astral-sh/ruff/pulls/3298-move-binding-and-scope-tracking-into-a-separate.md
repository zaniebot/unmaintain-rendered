```yaml
number: 3298
title: "Move binding and scope tracking into a separate `ast::Context` struct"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/split-ast
created_at: 2023-03-02T04:26:28Z
updated_at: 2023-03-04T19:01:22Z
url: https://github.com/astral-sh/ruff/pull/3298
synced_at: 2026-01-12T04:39:44Z
```

# Move binding and scope tracking into a separate `ast::Context` struct

---

_Pull request opened by @charliermarsh on 2023-03-02 04:26_

This PR is an attempt to split up `Checker` in such a way so as to allow us to break the Ruff linter itself into multiple crates.

The first step (seen here) is to remove all AST-visitation state into a new `Context` struct, which tracks the current scope stack, available bindings, etc. By moving these fields and methods off of `Checker`, rules can instead take `Context` as an argument, rather than `Checker`, which means they can be decoupled from the actual "driver" / "visitor" and thereby the rest of the lint rules.


---

_Comment by @charliermarsh on 2023-03-02 04:40_

`Diagnostic` is another tough cycle to break. Depends on `DiagnosticKind` which depends on all rule violations. Need to play around with this and reconsider the struct hierarchy.

---

_Comment by @charliermarsh on 2023-03-02 04:41_

`Settings` is another. Perhaps we can change rules to rely on the settings they care about, rather than the `Settings` struct, and rely on `Checker` to inject those settings at the call-site.

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ctx.rs`:1 on 2023-03-02 08:23_

Nit: Should the file be named `context`? Avoiding abbreviations can improve readability for people less familiar with the code base. 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ctx.rs`:16 on 2023-03-02 08:23_

Nit: `Vec<String>`. An empty `vec![]` is conceptually the same as `None` (but requires less space)

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast.rs`:112 on 2023-03-02 08:26_

Could we move the `context` out of the `Checker` entirely and pass it around instead of the checker so that:

* The checker orchestrates the "checking"
* `context` stores the state and is passed to the rules. 



---

_@MichaReiser reviewed on 2023-03-02 08:28_

---

_@charliermarsh reviewed on 2023-03-02 16:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ctx.rs`:16 on 2023-03-02 16:49_

(I'll change this on `main`, since this is just a move.)

---

_@charliermarsh reviewed on 2023-03-02 16:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:112 on 2023-03-02 16:49_

In that setup, would `ctx` be passed around as part of the `Visitor` trait?

---

_@MichaReiser reviewed on 2023-03-02 16:58_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast.rs`:112 on 2023-03-02 16:58_

I should have spent some more time reading the code. Storing the `context` on the `Visitor` is probably fine. What I'm mainly wondering is if it would be possible to pass the `context` to various rules instead of the `Checker`. 

---

_@charliermarsh reviewed on 2023-03-02 17:01_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast.rs`:112 on 2023-03-02 17:01_

👍 That's my hope, that we can stop passing `Checker`, and instead pass: `AstContext`, a mutable `diagnostics` vector, and some kind of settings representation.

(Beyond that, we'd also need `Locator`, `Stylist,` etc., which currently live on `Checker` but there's no reason they need to.)

---

_Marked ready for review by @charliermarsh on 2023-03-04 00:42_

---

_Renamed from "Split up `Checker` into independent structs" to "Move binding and scope tracking into a separate `ast::Context` struct" by @charliermarsh on 2023-03-04 00:42_

---

_Comment by @charliermarsh on 2023-03-04 00:43_

Okay, this can be merged as-is (in that, it doesn't depend on any additional refactors to be useful).

---

_Comment by @JonathanPlasse on 2023-03-04 07:57_

How much did the compilation time improved?
Or is it preparation for future improvements?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/deferred.rs`:15 on 2023-03-04 15:28_

Nit: Can you add some documentation explaining what a `Deferred` is? I'm unable to guess its usage from just its name. 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:154 on 2023-03-04 15:30_

Nit: Should these macros be changed to accept the context as an argument instead to decouple them from `Checker`? 

---

_@MichaReiser approved on 2023-03-04 15:33_

LGTM: Something that's unclear to me but isn't something that we need to solve now is how do I decide whether to add some logic to `Checker` or `Context`? It is my impression that there are still many methods on `Checker` that only use state stored on the `Context`. Should these be moved to `Context` at some point? 

---

_Comment by @charliermarsh on 2023-03-04 17:30_

> It is my impression that there are still many methods on Checker that only use state stored on the Context. Should these be moved to Context at some point?

The intent is that these should all be moved over to Context. Lemme see if I missed any...

---

_@charliermarsh reviewed on 2023-03-04 17:30_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:154 on 2023-03-04 17:30_

They still require `self.visit_expr`, we could change them to accept both though.

---

_Comment by @charliermarsh on 2023-03-04 17:31_

> How much did the compilation time improved? Or is it preparation for future improvements?

Yeah just in preparation for future improvements. I don't expect this alone to do much, it's more that it sets us up to be able to split the crate later.

---

_Merged by @charliermarsh on 2023-03-04 19:01_

---

_Closed by @charliermarsh on 2023-03-04 19:01_

---

_Branch deleted on 2023-03-04 19:01_

---
