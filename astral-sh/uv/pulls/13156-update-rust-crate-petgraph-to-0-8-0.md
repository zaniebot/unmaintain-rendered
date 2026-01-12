```yaml
number: 13156
title: Update Rust crate petgraph to 0.8.0
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/petgraph-0.x
created_at: 2025-04-28T01:32:41Z
updated_at: 2025-04-28T02:25:49Z
url: https://github.com/astral-sh/uv/pull/13156
synced_at: 2026-01-12T16:10:34Z
```

# Update Rust crate petgraph to 0.8.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [petgraph](https://redirect.github.com/petgraph/petgraph) | workspace.dependencies | minor | `0.7.1` -> `0.8.0` |

---

### Release Notes

<details>
<summary>petgraph/petgraph (petgraph)</summary>

### [`v0.8.1`](https://redirect.github.com/petgraph/petgraph/blob/HEAD/CHANGELOG.md#081---2025-04-07)

[Compare Source](https://redirect.github.com/petgraph/petgraph/compare/petgraph@v0.8.0...petgraph@v0.8.1)

This patch release re-adds a missing `VisitMap` implementation that was dropped in the `0.8.0` release,
improves error messaging in panicking functions, and adds capacity management methods to `UnionFind`.

##### Bug Fixes

-   Bring back `VisitMap` impl for std `HashSet` ([#&#8203;764](https://redirect.github.com/petgraph/petgraph/pull/764))

##### New Features

-   Add `UnionFind` capacity management methods ([#&#8203;736](https://redirect.github.com/petgraph/petgraph/pull/736))
-   add `#[track_caller]` to functions that panic  ([#&#8203;748](https://redirect.github.com/petgraph/petgraph/pull/748))

### [`v0.8.0`](https://redirect.github.com/petgraph/petgraph/blob/HEAD/CHANGELOG.md#080---2025-04-05)

[Compare Source](https://redirect.github.com/petgraph/petgraph/compare/petgraph@v0.7.1...petgraph@v0.8.0)

##### Breaking changes

-   Add `no_std` Support ([#&#8203;747](https://redirect.github.com/petgraph/petgraph/issues/747))
-   Add `VisitMap::unvisit` as proposed in [#&#8203;610](https://redirect.github.com/petgraph/petgraph/issues/610) ([#&#8203;611](https://redirect.github.com/petgraph/petgraph/issues/611))
-   Add support for specifying rankdir on dot plots. ([#&#8203;728](https://redirect.github.com/petgraph/petgraph/issues/728))
-   Make `dot::Config` non_exhaustive ([#&#8203;756](https://redirect.github.com/petgraph/petgraph/issues/756))
-   Add `from_f32/64` methods for `Float`, `Unit`, and `Bounded` measures ([#&#8203;733](https://redirect.github.com/petgraph/petgraph/issues/733))

##### New algorithms

-   Add articulation points implementation ([#&#8203;681](https://redirect.github.com/petgraph/petgraph/issues/681))
-   Add Prim's Algorithm for Minimum Spanning Tree ([#&#8203;625](https://redirect.github.com/petgraph/petgraph/issues/625))
-   Add Kou's algorithm for finding a MST ([#&#8203;682](https://redirect.github.com/petgraph/petgraph/issues/682))
-   Add Bron-Kerbosch algorithm for maximal cliques ([#&#8203;662](https://redirect.github.com/petgraph/petgraph/issues/662))
-   Add Shortest Path Faster Algorithm Implementation ([#&#8203;686](https://redirect.github.com/petgraph/petgraph/issues/686))

##### New features

-   Add `UnionFind::new_set` ([#&#8203;684](https://redirect.github.com/petgraph/petgraph/issues/684))
-   Implement `Csr::try_add_edge` ([#&#8203;719](https://redirect.github.com/petgraph/petgraph/issues/719))
-   Add checked `UnionFind` methods ([#&#8203;730](https://redirect.github.com/petgraph/petgraph/issues/730))
-   Add `MatrixGraph` methods with recoverable errors ([#&#8203;720](https://redirect.github.com/petgraph/petgraph/issues/720))
-   Add methods with recoverable errors for `Graph` and `StableGraph` ([#&#8203;718](https://redirect.github.com/petgraph/petgraph/issues/718))

##### CI & fixes

-   Fix all clippy lints and check them on CI ([#&#8203;726](https://redirect.github.com/petgraph/petgraph/issues/726))
-   Pin once_cell version for MSRV builds ([#&#8203;750](https://redirect.github.com/petgraph/petgraph/issues/750))
-   Require conventional commits tag in PR titles ([#&#8203;734](https://redirect.github.com/petgraph/petgraph/issues/734))
-   Fix wrong trigger for pr-title check ([#&#8203;751](https://redirect.github.com/petgraph/petgraph/issues/751))
-   Solve clippy warnings ([#&#8203;749](https://redirect.github.com/petgraph/petgraph/issues/749))
-   Fix github token in pr-title action ([#&#8203;752](https://redirect.github.com/petgraph/petgraph/issues/752))
-   Add new triggers for semver-checks ([#&#8203;754](https://redirect.github.com/petgraph/petgraph/issues/754))

##### Documentation

-   Add some missed features into crate-lvl doc ([#&#8203;758](https://redirect.github.com/petgraph/petgraph/issues/758))

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "before 4am on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzOS4yNTcuMyIsInVwZGF0ZWRJblZlciI6IjM5LjI1Ny4zIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-04-28 01:32_

---

_Merged by @charliermarsh on 2025-04-28 02:25_

---

_Closed by @charliermarsh on 2025-04-28 02:25_

---

_Branch deleted on 2025-04-28 02:25_

---
