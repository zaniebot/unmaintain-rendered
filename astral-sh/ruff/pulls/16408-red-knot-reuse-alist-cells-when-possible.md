```yaml
number: 16408
title: "[red-knot] Reuse alist cells when possible"
type: pull_request
state: closed
author: dcreager
labels:
  - internal
  - performance
  - ty
assignees: []
draft: true
base: main
head: dcreager/dont-have-a-cow
created_at: 2025-02-27T00:56:23Z
updated_at: 2025-02-28T19:58:23Z
url: https://github.com/astral-sh/ruff/pull/16408
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Reuse alist cells when possible

---

_Pull request opened by @dcreager on 2025-02-27 00:56_

This is in support of #16311, and adds some additional tracking to the alist implementation so that we can tell when a cell is only used once. This lets us reuse and modify those cells in place when doing certain updates.

This also adds some property tests for alists, since I wasn't convinced that my small set of hand-coded examples were exercising all of the code paths.

I'm leaving this as draft for now, but want to get some CI and Codspeed feedback on it.


---

_Label `internal` added by @dcreager on 2025-02-27 00:56_

---

_Label `performance` added by @dcreager on 2025-02-27 00:56_

---

_Label `red-knot` added by @dcreager on 2025-02-27 00:56_

---

_Review comment by @dcreager on `crates/ruff_index/src/list.rs`:81 on 2025-02-27 01:02_

This is a major part of this PR, and I'm not sure whether I like all of the ramifications.

The gist is that we want to track whether a particular list cell is aliased (used more than once).  To do that, we need to wrap the IndexVec IDs in this type that is _not_ `Clone` or `Copy`.  You must invoke the `clone_list` method below to make a copy of a list.  It's still cheap — copying a `u32` and setting a `bool` — but this lets us update the mutator methods below to take in owned `List` handles in cases where we want to try to reuse unaliased cells.

I like that this gives us type safety for tracking aliasing information that enables this optimization.  I don't love that it has knock-on effects through the rest of the code...

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:510 on 2025-02-27 01:03_

...like here, where `FlowSnapshot` is no longer `Clone`...

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:516 on 2025-02-27 01:03_

...at least not without threading extra state all of over the place!

---

_@dcreager reviewed on 2025-02-27 01:03_

---

_Comment by @github-actions[bot] on 2025-02-27 01:35_

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

_Comment by @dcreager on 2025-02-28 19:56_

I've tried a few variations of this, but the overhead of tracking when a cell is aliased is too high compared to the savings from being able to reuse cells, at least for the use case in #16311 

---

_Closed by @dcreager on 2025-02-28 19:56_

---

_Branch deleted on 2025-02-28 19:58_

---
