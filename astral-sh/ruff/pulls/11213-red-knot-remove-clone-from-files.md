```yaml
number: 11213
title: "[red-knot] Remove `Clone` from `Files`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: files-snapshot
created_at: 2024-04-30T14:14:09Z
updated_at: 2024-05-01T15:00:35Z
url: https://github.com/astral-sh/ruff/pull/11213
synced_at: 2026-01-12T15:55:37Z
```

# [red-knot] Remove `Clone` from `Files`

---

_@MichaReiser_

## Summary

This PR removes the `Clone` implementation on `Files` and replaces it with a `snapshot` function similar to `Database`. 

The main motivation for this change is that I think it's important that we prevent that `Files` (or any other `Database` state) can change while we're creating a persistent cache, because that could potentially lead to partial caches. 

What I have in mind is that we enforce a `&mut` reference when creating the cache (maybe not for the entire period, e.g. maybe only for creating the cacheable data structure, but we release the `&mut` before serializing the data structure to disk). 
Taking a `&mut` guarantees us that there's no other system (e.g. a file watcher) quietly mutating `Files` while we try to persist its state to disk. 

The guarantee is not as strong as in the `Database` case where the `Snapshot` type prohibits access to the wrapped `Database` because we need a `Files` instance to create a snapshoted `Program`. I hope that the fact that
the type doesn't implement `Clone` and a, in the future, usage of `Arc::make_mut().unwrap` should be protection enough ;)

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-04-30 14:19_

---

_Review requested from @carljm by @MichaReiser on 2024-04-30 14:19_

---

_Comment by @github-actions[bot] on 2024-04-30 14:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot/src/db.rs`:60 on 2024-05-01 00:57_

Why is this needed?

---

_Review comment by @carljm on `crates/red_knot/src/program/mod.rs`:201 on 2024-05-01 01:02_

Seems like a lot to dump all the file changes stuff straight into program/mod.rs. Submodule maybe?

---

_@carljm approved on 2024-05-01 01:02_

---

_@MichaReiser reviewed on 2024-05-01 07:10_

---

_Review comment by @MichaReiser on `crates/red_knot/src/db.rs`:60 on 2024-05-01 07:10_

Clippy's [`must_use`](https://rust-lang.github.io/rust-clippy/master/index.html#/must_use_candidate) rule was yelling at me ;) but I think it's actually right.

You can mark struct/enums and methods as `must_use`. The compiler will then emit a warning if you call a function that returns a *must use* object that you aren't handling. You've probably seen these warnings before when calling a function that returns a `Result` that you aren't handling. The compiler generates these warnings because `Result` is marked as `must_use`. 

I mark the `snapshot` function here as `must_use` because calling the function is useless if you aren't using the result. That means, it's most likely an error and you wanted to assign the result to a variable.

---

_@MichaReiser reviewed on 2024-05-01 07:11_

---

_Review comment by @MichaReiser on `crates/red_knot/src/program/mod.rs`:201 on 2024-05-01 07:11_

Yeah, I don't know how to structure this yet. Maybe I think the file is still reasonably small (Rust has different standards when it comes to large files compared to JS/python ;)). 

---

_Merged by @MichaReiser on 2024-05-01 07:11_

---

_Closed by @MichaReiser on 2024-05-01 07:11_

---

_Branch deleted on 2024-05-01 07:11_

---

_@carljm reviewed on 2024-05-01 15:00_

---

_Review comment by @carljm on `crates/red_knot/src/db.rs`:60 on 2024-05-01 15:00_

Ah, ok, that makes perfect sense. I understand what `#[must_use]` does, I was just trying to figure out if there was some correctness or safety invariant that would be violated by failure to use the return value of this. But even if not it makes sense that it would likely be a bug not to use it, and it's nice to flag that likely bug.

---
