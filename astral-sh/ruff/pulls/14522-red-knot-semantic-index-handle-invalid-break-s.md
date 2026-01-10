```yaml
number: 14522
title: "[red-knot] Semantic index: handle invalid `break`s"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-14342
created_at: 2024-11-22T09:58:05Z
updated_at: 2024-11-22T12:13:57Z
url: https://github.com/astral-sh/ruff/pull/14522
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Semantic index: handle invalid `break`s

---

_Pull request opened by @sharkdp on 2024-11-22 09:58_

## Summary

This fix intends to address panics related to invalid syntax like the following where a `break` statement is used in a nested definition inside a loop:

```py
while True:

    def b():
        x: int

        break
```

> [!NOTE]  
> I don't like the fact that we add even more mutable state to `SemanticIndexBuilder`. There might very well be a better solution here. And maybe we need something more general to handle other cases of invalid syntax. I still thought it would be valuable to propose this change with relevant regression tests, so we can start thinking about what would be required to properly fix this.

closes #14342

## Test Plan

* New corpus regression tests.
* New unit test to make sure we handle nested while loops correctly. This test is passing on `main`, but can easily fail if the `is_inside_loop` state isn't properly saved/restored.

---

_Label `red-knot` added by @sharkdp on 2024-11-22 09:58_

---

_Review requested from @carljm by @sharkdp on 2024-11-22 09:58_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-22 09:58_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-22 09:58_

---

_Comment by @github-actions[bot] on 2024-11-22 10:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-11-22 10:06_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:536 on 2024-11-22 10:06_

We also need to unset the flag when entering a lambda expressoin

---

_@AlexWaygood reviewed on 2024-11-22 10:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:536 on 2024-11-22 10:06_

would it make more sense to reset this in `push_scope()` and `pop_scope()`, rather than having very similar code in the branches for `Stmt::FunctionDef` and `Stmt::ClassDef`?

---

_@MichaReiser reviewed on 2024-11-22 10:06_

---

_@AlexWaygood reviewed on 2024-11-22 10:06_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:874 on 2024-11-22 10:06_

We need to set the flag to `true` for while statements

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:536 on 2024-11-22 10:06_

Doing this would fix @MichaReiser's comment in https://github.com/astral-sh/ruff/pull/14522#discussion_r1853665736

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:874 on 2024-11-22 10:14_

I don't understand? We already do?

---

_@sharkdp reviewed on 2024-11-22 10:14_

---

_@sharkdp reviewed on 2024-11-22 10:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:536 on 2024-11-22 10:22_

Not saying your suggestion is wrong, just trying to understand: isn't the body of a lambda an expression? or can it contain a (`break`) statement somehow? Or is there another reason why we need to handle it? As in: what test do I need to write to make sure I handle things correctly?

---

_@MichaReiser reviewed on 2024-11-22 10:50_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:536 on 2024-11-22 10:50_

You're right. Technically it's not possible that the lambda can contain a break. My motivation for adding it to lambda is mainly a precaution in case we use `inside_loop` to detect anything else where this restriction doesn't apply. 

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:536 on 2024-11-22 10:54_

I think this would involve handling a whole stack of `inside_loop` flags? While doing it here naturally uses the call stack for the same outcome.  But happy to try that if we think that's a better solution.

---

_@sharkdp reviewed on 2024-11-22 10:54_

---

_@AlexWaygood reviewed on 2024-11-22 11:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:536 on 2024-11-22 11:07_

Hmm, I see your point.

I think the design of the builder here would be nicer to deal with if the pushing and popping of the scope happened in the same method -- something more similar to the `with_type_params` method and the `with_generators_scope` methods that we already have. We'd have to keep track of fewer stacks on the builder that deal with issues of nested scopes, and it would make it easier to maintain the invariant that a `push_scope` call should always be balanced with a `pop_scope` call.

For now I think I'm okay with what you have. The implicit invariant we're relying on here is that class definitions and functions defined via `def` are the only kinds of nested scopes that can contain statements (and therefore `break` statements). This will break if Python adds another kind of scope in a future version that allows statements in its body, but this does seem unlikely.

---

_@MichaReiser reviewed on 2024-11-22 11:08_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:536 on 2024-11-22 11:08_

I'm fine with either solution. Using a stack seems complicated for just the one flag.

---

_@MichaReiser approved on 2024-11-22 11:08_

---

_@sharkdp reviewed on 2024-11-22 11:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:536 on 2024-11-22 11:51_

I went with the stack solution after all :smile:. I thought about amending the existing stack for scopes, but for now it's a separate stack that mirrors `scope_stack`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:55 on 2024-11-22 11:53_

non-blocking comment: it might be slightly fewer allocations (and better reflect the invariant that the two stacks always have the same length) if we had something like this, rather than two parallel stacks:

```rs
enum LoopState {
    InLoop,
    NotInLoop,
}

pub(super) struct SemanticIndexBuilder<'db> {
    scope_stack: Vec<(FileScopeId, LoopState)>
}
```

But I'm also fine with what you have now!

---

_@AlexWaygood approved on 2024-11-22 11:53_

---

_@AlexWaygood reviewed on 2024-11-22 11:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:55 on 2024-11-22 11:56_

oops, I typed out this comment before I saw https://github.com/astral-sh/ruff/pull/14522#discussion_r1853798868 😆

---

_@sharkdp reviewed on 2024-11-22 11:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:55 on 2024-11-22 11:56_

~~This is what I meant above by "I thought about amending the existing stack for scopes" :smile:.~~

Edit: same problem here

I'll check now what implications this has.


---

_@sharkdp reviewed on 2024-11-22 12:05_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:55 on 2024-11-22 12:05_

It's better. Thanks for pushing me.

---

_Merged by @sharkdp on 2024-11-22 12:13_

---

_Closed by @sharkdp on 2024-11-22 12:13_

---

_Branch deleted on 2024-11-22 12:13_

---
