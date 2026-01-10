```yaml
number: 11624
title: "[red-knot] initial (very incomplete) flow graph"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cfg
created_at: 2024-05-31T03:21:51Z
updated_at: 2024-05-31T23:56:38Z
url: https://github.com/astral-sh/ruff/pull/11624
synced_at: 2026-01-10T21:56:00Z
```

# [red-knot] initial (very incomplete) flow graph

---

_Pull request opened by @carljm on 2024-05-31 03:21_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Introduces the skeleton of the flow graph. So far it doesn't actually handle any non-linear control flow :) But it does show how we can go from an expression that references a symbol, backward through the flow graph, to find reachable definitions of that symbol.

Adding non-linear control flow will mean adding flow nodes with multiple predecessors, which will introduce more complexity into `ReachableDefinitionsIterator.next()`. But one step at a time.

## Test Plan

Added a (very basic) test.

---

_Review requested from @MichaReiser by @carljm on 2024-05-31 03:24_

---

_Review requested from @AlexWaygood by @carljm on 2024-05-31 03:24_

---

_Comment by @github-actions[bot] on 2024-05-31 03:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @carljm on 2024-05-31 03:39_

---

_Review requested from @plredmond by @carljm on 2024-05-31 03:40_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:249 on 2024-05-31 06:55_

I would have expected that the flow graph is separate from the symbol table so that we can built it separately. Maybe that's not worth it because of the overhead of a second AST pass. But it just doesn't feel like it's part of a symbol table. 

Two other reasons why it might be worth to move it out of `SymbolTable`

* I think it could be interesting to build the flow graph at a more granular level. E.g. allow querying the top-level module graph or the module graph per function, or class. I think that could be interesting because building the CFG can be somewhat involved and we should avoid building it for code that we never end up analyzing (This assumes that basic blocks never have edges to basic blocks from other functions)
* We need to think about how we want to split `SymbolTable` long term between the data we want to store in a persistent cache and we probably don't want to cache the CFG (but it might as well be that we don't cache anything in SymbolTable, so that this isn't a concern)


---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:283 on 2024-05-31 06:57_

Can we add some documentation to what this iterator returns. What I understand is that it finds the definitions that are reachable from the current node by navigating backwards through the CFG? How does this work for definitions that aren't defined in the same function?

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:562 on 2024-05-31 06:58_

We should look into making `Definition` `Copy` if that's something we return frequently. I add it to our monthly plan.

---

_@MichaReiser approved on 2024-05-31 06:59_

---

_@AlexWaygood reviewed on 2024-05-31 09:33_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/symbols.rs`:283 on 2024-05-31 09:33_

+1 for some more docs here

---

_@MichaReiser reviewed on 2024-05-31 09:44_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:249 on 2024-05-31 09:44_

another reason for splitting the CFG out is that we may want to navigate the AST in a different order. I think our existing implementation navigates the AST backward to ensure it knows the branch targets ahead of time. 

https://github.com/augustelalande/ruff/blob/87a3e967d4e8df53989ff1d1512749ba1df23328/crates/ruff_linter/src/rules/ruff/rules/unreachable.rs#L737-L740

---

_@carljm reviewed on 2024-05-31 17:04_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:249 on 2024-05-31 17:04_

I initially thought the same, and even started building it separately initially. But after working on it for a while, I concluded that it should not be separate.

The CFG is very closely tied to Definitions. The main purpose of our CFG is for determining reachability of definitions. Definitions are also tied to a Symbol. It will be a lot more difficult to reconstruct these relationships if we build CFG in a separate pass from symbol tables; it's much simpler if we build them together. And it's also more efficient to do a single AST traversal, rather than two.

There is never a scenario where we need (local) symbols and definitions, and we don't need a CFG, or vice versa. We always need to use them together. Therefore there isn't a gain in keeping them separate.

I agree that it's awkward naming-wise; I don't think CFG is really "part of" a symbol table, but that is currently how it's structured in this PR. This is mostly just because I wanted to get feedback on the PR before embarking on more invasive renamings / restructuring (and I'm kind of trying to avoid big restructurings anyway while Salsa rework is in progress.) I think that ultimately this should be restructured so we have a different name (in one doc I called it "indexing", pyright calls it "binding") for this information-collecting AST pass, and then symbol tables and flow graph would both be part of the "index" or whatever we call it, rather than having flow graph be part of symbol tables. But we would still build them together.

I also agree that we may want to explore "indexing" at a more granular level than whole-module-at-a-time. (I'm not totally sure this will actually make sense, but it's worth discussing at least.) But if we do this, it should apply to symbol tables and definitions also, it shouldn't apply only to CFG. If we don't need the CFG of some function, we also don't need its symbol tables or definitions. If we need its symbol tables or definitions, we need the CFG (otherwise we have no idea which definitions are relevant.) So more granular indexing does not imply doing something different for CFG than for symbol definitions.

Related to the previous point, I also agree we need to think about symbol table splitting, but again I think this is orthogonal to CFG. Wherever we have definitions, we should have CFG. So CFG should always be tied together with "local symbols" -- the stuff we need for local analysis -- but separate from "public symbols" and their types -- the stuff we persistent cache.

I am quite confident after looking at pyright and the ruff PR that for our purposes there is no reason to build CFG by backwards AST traversal, so I don't think that's a consideration. (If you are building a CFG that you want to traverse in the forwards direction, it can be more convenient to build it by backwards AST traversal, because you want to track successors of each node. But we are building a CFG for backwards traversal, so we want predecessors of each node; it's more convenient to build this by forwards AST traversal. Of course if you ultimately want both successor and predecessor linkages, you do this by making another pass over the CFG once it is initially built with linkage in just one direction.)

---

_@carljm reviewed on 2024-05-31 17:10_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:283 on 2024-05-31 17:10_

Sure, I can add a docstring here.

> What I understand is that it finds the definitions that are reachable from the current node by navigating backwards through the CFG?

Yes, that's correct.

> How does this work for definitions that aren't defined in the same function?

We only consider local definitions when deciding the type of a symbol. This is necessary for efficient type inference, otherwise the entire program would have to be analyzed in order to decide the type of any module-level symbol.

Imports count as local definitions, and of course type inference will traverse the import and look at the type of the imported name to decide the type of the import "definition." So reachability analysis only needs to worry about finding the local import definition. Similarly function arguments are definitions; in this case the only type information we have is the annotation on the argument, since we don't want to have to find every call site of the function. (That would mean we have to analyze all dependent modules in order to infer types in any function.) We may in some cases infer a relationship via our local CFG between the type of an argument and the function's return type, which we would model using typevars; this can still allow some amount of cross-function inference.

Assignments to a name from outside its scope (e.g. a nested function using `nonlocal` to assign to a name in an enclosing scope, a function using `global` to assign to a module-level name, or a module doing `import mod; mod.a = ...`) should be compatible with its public type. Those assignments will be analyzed when their own scope is analyzed. Names that might be subject to such external reassignments (i.e. module-level names, or cellvars that are assigned to by a nested scope) do need special handling in type inference / narrowing to account for the possibility of external reassignment.

---

_Comment by @carljm on 2024-05-31 20:27_

Going to go ahead and merge, remaining comments are all larger issues that may need discussion and can be changed in future.

---

_Merged by @carljm on 2024-05-31 20:27_

---

_Closed by @carljm on 2024-05-31 20:27_

---

_Branch deleted on 2024-05-31 23:56_

---
