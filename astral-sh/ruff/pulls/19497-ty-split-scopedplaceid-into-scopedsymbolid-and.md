```yaml
number: 19497
title: "[ty] Split `ScopedPlaceId` into `ScopedSymbolId` and `ScopedMemberId`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/place-refactor
created_at: 2025-07-22T20:28:57Z
updated_at: 2025-07-25T20:56:27Z
url: https://github.com/astral-sh/ruff/pull/19497
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Split `ScopedPlaceId` into `ScopedSymbolId` and `ScopedMemberId`

---

_@MichaReiser_

## Summary

This PR changes the place structs in the semantic index module to enums over a symbol or a member:

* `ScopedPlaceId`: Is now an enum over `ScopedSymbolId` and `ScopedMemberId`
* `PlaceExpr`: Is now an enum over `Symbol` and `Member`
* `PlaceExprRef`: Is now an enum over `&Symbol` and `&Member`
* `PlaceTable`: now separately tracks symbols and members

This idea was initially mentioned in https://github.com/astral-sh/ruff/pull/19470. The hope was that splitting `Symbol` and `Member` would help to reduce memory usage because `Symbol` is only 32 bytes (compared to `PlaceExpr`, which is 40 bytes). 

However, the memory usage is roughly unchanged after running ty on a large repository (the saving must be less than 100MB if there's even any). Splitting symbol and members also introduces some extra memory overhead because all `IndexVec` over `ScopedPlaceId` now needs to be two separate vectors. 

Performance also shows to be mostly neutral. 

The main benefit I now see in this refactor is that it clarifies some usages of places. Before, it was often unclear whether some code iterates over symbols or members or both. In fact, I spent the majority of my time during this refactor on trying to understand which of the two it is. The split also helped me better understand which flags are only supported on flags/members and which flags are supported by both.

Now, whether this distinction makes sense long term heavily depends on whether we expect that many places that currently are only over symbols will need to handle members the same way in the future. 

Overall: I don't have a strong opinion on this. I do think it helps clarify some code but it does come at a cost.

## Code increase

This PR adds a fair amount of new code. However, most of it is just the boilerplate from having separate `Symbol`/`Member`, `SymbolTable`/`MemberTable`/`PlaceTable` and `SymbolTableBuilder`/`MemberTableBuilder`/`PlaceTableBuilder`. Most of that code is fairly simple (many getters)


---

_Label `ty` added by @AlexWaygood on 2025-07-22 21:08_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/member.rs`:141 on 2025-07-23 20:46_

Would it be better to store the symbol in a separate field, like `PlaceExpr` currently does with its `root_name` field?

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/member.rs`:207 on 2025-07-23 20:47_

In that case, maybe the `Symbol` variant wouldn't be needed here?

---

_@oconnor663 reviewed on 2025-07-23 20:47_

---

_Comment by @github-actions[bot] on 2025-07-24 05:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-07-24 05:54_

Hmm. The memory increase doesn't come entirely as a surprise. I suspect that we end up with more "empty buckets" with two hash maps compared to just using one but I need to look at more detailed reports.

---

_Comment by @github-actions[bot] on 2025-07-24 09:53_

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

_@MichaReiser reviewed on 2025-07-24 09:54_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/member.rs`:1 on 2025-07-24 09:54_

I'll write some more docs if we decide to go forward with this PR. The current docs are carried over from `PlaceExpr`

---

_@MichaReiser reviewed on 2025-07-24 09:55_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/place.rs`:925 on 2025-07-24 09:55_

I moved everything *Scope* to `scope.rs`. That code doesn't need to belong into `place`.

---

_Marked ready for review by @MichaReiser on 2025-07-24 09:55_

---

_Review requested from @carljm by @MichaReiser on 2025-07-24 09:55_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-24 09:55_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-24 09:55_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-24 09:55_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-24 10:01_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/member.rs`:150 on 2025-07-24 11:44_

I think we can optimize this further by: 

* Storing a single `Name` which is the entire path of the member
* In the `SmallVec`: Store the kind of each segment and where it starts as a `u32`. 

This doesn't change the size of `Member`, but it reduces the size of the stored segments because it doesn't require a length and capacity for each segment.

This should be sufficient because all we need is a unique key to hash, knowing if it has the form `a.b`, and a `Display` implementation

---

_@MichaReiser reviewed on 2025-07-24 11:44_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/scope.rs`:239 on 2025-07-25 00:43_

nit: this got search-replaced too eagerly in the symbol->place PR, CPython doesn't have a "place table"
```suggestion
        // symbol table also uses the term "function-like" for these scopes.
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/member.rs`:80 on 2025-07-25 00:48_

This comment doesn't seem like it matches the code now?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:318 on 2025-07-25 01:01_

And similar for above comment.

```suggestion
    /// [`PlaceState`] visible at end of scope for each member.
```

---

_@carljm approved on 2025-07-25 01:05_

I think this is a strong improvement in clarity; if it's neutral in performance, I'm +1 to go ahead with it. Thank you!

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2244 on 2025-07-25 04:07_

nit: it might be useful to reduce the nesting here by chaining the calls

---

_@dhruvmanila reviewed on 2025-07-25 04:11_

---

_Label `internal` added by @dhruvmanila on 2025-07-25 04:11_

---

_@sharkdp approved on 2025-07-25 10:54_

Thank you!

A minor naming quibble: "member" refers to something like `self.x` (which seems consistent with how we use that term elsewhere, as an attribute on something), but also to a subscript expression like `xs[0]`. But no need to change anything.

---

_@MichaReiser reviewed on 2025-07-25 11:05_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2244 on 2025-07-25 11:05_

It's not clear to me how I would do this without let chains

---

_Comment by @MichaReiser on 2025-07-25 11:30_

> A minor naming quibble: "member" refers to something like self.x (which seems consistent with how we use that term elsewhere, as an attribute on something), but also to a subscript expression like xs[0]. But no need to change anything.

That's fair. Not sure what else to use. I considered `Attribute` but that is confusing because only `x.a` is an attribute access. That's why I landed on `Member` because, to me, that includes all sort of member access (but maybe that's just me coming from the JS ecosystem where all those expressions are called member expression)

---

_Comment by @MichaReiser on 2025-07-25 11:43_

A possible alternative is `Subscript`. It might be confusing too, but `x.a` is just a special way of writing the subscript. I'm happy to make that rename if we decide for that name in a separate PR

---

_Merged by @MichaReiser on 2025-07-25 11:54_

---

_Closed by @MichaReiser on 2025-07-25 11:54_

---

_Branch deleted on 2025-07-25 11:54_

---

_Comment by @carljm on 2025-07-25 15:25_

Yes, the naming issue occurred to me in review also, but I didn't mention it because I didn't have any concrete suggestion that I liked. I don't know of a term that clearly means "either attribute or subscript", and "member" (borrowed from JS) seems not unreasonable (though I would also initially assume it to mean just an attribute).

The clearest options would be the verbose ones: `AttributeOrSubscript`

---

_Comment by @dcreager on 2025-07-25 20:56_

> either attribute or subscript

subscribute :grin: 

---
