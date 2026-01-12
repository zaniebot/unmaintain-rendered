```yaml
number: 12948
title: "Fix re-entrance deadlock in Package::files"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: package-files-reentrence-deadlock
created_at: 2024-08-17T12:42:00Z
updated_at: 2024-08-20T07:00:25Z
url: https://github.com/astral-sh/ruff/pull/12948
synced_at: 2026-01-12T15:55:42Z
```

# Fix re-entrance deadlock in Package::files

---

_@MichaReiser_

## Summary

This PR fixes a bug in the lazy `packages.files` logic where red knot dead-locked because:

* `check` iterated over all files in the package, holding on to the `files` lock
* `infer` tried to determine if the current file is open by calling `package.contains_file` which calls `files` internally. This deadlocked because rust's mutex doesn't allow re-entrance. 


The plus of this is that the new logic is much simpler, and probably faster.
It also entirely encapsulates the mutex (it no longer returns a lock guard) so that re-entrance bugs should be history. 

## Test Plan

Added a new test, verified that it dead locks on main. 


---

_Review requested from @carljm by @MichaReiser on 2024-08-17 12:42_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-08-17 12:42_

---

_Label `red-knot` added by @MichaReiser on 2024-08-17 12:42_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/files.rs`:120 on 2024-08-17 12:45_

That was an incorrect assumption of mine. Salsa doesn't compare input-fields for equality. Setting an input field is always considered to be a change.

---

_@MichaReiser reviewed on 2024-08-17 12:52_

---

_Comment by @github-actions[bot] on 2024-08-17 12:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-08-17 13:27_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace.rs`:304 on 2024-08-17 13:27_

I'm not sure why I added caching for files when I implemented this. We only do the indexing once and then cache the result. There's no need to cache it again

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/workspace/files.rs`:35 on 2024-08-19 17:28_

It feels kinda awkward how mnay `Arc<FxHashSet<File>>` annotations there are everywhere now. Would it be worth creating a newtype wrapper `struct IndexedFilesInner(Arc<FxHashSet<File>>);` that dereferences to `FxHashSet<File>`? Or at least create a type alias somewhere?

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/src/workspace/files.rs`:77 on 2024-08-19 17:30_

```suggestion
        //   can't outlive the database (constrained by the `db` lifetime).
```

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/Cargo.toml`:18 on 2024-08-19 17:36_

Oh, I think this is no longer necessary after https://github.com/astral-sh/ruff/pull/12985

---

_@AlexWaygood reviewed on 2024-08-19 17:37_

Carl would probably be a better reviewer here as I'm not too familiar with this code, but this seems reasonable

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/src/workspace/files.rs`:35 on 2024-08-20 06:38_

A newtype wrapper seems unnecessary complicated. I don't want to hide the type. I can do a type alias, although 4 usages don't seem that many to me and naming now kind of becomes awkward

---

_@MichaReiser reviewed on 2024-08-20 06:38_

---

_Comment by @MichaReiser on 2024-08-20 06:45_

> Carl would probably be a better reviewer here as I'm not too familiar with this code, but this seems reasonable

Thanks for reviewing. 

I feel fairly confident about the change and it's also a local change with very limited impact if we need to do any follow up "cleanup"

---

_Merged by @MichaReiser on 2024-08-20 06:51_

---

_Closed by @MichaReiser on 2024-08-20 06:51_

---

_Branch deleted on 2024-08-20 06:51_

---

_Comment by @codspeed-hq[bot] on 2024-08-20 06:52_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/package-files-reentrence-deadlock)

### Merging #12948 will **degrade performances by 4.82%**

<sub>Comparing <code>package-files-reentrence-deadlock</code> (f8b4dfd) with <code>main</code> (abb4cdb)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/package-files-reentrence-deadlock)._

### Benchmarks breakdown

|     | Benchmark | `main` | `package-files-reentrence-deadlock` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 727.4 µs | 764.3 µs | -4.82% |


---
