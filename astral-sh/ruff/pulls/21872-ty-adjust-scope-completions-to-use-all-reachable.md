```yaml
number: 21872
title: "[ty] Adjust scope completions to use all reachable symbols"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/fix-end-of-scope-symbols
created_at: 2025-12-09T19:47:55Z
updated_at: 2025-12-11T13:26:18Z
url: https://github.com/astral-sh/ruff/pull/21872
synced_at: 2026-01-12T15:57:35Z
```

# [ty] Adjust scope completions to use all reachable symbols

---

_@BurntSushi_

This PR adds a new `UseDefMap::all_reachable_symbols_with_definitions`
API for use by the LSP for offerring scoped completions. In particular,
this API uses all reachable symbols instead of only those that are
reachable at the end of the scope. Notably, this fixes a problem where
completions for local variables in functions with a `return` statement
are no longer offered.

(I opened this as a draft because I'm uncertain of the approach, and
because this needs more tests.)

Note that the first commit is just a rename. So reviewers might want to
look at this PR commit-by-commit.

Fixes https://github.com/astral-sh/ty/issues/1294

---

_Review requested from @carljm by @BurntSushi on 2025-12-09 19:47_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-09 19:47_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-09 19:47_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-09 19:47_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-09 19:47_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-09 19:48_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-12-09 19:48_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-09 19:48_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-12-09 19:48_

---

_Converted to draft by @BurntSushi on 2025-12-09 19:48_

---

_Label `server` added by @BurntSushi on 2025-12-09 19:49_

---

_Label `ty` added by @BurntSushi on 2025-12-09 19:49_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 19:50_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 19:52_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 42 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:599 on 2025-12-09 21:20_

