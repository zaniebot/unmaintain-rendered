```yaml
number: 19703
title: "[ty] Track enclosing definitions and nested references in the semantic index"
type: pull_request
state: closed
author: oconnor663
labels:
  - ty
assignees: []
draft: true
base: main
head: jack/semantic-index-nonlocals
created_at: 2025-08-02T01:15:41Z
updated_at: 2025-08-08T04:00:20Z
url: https://github.com/astral-sh/ruff/pull/19703
synced_at: 2026-01-12T15:56:45Z
```

# [ty] Track enclosing definitions and nested references in the semantic index

---

_@oconnor663_

Add three new maps to the `SemanticIndex`:
- `definitions_in_enclosing_scopes`
- `references_in_nested_scopes`
- `bindings_in_nested_scopes`

These will serve several purposes:
- LSP is going to need these for features like "find all references" and "rename".
- We currently re-implement scope walking in several places, including `add_binding`, `infer_place_load`, and `ide_support.rs`. I don't know whether the performance cost of that matters (maybe not, since scopes aren't usually super deeply nested), but there are a lot of corner cases that it would be nice to unify, like skipping class bodies, `nonlocal` on top of `global`, etc.
- We don't currently consider bindings from sibling/cousin scopes when inferring types, and it would be nice to consider them.

