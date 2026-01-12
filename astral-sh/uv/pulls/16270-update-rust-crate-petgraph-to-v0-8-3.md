```yaml
number: 16270
title: Update Rust crate petgraph to v0.8.3
type: pull_request
state: merged
author: renovate
labels:
  - internal
assignees: []
merged: true
base: main
head: renovate/petgraph-0.x-lockfile
created_at: 2025-10-13T02:23:26Z
updated_at: 2025-10-13T02:42:50Z
url: https://github.com/astral-sh/uv/pull/16270
synced_at: 2026-01-12T16:12:12Z
```

# Update Rust crate petgraph to v0.8.3

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [petgraph](https://redirect.github.com/petgraph/petgraph) | workspace.dependencies | patch | `0.8.2` -> `0.8.3` |

---

### Release Notes

<details>
<summary>petgraph/petgraph (petgraph)</summary>

### [`v0.8.3`](https://redirect.github.com/petgraph/petgraph/blob/HEAD/CHANGELOG.md#083---2025-09-30)

[Compare Source](https://redirect.github.com/petgraph/petgraph/compare/petgraph@v0.8.2...petgraph@v0.8.3)

##### Bug Fixes

- Infinite `subgraph_isomorphisms_iter` for empty isomorphisms ([#&#8203;780](https://redirect.github.com/petgraph/petgraph/pull/780))
- Algos don't work on `UndirectedAdaptor` ([#&#8203;870](https://redirect.github.com/petgraph/petgraph/pull/870)) ([#&#8203;871](https://redirect.github.com/petgraph/petgraph/pull/871))
- use a queue for SPFA  ([#&#8203;893](https://redirect.github.com/petgraph/petgraph/pull/893))
- `StableGraph::reverse` breaks free lists ([#&#8203;890](https://redirect.github.com/petgraph/petgraph/pull/890))

##### Documentation

- Fix examples link in README and unify typesetting of one word ([#&#8203;823](https://redirect.github.com/petgraph/petgraph/pull/823))
- Add link to multigraph definition to isomorphism algos ([#&#8203;824](https://redirect.github.com/petgraph/petgraph/pull/824))
- Fix auxiliary space (and time) complexity of bron-kerbosch ([#&#8203;825](https://redirect.github.com/petgraph/petgraph/pull/825))
- Fix Typo in Operator Module Documentation ([#&#8203;831](https://redirect.github.com/petgraph/petgraph/pull/831))
- Sync the crate feature flags in the README and docs ([#&#8203;832](https://redirect.github.com/petgraph/petgraph/pull/832))
- Remove all \[Generic] tags from algo docstrings ([#&#8203;835](https://redirect.github.com/petgraph/petgraph/pull/835))
- Fix typos in comments ([#&#8203;836](https://redirect.github.com/petgraph/petgraph/pull/836))
- Revamp CONTRIBUTING.md ([#&#8203;833](https://redirect.github.com/petgraph/petgraph/pull/833))
- Update `GraphMap` link in README ([#&#8203;857](https://redirect.github.com/petgraph/petgraph/pull/857))
- Add doc comment for `Dot::with_attr_getters` ([#&#8203;850](https://redirect.github.com/petgraph/petgraph/pull/850))
- Specify iteration order for neighbors and edges and their variants ([#&#8203;790](https://redirect.github.com/petgraph/petgraph/pull/790))
- Collection of Doc fixes ([#&#8203;856](https://redirect.github.com/petgraph/petgraph/pull/856))

##### New Features

- Add `into_nodes_edges_iters` to `StableGraph` ([#&#8203;841](https://redirect.github.com/petgraph/petgraph/pull/841))
- Add methods to reserve & shrink `StableGraph` capacity ([#&#8203;846](https://redirect.github.com/petgraph/petgraph/pull/846))
- Add Dinic's Maximum Flow Algorithm ([#&#8203;739](https://redirect.github.com/petgraph/petgraph/pull/739))
- make Csr::from\_sorted\_edges generic over edge type and properly increase edge\_count in Csr::from\_sorted\_edges ([#&#8203;861](https://redirect.github.com/petgraph/petgraph/pull/861))
- Add `map_owned` and `filter_map_owned` for `Graph` and `StableGraph` ([#&#8203;863](https://redirect.github.com/petgraph/petgraph/pull/863))
- Add dijkstra::with\_dynamic\_goal ([#&#8203;855](https://redirect.github.com/petgraph/petgraph/pull/855))
- Fix self-loop bug in all\_simple\_paths and enable multiple targets ([#&#8203;865](https://redirect.github.com/petgraph/petgraph/pull/865))
- mark petgraph::dot::Dot::graph\_fmt as public ([#&#8203;866](https://redirect.github.com/petgraph/petgraph/pull/866))
- Add bidirectional Dijkstra algorithm ([#&#8203;782](https://redirect.github.com/petgraph/petgraph/pull/782))

##### Performance

- Make A\* tie break on lower h-values ([#&#8203;882](https://redirect.github.com/petgraph/petgraph/pull/882))

##### Refactor

- add examples for scc algorithms and reorganize into dedicated module ([#&#8203;830](https://redirect.github.com/petgraph/petgraph/pull/830))
- Remove unnecessary trait bounds from impls/methods ([#&#8203;828](https://redirect.github.com/petgraph/petgraph/pull/828))
- replace uses of 'crate::util::zip' with 'core::iter::zip' ([#&#8203;849](https://redirect.github.com/petgraph/petgraph/pull/849))
- Fix clippy (and other) lints ([#&#8203;851](https://redirect.github.com/petgraph/petgraph/pull/851))
- Cleanup repo ([#&#8203;854](https://redirect.github.com/petgraph/petgraph/pull/854))
- replace crate::util::enumerate with Iterator::enumerate ([#&#8203;881](https://redirect.github.com/petgraph/petgraph/pull/881))

##### Testing

- Add dependency list for 'quickcheck' feature ([#&#8203;822](https://redirect.github.com/petgraph/petgraph/pull/822))
- Fix feature cfg capitalization in doctest ([#&#8203;852](https://redirect.github.com/petgraph/petgraph/pull/852))

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
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0MS4xNDMuMSIsInVwZGF0ZWRJblZlciI6IjQxLjE0My4xIiwidGFyZ2V0QnJhbmNoIjoibWFpbiIsImxhYmVscyI6WyJpbnRlcm5hbCJdfQ==-->


---

_Label `internal` added by @renovate[bot] on 2025-10-13 02:23_

---

_Merged by @charliermarsh on 2025-10-13 02:42_

---

_Closed by @charliermarsh on 2025-10-13 02:42_

---

_Branch deleted on 2025-10-13 02:42_

---
