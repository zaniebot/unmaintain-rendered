```yaml
number: 11161
title: "[red-knot] Resolve and check dependencies"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: red-knot-dependencies
created_at: 2024-04-26T08:58:32Z
updated_at: 2024-04-27T16:15:41Z
url: https://github.com/astral-sh/ruff/pull/11161
synced_at: 2026-01-10T22:37:01Z
```

# [red-knot] Resolve and check dependencies

---

_Pull request opened by @MichaReiser on 2024-04-26 08:58_

## Summary

Discover a modules dependencies and schedule them for checking. 

This does not yet support native dependencies of which we don't have access to the source.

## Test Plan

I created a `test.py` and ran `red_knot test.py`. The test.py imports `match.py`. 

I can see from the logs that `match.py` is correctly scheduled for checking. Cyclic dependencies work too. 
Red knot won't emit any diagnostics for `match.py` because it isn't an open file. 


---

_Review requested from @carljm by @MichaReiser on 2024-04-26 08:58_

---

_@MichaReiser reviewed on 2024-04-26 09:01_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:242 on 2024-04-26 09:01_

I think that's actually fine. There's no risk for deadlocks because the function only ever holds on to a single lock (and not multiple ones). Any mutations happen through `resolve_module` which must be idempotent. The only overhead here is that we might end up calling `resolve_module` unnecessary but that's fine. 

we could even consider not making this method a query. Searching the root paths is cheap. I think the only place where we safe some performance is that we avoid the `canoncialize` call. So that's why I think it's still worth keeping the cache for now.

---

_Comment by @github-actions[bot] on 2024-04-26 09:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:56 on 2024-04-26 23:54_

Unfortunately this isn't quite correct :/ The resolution of a relative import can't be determined solely from a module name, it requires knowing whether the module is an `__init__.py` file or not.

Both `foo/bar.py` and `foo/bar/__init__.py` have module name `foo.bar`. But if you have `from . import baz` in `foo/bar.py`, that resolves to `foo.baz`. If you have `from . import baz` in `foo/bar/__init__.py`, that resolves to `foo.bar.baz`.

So this operation isn't possible to implement correctly on `ModuleName` without adding to `ModuleName` a boolean flag for whether it is a package or not.

Or we could use `ModuleName` less, since we are already indexing most things by `FileId` anyway, and just have it be a name, and implement all the interesting operations on `Module` instead, which knows its path.

---

_Review comment by @carljm on `crates/red_knot/src/program/check.rs`:53 on 2024-04-27 00:04_

Will we need dependencies of a module in cases where we don't also need its symbol table anyway?

---

_Review comment by @carljm on `crates/red_knot/src/program/check.rs`:58 on 2024-04-27 00:07_

I think this TODO is a little confusing, because we'll never support "native dependencies." What we'll support in the future is stub files, and stub files in typeshed. Which are used to provide type information both for native and non-native dependencies in the standard library. (And also for some third-party code, and sometimes first-party code too.)

Also this seems like an odd place for the TODO, because nothing here will need to change to support that.

```suggestion
                    // TODO The resolver doesn't support stub files or typeshed yet.
```

---

_Review comment by @carljm on `crates/red_knot/src/program/check.rs`:63 on 2024-04-27 00:09_

We may want to distinguish between `schedule_check_file` and `schedule_index_file`. For non-first-party dependencies, there's no point in scheduling a check of them (since we won't check them) but we may still want to eagerly schedule indexing them (that is, parsing and creating symbol tables).

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:504 on 2024-04-27 00:24_

Our AST is confusing/redundant, because it has level as an `Option`, but it is never `None`. An import like `from foo import bar` (no dots) is represented in our AST with `level` as `Some(0)`, not `None`.

So if we leave the AST as-is, this code needs to be fixed to also push a `Module` dependency if `level` is `Some(0)`. I think we should fix the AST, though -- it should either not use `Option` at all, or it should set `None` instead of `Some(0)`. (I think the latter is clearer; removing the `Option` would save 4 bytes, but I don't think that matters since `StmtImportFrom` is not close to being the largest statement.)

---

_@carljm approved on 2024-04-27 00:28_

Some comments in here (the `__init__.py` thing for relative imports, and the `level = Some(0)` thing) that should definitely at least get TODO comments, if not fixed before merge.

---

_@MichaReiser reviewed on 2024-04-27 08:56_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/check.rs`:53 on 2024-04-27 08:56_

Maybe not if we can short-circuit the entire `check_file` operation. I'll look into this next week

---

_Label `internal` added by @MichaReiser on 2024-04-27 08:56_

---

_@MichaReiser reviewed on 2024-04-27 08:56_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:504 on 2024-04-27 08:56_

Thanks for resolving this for me :)

---

_@MichaReiser reviewed on 2024-04-27 09:27_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/check.rs`:63 on 2024-04-27 09:27_

Yeah, that might be good. The scheduling is still very much unclear to me. So I'll defer figuring out the right solution for now.

---

_@MichaReiser reviewed on 2024-04-27 09:28_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:56 on 2024-04-27 09:28_

I think I still like to have `ModuleName` used where we know it is a module name. It makes it clear what kind of name it is. We might also want to do this for other names. 

I think moving this to module makes sense (I don't think it's something we need to cache. 

---

_@carljm reviewed on 2024-04-27 12:03_

---

_Review comment by @carljm on `crates/red_knot/src/program/check.rs`:63 on 2024-04-27 12:03_

Maybe a TODO here then?

---

_@MichaReiser reviewed on 2024-04-27 15:40_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/check.rs`:63 on 2024-04-27 15:40_

I rephrased the native dependencies todo

---

_Merged by @MichaReiser on 2024-04-27 15:49_

---

_Closed by @MichaReiser on 2024-04-27 15:49_

---

_Branch deleted on 2024-04-27 15:49_

---

_@carljm reviewed on 2024-04-27 15:59_

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:66 on 2024-04-27 15:59_

I think this is still off-by-one, because ranges are half-open?

If we have a module (of kind module) `foo.bar`, and we an import with level 1, that should resolve to `foo` (strip one level). But this code would resolve it to `foo.bar` (strip nothing), because `0..1` is empty.

This kind of code is where it's really useful to add a few isolated unit tests of the logic, so it's easy to eyeball that it's doing what we expect.

---

_@carljm reviewed on 2024-04-27 16:04_

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:66 on 2024-04-27 16:04_

Haha and I see you did exactly that! I should read the whole change before commenting, sorry. Let me look at the tests and see where my reading of the code is wrong, or the tests are.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:1003 on 2024-04-27 16:12_

This one isn't right, `from baz import bar` is not a relative import (no dots), it's an absolute import, so this should resolve to just `baz`.

Either `relative_import` should error on `level = 0` (and we should never call it that way), or it should just resolve it as an absolute import.

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:1024 on 2024-04-27 16:13_

this one is also wrong for the same reason mentioned above

---

_Review comment by @carljm on `crates/red_knot/src/module.rs`:66 on 2024-04-27 16:15_

Oh of course I'm wrong, 0..1 is not empty, the range is half-open, that has 0 in it. Sorry, clearly haven't woken up yet this morning.

Reviewing the tests found a different issue (level = 0 is not a relative import), but this logic here looks just fine.

---

_@carljm reviewed on 2024-04-27 16:15_

---