To populate these maps, `SemanticIndexBuilder` tracks a set of free variables for each scope. (There's an interesting reason this has to be per-scope, see the new comment in `struct ScopeInfo`.) When popping scopes, it checks to see whether the popped scope resolves any free variables from nested scopes, and whether it creates any new ones. This makes us agnostic to whether the definition or the use comes first, since either way we'll have encountered both by the time we pop the defining scope.

The first/main commit in this PR defines the new maps and populates them. There are a couple of small follow-on commits that make use of the new data:
- deleting `infer_nonlocal()`, which is now fully redundant with checks the `SemanticIndexBuilder` is already doing
- removing the scope walk from `add_binding`

Larger changes are still TODO. I could add more to this PR, but I need some help with these bits to understand how things work today and how best to change things:
- `infer_place_load`. This one is kinda doing two separate scope walks at the same time. One is [calling `place()` and unioning `nonlocal` types as it goes](https://github.com/astral-sh/ruff/blob/85bd961fd3a98208e170e3265797816aad7c7961/crates/ty_python_semantic/src/types/infer.rs#L6743-L6763), which will benefit from using the new maps (particularly to include `nonlocal` bindings from other nested scoeps). The other is [looking at `enclosing_snapshots`](https://github.com/astral-sh/ruff/blob/85bd961fd3a98208e170e3265797816aad7c7961/crates/ty_python_semantic/src/types/infer.rs#L6653-L6709), which is a completely separate mechanism. (There are also a couple of bugs in how the snapshot mechanism handles scopes: https://github.com/astral-sh/ty/issues/916 and https://github.com/astral-sh/ty/issues/927.)
- `ide_support.rs`. Some of this might be straightforward for all I know, but I haven't touched this file at all yet.

cc @mtshiba

---

_Label `ty` added by @oconnor663 on 2025-08-02 01:15_

---

_Review requested from @carljm by @oconnor663 on 2025-08-02 01:15_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-08-02 01:15_

---

_Review requested from @sharkdp by @oconnor663 on 2025-08-02 01:15_

---

_Review requested from @dcreager by @oconnor663 on 2025-08-02 01:15_

---

_Review requested from @MichaReiser by @oconnor663 on 2025-08-02 01:15_

---

_Review requested from @dhruvmanila by @oconnor663 on 2025-08-02 01:15_

---

_Comment by @github-actions[bot] on 2025-08-02 01:17_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-02 01:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~52MB
+     memo fields = ~54MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-08-02 01:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:146 on 2025-08-02 08:22_

Would it be possible to store this information directly on `Symbol`, given that these maps are indexed by `ScopedSymbolId`. Or are we using a hash map because only few symbols in a scope need this information?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:144 on 2025-08-02 08:24_

Given that this is a subset. Could we store an extra flag in `references_in_nested_scopes` that tells them apart. This reduces the number of Vecs and hash maps 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:141 on 2025-08-02 08:44_

You Could use a `SmallVec` here. This would allow us to store 4 scoped symbol ids inline. 

---

_@MichaReiser reviewed on 2025-08-02 08:44_

Thank you for working on this

What would help me to review this PR is to extend the PR summary with more details on what the new data is that we collect and why it is essential that we do this during semantic indexing.

I'm asking because we build the semantic index for every file in completeness, even for third-party files (eagerly) and we keep it in memory forever. The semantic index is also by far the largest query result and we've made various efforts to reduce its size (and have more planned). This raises the questions if it would be better if this computation is a separate lazy query (should be sufficient for rename, or it doesn't even have to be a query), recomputing, in fact, is fine (the performance profiles on this PR suggest a small regression). The part that's the least clear to me is that we want to consider those bindings for type infernece. I don't understand the use case enough to judge if there isn't an alternative or if upfront collection is indeed the only way to go. 

The good news is: This PR doesn't regress memory usage that much. I ran it on a large repository and it only increases the semantic index size by 216 MB or ~3% (but that's still more than what an optimization like https://github.com/astral-sh/ruff/pull/19572 gives us back). So maybe that's fine and maybe there's a way we can get this down if we can avoid some of the hash maps or inner vecs


I haven't been involved in this work before. So I'm sorry if this is something that you discussed at length with @carljm and @AlexWaygood. If that's the case, feel free to disregard this.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:132 on 2025-08-03 16:09_

Wouldn't it be enough to just store `FileScopeId` as the value here, rather than `(FileScopeId, ScopedSymbolId)`?
Because, for instance, the recorded values are used [here](https://github.com/astral-sh/ruff/blob/b036317b8e51b9d777ecb2f60410df19ee825897/crates/ty_python_semantic/src/types/infer.rs#L1805-L1812), but we can get the `PlaceTable` of the enclosing scope from `enclosing_scope_id` (using `self.index.place_table()`), and then get the `ScopedSymbolId` with `PlaceTable::symbol_id`.

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:141 on 2025-08-03 16:15_

For the same reason as above, wouldn't it be enough to just store `FileScopeId` as the value for the following maps?

---

_Review comment by @mtshiba on `crates/ty_python_semantic/src/semantic_index/builder.rs`:99 on 2025-08-03 16:30_

For the same reason as described below (`symbol.rs`), it seems that this `ScopedSymbolId` is redundant (it can be obtained from the `PlaceTable` of the nested scope).

---

_@mtshiba reviewed on 2025-08-03 16:32_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/symbol.rs`:144 on 2025-08-04 19:36_

My thinking was that most variables aren't bound in nested scopes, but many are used, and I wanted it to be quick to ask "do we actually need to look at any of these references?" during typechecking. But now that you have me thinking more about storage overhead, maybe the best thing would be to just allocate another bit in `SymbolFlags` called something like `IS_BOUND_IN_NESTED_SCOPE`?

---

_@oconnor663 reviewed on 2025-08-04 19:36_

---

_Comment by @oconnor663 on 2025-08-04 20:01_

> I haven't been involved in this work before. So I'm sorry if this is something that you discussed at length with @carljm and @AlexWaygood. If that's the case, feel free to disregard this.

@carljm and I discussed it, but I can't say it was _at length_ :) My impression was that he didn't want "find all references" / "rename all" to require walking all scopes. But now that you mention it, my understanding of the problem isn't very good: I don't know whether that's because we expect it to perform poorly, or whether we don't want the LSP to be doing complicated things, or what? Carl could you clarify? (I wonder if part of the problem with doing a big walk is just that it would get invalidated frequently?)

> The part that's the least clear to me is that we want to consider those bindings for type infernece.

In my head it would be nice to handle cases like this:

```py
x = None
def f():
    global x
    x = 42
reveal_type(x);  # revealed: `None` (should be `None | Literal[42]`?)
```

I know I've seen cases like that come up in ecosystem analysis results for `nonlocal` changes, but I don't know how common any of it is. @AlexWaygood I think you've expressed some skepticism about whether we'd want do analyze these? (Related TODOs in mdtests: [this one](https://github.com/astral-sh/ruff/blob/739c94f95a4871d8f31e42817c3ce13b5c561ddd/crates/ty_python_semantic/resources/mdtest/scopes/global.md#L241) and [these ones](https://github.com/astral-sh/ruff/blob/739c94f95a4871d8f31e42817c3ce13b5c561ddd/crates/ty_python_semantic/resources/mdtest/scopes/nonlocal.md#L129).)

---

_Comment by @carljm on 2025-08-05 04:35_

In our initial discussions around this PR, I was mostly thinking about the `bindings_in_nested_scopes` piece of it, which seems the most beneficial to me.

On the "benefit" side, it was primarily motivated by a) goto-definition including nested definitions (and sibling definitions, if you goto-definition on a nested reference!) without having to re-walk all nested scopes, which @UnboundVariable mentioned as a significant TODO in the goto-definition PR, and b) considering these nested definitions in type inference, which I had originally thought maybe we wouldn't do, but is definitely nicer / more accurate if we can do it, for scenarios like the one shown by @oconnor663 (and similar ones with `nonlocal`), and if we track this information for goto-definition anyway, then it is not too hard to do.

On the "cost" side, my assumption had been that use of `nonlocal` and `global` is rare enough that this wouldn't be a significant extra memory cost, if we pay some attention to optimizing our storage. (And this also makes it a good tradeoff to do up-front when we are already walking the AST anyway, since it is cheap to store relatively rare information, but would be expensive to always re-walk all nested/sibling scopes later on, even though by doing so we are unlikely to discover any nested `global` or `nonlocal` statements referencing a given symbol.)

I'm less clear on the cost-benefit analysis for `references_in_nested_scopes` and `definitions_in_enclosing_scopes`. Both of these will be much more commonly populated (nested references are more common than `nonlocal` and `global` bindings), therefore have a higher storage cost.

The latter can avoid the need to re-walk scope ancestors respecting Python scoping rules (which, as the PR description notes, we do, with some code redundancy, in a few different places). This could be a nice code simplification, but I'm not sure if it is a good tradeoff performance-wise or not. Walking just ancestor scopes is cheaper than walking all sibling/nested scopes.

The former seems mostly useful for rename-all and find-all-references. But these are features where I think users will accept a bit slower response time, and features that are less frequently used (relative to e.g. core type inference), so it's less clear to me that it's worth paying a cost in every semantic indexing just in order to speed up these features.

If it's not too much work (I haven't reviewed the PR in detail yet), I think it would be ideal to split these, and initially land a diff adding only `bindings_in_nested_scopes`, evaluate its memory cost, and then also make use of it to fix the TODOs in goto-definition, and consider nested bindings in type inference. And then separately evaluate the tradeoffs of the other two maps.

---

_Converted to draft by @oconnor663 on 2025-08-06 21:58_

---

_Comment by @oconnor663 on 2025-08-08 04:00_

Closing in favor of https://github.com/astral-sh/ruff/pull/19820.

---

_Closed by @oconnor663 on 2025-08-08 04:00_

---
