```yaml
number: 11134
title: Red Knot - Add symbol flags
type: pull_request
state: merged
author: plredmond
labels: []
assignees: []
merged: true
base: main
head: red-knot_symbol-flags
created_at: 2024-04-24T19:02:03Z
updated_at: 2024-04-30T00:07:30Z
url: https://github.com/astral-sh/ruff/pull/11134
synced_at: 2026-01-10T22:37:01Z
```

# Red Knot - Add symbol flags

---

_Pull request opened by @plredmond on 2024-04-24 19:02_

* Adds `Symbol.flag` bitfield. Populates it from (the three renamed) `add_or_update_symbol*` methods.
* Currently there are these flags supported:
  * `IS_DEFINED` is set in a scope where a variable is defined.
  * `IS_USED` is set in a scope where a variable is referenced. (To have both this and `IS_DEFINED` would require two separate appearances of a variable in the same scope-- one def and one use.)
  * `MARKED_GLOBAL` and `MARKED_NONLOCAL` are **not yet implemented**. (*TODO: While traversing, if you find these declarations, add these flags to the variable.*)
* Adds `Symbol.kind` field (commented) and the data structure which will populate it: `Kind` which is an enum of freevar, cellvar, implicit_global, and implicit_local. **Not yet populated**. (*TODO: a second pass over the scope (or the ast?) will observe the `MARKED_GLOBAL` and `MARKED_NONLOCAL` flags to populate this field. When that's added, we'll uncomment the field.*)
* Adds a few tests that the `IS_DEFINED` and `IS_USED` fields are correctly set and/or merged:
  * Unit test that subsequent calls to `add_or_update_symbol` will merge the flag arguments.
  * Unit test that in the statement `x = foo`, the variable `foo` is considered used but not defined.
  * Unit test that in the statement `from bar import foo`, the variable `foo` is considered defined but not used.

---

_Review requested from @carljm by @plredmond on 2024-04-24 19:04_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:105 on 2024-04-24 19:58_

I think it will need to be a field, because it depends (in some cases) not only on our own flags, but on what symbols exist with the same name (and with what flags) in ancestor and enclosed scopes. We will want to do this analysis in one pass and store it for all symbols, we don't want to have to redo it every time we ask about a symbol.

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:84 on 2024-04-24 20:00_

naming nit, but I'd probably go for `Kind` here. Both `Kind` and `Disposition` seem equally generic (as in, `Disposition` doesn't communicate useful additional context, at least to me), and `Kind` is shorter :)

But if you have arguments for preferring `Disposition`, I'm all ears.

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:269 on 2024-04-24 20:02_

I would say go ahead and rename this to `add_or_update_symbol` (I think we can just drop `_to_scope` without losing anything valuable.) I think this PR is the right place to do that rename, since adding the flags really adds "update" as a possibility for the first time.

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:398 on 2024-04-24 20:05_

```suggestion
    fn add_or_update_symbol(&mut self, identifier: &str, flags: SymbolFlags) -> SymbolId {
```

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:403 on 2024-04-24 20:05_

```suggestion
    fn add_or_update_symbol_with_def(&mut self, identifier: &str, definition: Definition) -> SymbolId {
```

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:774 on 2024-04-24 20:07_

Would be good to also add/modify tests above here to actually verify flags are set correctly

---

_@carljm reviewed on 2024-04-24 20:07_

---

_@plredmond reviewed on 2024-04-25 16:45_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:269 on 2024-04-25 16:45_

I'll do this, but probably not until the rest of the work is done and the review is looking like :+1:. Renaming it now might make rebases annoying.

---

_Comment by @github-actions[bot] on 2024-04-25 17:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@plredmond reviewed on 2024-04-29 22:12_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:105 on 2024-04-29 22:12_

Uncommenting this before merge with a docstring that says it's not yet implemented.

---

_@plredmond reviewed on 2024-04-29 22:14_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:269 on 2024-04-29 22:14_

Doing this now ahead of rereview/merge.

---

_@plredmond reviewed on 2024-04-29 22:14_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:398 on 2024-04-29 22:14_

Doing this now ahead of rereview/merge.

---

_@plredmond reviewed on 2024-04-29 22:14_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:403 on 2024-04-29 22:14_

Doing this now ahead of rereview/merge.

---

_@carljm reviewed on 2024-04-29 22:28_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:105 on 2024-04-29 22:28_

Also fine to just leave it out of this PR entirely and include it in the next one.

---

_@plredmond reviewed on 2024-04-29 22:46_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:774 on 2024-04-29 22:46_

I added a few assertions to existing tests. If that's not alright, lmk and I'll make distinct tests.

---

_Marked ready for review by @plredmond on 2024-04-29 22:51_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:492 on 2024-04-29 23:01_

Oh, this isn't something we ever discussed, but type params are defined by the annotation scope that introduces them. And we should test for this case as well. (There are already tests for generic func and generic class.)
```suggestion
                self.add_or_update_symbol(name, SymbolFlags::IS_DEFINED);
```

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:512 on 2024-04-29 23:04_

Now that you're already matching on ctx above, this line might be simpler as a flags intersection check?

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:99 on 2024-04-29 23:09_

let's add a TODO here to note that we aren't setting these flags yet

---

_@carljm requested changes on 2024-04-29 23:11_

Looks good! Couple comments.

---

_@plredmond reviewed on 2024-04-29 23:17_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:492 on 2024-04-29 23:17_

Great; was wondering about that! I'll add a test.

---

_@plredmond reviewed on 2024-04-29 23:18_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:512 on 2024-04-29 23:18_

Yes, I was wondering about this redundancy. Willfix.

---

_@plredmond reviewed on 2024-04-29 23:26_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:512 on 2024-04-29 23:26_

Added Copy+Clone on SymbolFlags:u8

---

_@carljm approved on 2024-04-29 23:42_

Awesome!

Couple comments on the PR description:

1. It says three flags are supported, but it seems like just two?
2. This is more a forward-looking comment for the TODOs, but I realized from some conversation this morning that we will probably want to track both `CellVar` and `CellVarAssigned` as separate kinds, where the former is "defined in current scope, used in at least one nested scope" and the latter is "defined in current scope, defined and marked_nonlocal in at least one nested scope", because the latter will have different implications for type checking.

---

_Comment by @plredmond on 2024-04-30 00:06_

> It says three flags are supported, but it seems like just two?

That was a typo of "these". Sorry.

I've added a commit for the other variant of Kind.

---

_Merged by @plredmond on 2024-04-30 00:07_

---

_Closed by @plredmond on 2024-04-30 00:07_

---

_Branch deleted on 2024-04-30 00:07_

---
