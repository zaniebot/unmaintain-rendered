```yaml
number: 11177
title: "Fix `cargo build --release`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: release-me
created_at: 2024-04-27T15:20:18Z
updated_at: 2024-04-27T15:47:15Z
url: https://github.com/astral-sh/ruff/pull/11177
synced_at: 2026-01-10T22:37:02Z
```

# Fix `cargo build --release`

---

_Pull request opened by @AlexWaygood on 2024-04-27 15:20_

## Summary

`cargo build --release` currently fails to compile on `main`:

<details>

```
error[E0599]: no method named `hit` found for struct `ReleaseStatistics` in the current scope
   --> crates/red_knot/src/cache.rs:22:29
    |
22  |             self.statistics.hit();
    |                             ^^^ method not found in `ReleaseStatistics`
...
145 | pub struct ReleaseStatistics;
    | ---------------------------- method `hit` not found for this struct

error[E0599]: no method named `miss` found for struct `ReleaseStatistics` in the current scope
   --> crates/red_knot/src/cache.rs:25:29
    |
25  |             self.statistics.miss();
    |                             ^^^^ method not found in `ReleaseStatistics`
...
145 | pub struct ReleaseStatistics;
    | ---------------------------- method `miss` not found for this struct

error[E0599]: no method named `hit` found for struct `ReleaseStatistics` in the current scope
   --> crates/red_knot/src/cache.rs:36:33
    |
36  |                 self.statistics.hit();
    |                                 ^^^ method not found in `ReleaseStatistics`
...
145 | pub struct ReleaseStatistics;
    | ---------------------------- method `hit` not found for this struct

error[E0599]: no method named `miss` found for struct `ReleaseStatistics` in the current scope
   --> crates/red_knot/src/cache.rs:41:33
    |
41  |                 self.statistics.miss();
    |                                 ^^^^ method not found in `ReleaseStatistics`
...
145 | pub struct ReleaseStatistics;
    | ---------------------------- method `miss` not found for this struct
```

</details>

This is because in a release build, `CacheStatistics` is a type alias for `ReleaseStatistics`, and `ReleaseStatistics` doesn't have `hit()` or `miss()` methods. (In a debug build, `CacheStatistics` is a type alias for `DebugStatistics`, which _does_ have those methods.)

Possibly we could make this less likely to happen in the future by making both structs implement a common trait instead of using type aliases that vary depending on whether it's a debug build or not? For now, though, this PR just brings the two structs in sync w.r.t. the methods they expose.

## Test Plan

`cargo build --release` now once again compiles for me locally


---

_Label `internal` added by @AlexWaygood on 2024-04-27 15:20_

---

_Review requested from @carljm by @AlexWaygood on 2024-04-27 15:20_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-04-27 15:20_

---

_Comment by @github-actions[bot] on 2024-04-27 15:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-27 15:45_

---

_Merged by @charliermarsh on 2024-04-27 15:45_

---

_Closed by @charliermarsh on 2024-04-27 15:45_

---

_Comment by @charliermarsh on 2024-04-27 15:45_

Seems fine for now, merging to get builds passing. Thanks!

---

_Branch deleted on 2024-04-27 15:47_

---