nit: the second element here is a `&ReachableDefinitions`, which stores multiple bindings and declarations -- so it seems confusing to name the variable `reachable_definition` (singular)
```suggestion
            .flat_map(|(symbol_id, reachable_definitions)| {
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:608 on 2025-12-09 21:26_

A lot of stuff here (privileging the first definition, ignoring conflicting declarations, etc) seem like very use-case-specific policy decisions, which feels a bit out-of-place for the use-def map. Also in terms of layering, using `place_from_declarations` and `place_from_bindings` here is a bit off -- those are type-inference-level functions, should be layered above the use-def map, which is just responsible for definitions.

So while I think adding this API is good, I feel like it's doing too much. I was originally thinking (when we were chatting in Discord) that it could just return an iterator of all bindings and declarations for each symbol, but I see that that is complicated because they are already separated in `ReachableDefinitions`. It now looks to me like a return type of `(symbol_id, bindings_iterator, declarations_iterator)` might be best? That feels like it fulfills the responsibility of the use-def map -- the rest is up to the caller to decide what to do with those iterators of definitions. (It does I guess mean that chaining in some form will still be needed on the caller side.)

---

_@carljm reviewed on 2025-12-09 21:27_

---

_Comment by @BurntSushi on 2025-12-10 17:03_

OK, I've taken another stab at this, incorporating your feedback. Thank you. :-) Some notes though:

First is that I've left the existing `all_members_of_scope` (renamed to `all_end_of_scope_members`) in place, since it looks like places other than completions use it.

Second is that I don't think I've accounted for [this](https://github.com/astral-sh/ty/issues/1294#issuecomment-3628426664):

> Switching from "end-of-scope" to "all-reachable" for module-global scopes would probably not be desirable

Can you say more about how I should tackle this? And in particular, why is module-global scope special here? I've added a test case matching your example, but I also added a test case in function scope and it is similarly wonky. The end result is that we end up with imprecise types. Which is maybe what you were referring to on Discord and probably something we should just live with for now?

My last thought is that I'm wondering if there is an inconsistency in the terminology used for the use-def docs and its routines. In particular, reading this:

```rust
//! Let's take this code sample:
//!
//! ```python
//! x = 1
//! x = 2
//! y = x
//! if flag:
//!     x = 3
//! else:
//!     x = 4
//! z = x
//! ```
//!
//! In this snippet, we have four bindings of `x` (the statements assigning `1`, `2`, `3`, and `4`
//! to it), and two uses of `x` (the `y = x` and `z = x` assignments). The first binding of `x`
//! does not reach any use, because it's immediately replaced by the second binding, before any use
//! happens. (A linter could thus flag the statement `x = 1` as likely superfluous.)
```

That is, this verbiage is using the word "reach" here to, I believe, imply that the first binding of `x` is _not reachable_ at its first use in `y = x`. Similarly, I would say that:

```
zqzqzq: int = 1
zqzqzq: str = 'foo'
zqzq<CURSOR>
```

The first binding of `zqzqzq` is _not reachable_ on the line containing the cursor. However, the use-def map's `reachable_definitions_by_symbol` map, notably including "reachable," seems to specifically contain both declarations of `zqzqzq` in the above example.

So I guess my question is: is my understanding wrong or is the terminology not quite consistent?

---

_Comment by @carljm on 2025-12-11 01:37_

> Switching from "end-of-scope" to "all-reachable" for module-global scopes would probably not be desirable

I think this comment was poorly worded. For the current module I think it's reasonable to use all-reachable in module global scope, just like in any other enclosing scope. What I really meant to say was we shouldn't resolve imports like that -- an import should assume that the module it is importing from executed fully. But I think the code you are touching here is only for walking enclosing scopes in the current module, and is unrelated to imports, so I suspect you don't need to do anything regarding my poorly-phrased comment.

Regarding the wording of "reach", we do use it in two somewhat different ways. When we talk about "all reachable" definitions, we mean any definition that can be reached from start-of-scope (that is, any definition that does not live in unreachable code). This is a useful concept for references to names in enclosing scopes, because fundamentally a (lazily executed, like a function) nested scope usually doesn't know from precisely what point(s) in the enclosing scope it might be called. So we have to respect the possibility that any _reachable_ definition in the enclosing scope _might_ be live at the (unknown) point we were called from. (Of course with more sophisticated analysis, more precision can be possible -- but Python is so dynamic, and so many things can execute arbitrary code, that it's very hard to do this analysis fully robustly.)

But we also do talk about a definition "reaching" a specific use, meaning that definition remains possibly live at that use. Perhaps here we should ideally choose a different word (like for example, "live").

---

_Comment by @carljm on 2025-12-11 01:41_

I'll note that I think using all-reachable definitions in the enclosing scope is actually correct (or "best we can reasonably do"), and matches what we do in type inference, for lazy enclosed scopes (like functions), because in that case we fundamentally don't know which point in the outer scope's control flow we should be considering.

For eager enclosed scopes (scopes that are executed immediately at the point of definition, like class bodies or comprehensions) we do better in type inference, because in that case we do know precisely where in the enclosing scope the nested scope executed. (If you wanted, I think you could use those same "eager nested scope snapshots" from the semantic index for more precision here as well, but I think that could be a separate follow-up change.)

And of course in the immediate local scope you are less precise than we'd ideally like here, because you don't have a "use" recorded for the position of the cursor.

---

_Comment by @carljm on 2025-12-11 01:43_

The new `all_reachable_symbols` API and the renaming of existing API (and leaving `all_end_of_scope_members`) all look great to me!

---

_Marked ready for review by @BurntSushi on 2025-12-11 01:59_

---

_Comment by @BurntSushi on 2025-12-11 02:05_

Sweet, okay. I think that all mostly makes sense to me. I've created a [follow-up issue for eager nested scopes](https://github.com/astral-sh/ty/issues/1849). Thank you for explaining all of that. I've pulled this out of draft. :-)

---

_@MichaReiser approved on 2025-12-11 08:57_

---

_Merged by @BurntSushi on 2025-12-11 13:26_

---

_Closed by @BurntSushi on 2025-12-11 13:26_

---

_Branch deleted on 2025-12-11 13:26_

---
