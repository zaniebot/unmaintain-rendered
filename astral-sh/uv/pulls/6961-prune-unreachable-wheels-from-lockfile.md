```yaml
number: 6961
title: Prune unreachable wheels from lockfile
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/remove-dead-wheels
created_at: 2024-09-03T08:52:42Z
updated_at: 2024-09-04T13:09:41Z
url: https://github.com/astral-sh/uv/pull/6961
synced_at: 2026-01-12T16:07:36Z
```

# Prune unreachable wheels from lockfile

---

_@konstin_

When a package is included under a platform-specific marker, we know that wheels that mismatch this marker can never be installed, so we drop them from the lockfile.

---

_Label `enhancement` added by @konstin on 2024-09-03 08:52_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:290 on 2024-09-03 13:01_

Can we write this in a way such that we can have test coverage for the disjointness checks?

---

_@charliermarsh reviewed on 2024-09-03 13:01_

---

_@charliermarsh reviewed on 2024-09-03 13:01_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:271 on 2024-09-03 13:01_

How does this work? Is this the full markers along all paths to the node?

---

_@charliermarsh reviewed on 2024-09-03 13:02_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:271 on 2024-09-03 13:02_

Don't you need `propagate_markers` here?

---

_@konstin reviewed on 2024-09-04 09:00_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:271 on 2024-09-04 09:00_

> How does this work? Is this the full markers along all paths to the node?

Yes

> Don't you need propagate_markers here?

I didn't want to modify the graph and we need to consider the starting environments too, so it's separate.

---

_@konstin reviewed on 2024-09-04 09:03_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:290 on 2024-09-04 09:03_

https://github.com/astral-sh/packse/pull/217 should have test coverage for all cases, or did you mean something else?

---

_Merged by @konstin on 2024-09-04 09:08_

---

_Closed by @konstin on 2024-09-04 09:08_

---

_Branch deleted on 2024-09-04 09:08_

---

_@charliermarsh reviewed on 2024-09-04 12:49_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:271 on 2024-09-04 12:49_

I don’t understand how this is correct without using marker propagation though.

---

_@charliermarsh reviewed on 2024-09-04 12:52_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:290 on 2024-09-04 12:52_

This looks really unit-testable. Can we have some unit tests for it? I would rather not have to go through packse just to make updates to it in the future to fix bugs.

---

_@konstin reviewed on 2024-09-04 13:09_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:271 on 2024-09-04 13:09_

Wdym by marker propagation? Currently, `graph.reachability` is computed by `ResolutionGraph::reachability`, which determines for package the set of markers it can be reached by across the whole graph. It's similar to `propagate_markers` except that we include the starting fork environments and we don't modify the graph itself so we can still use the edge weights in `Lock::from_resolution_graph`.

I'll check if we can replace `propagate_markers` with using `reachability` or if `tool.uv.environments` causes problems with that.

---
